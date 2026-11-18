# Add generation to an existing Bubble web app

This walkthrough adds a **Create cover image** button to a signed-in user's project. The user types a book description, generates one square cover, and can reopen the same result tomorrow. To see what a finished media page looks like first, play the saved 15-second film and expand its exact recipe. Playback uses no fal credits. [Open the Bubble demo](https://fal-plugin-test.bubbleapps.io/version-test/video_studio)

**Connector availability, checked November 18, 2026.** The bring-your-own-key connector is called **fal AI Reliable Queue**. Bubble's install check returned no published version. The workflow below can run in a web app with access to the testing plugin, or after a public release passes installation checks. Today anyone can connect the Bubble MCP, inspect their app and build the interface with a saved sample. **fal AI Managed** is a separate service with different actions and billing.

## Get your bearings

Open your app from your Bubble dashboard. **Design** contains pages and their visible elements, such as inputs, buttons and images. **Workflow** connects an event such as a button click to a sequence of actions. **Data** holds persistent records, which Bubble calls *Things*. A custom state such as the selected job belongs to a page element and disappears on reload. Put the provider request ID and result on a Thing. Development and Live have separate databases. [Data tab](https://manual.bubble.io/core-resources/bubbles-interface/data-tab) · [Version control](https://manual.bubble.io/help-guides/maintaining-an-application/version-control)

## Connect the assistant to Bubble

In **ChatGPT → Plugins**, find **Bubble**, select **Install**, and sign in to Bubble as the app owner or an invited collaborator with editor access. With **Codex CLI**, run the two commands below and complete Bubble's browser sign-in. The connector uses the signed-in account's permissions and the app's plan. Review the requested data access. Structural app discovery and a logged-out preview are enough to start. [Official Bubble MCP setup and permissions](https://manual.bubble.io/help-guides/ai/bubble-mcp)

```sh
codex mcp add bubble --url https://mcp.bubble.io/mcp
codex mcp login bubble
```

Give the assistant your editor URL and the development branch name. Start with this message.

> Inspect this Bubble web app on Development. Find the existing project page, how a signed-in user owns a project, and where its cover image is displayed. Check which fal plugin and version are available. Show me what you'll reuse and where you'd put Generate, Check progress and the saved cover. Use a saved sample until I approve a paid run.

The assistant can verify the account and app with `whoami`, `list_apps` and `get_versions`, then read the relevant page, workflows and types with `get_app_outline` and `get_node`. If your app is missing, verify the signed-in account and collaborator access. On an existing app, request a savepoint or record the specific components being changed before editing.

## Check the runtime connector

In the target app, ask the assistant to look for **fal AI Reliable Queue**, plugin ID `1789043147994x526977149156458500`. Use `get_plugin_install_check` to confirm a version can be installed. If you already have the test version, inspect the actions available in your editor. For a published version, install it in Development and inspect its actual action names and input/output types. A directory search result, an editor link or an installed ChatGPT plugin alone cannot run a generation workflow.

In Bubble's **Plugins** tab, enter your own fal key in the BYOK plugin's **private development key** field. Run **Check SDK setup** and confirm the SDK loaded and a key is configured. That check reads configuration. A separately authorized generation tests whether the key works with the model. Keep the key in the server-side plugin setting. Never paste it into an assistant conversation, page field, custom state or repository.

## Build the book-cover workflow

1. **Keep one record per generation.** Reuse the app's existing job type if it has the right fields. Otherwise create an `AI Job` Thing with `owner` (User), `project` (the existing project type), `prompt`, `endpoint`, `request ID`, `status`, `provider URL`, `error` (text), and `saved file` (file). In **Data → Privacy**, grant the job owner access to their own fields and jobs. For a team app, use the project's existing membership policy. Apply the same ownership check in every workflow that submits, refreshes or saves a job. [Bubble privacy rules](https://manual.bubble.io/help-guides/data/the-database/protecting-data-with-privacy-rules)
2. **Use native controls.** Add a multiline book-description input, **Create cover**, **Check progress**, an Image element and a short status text to the project page. Use one selected-job state if needed. Bind button availability, status text and the image to the job. On page load, find the most recent job for this project and authorized user. For a sample-only page, bind an approved saved image and omit spending actions entirely.
3. **Submit once.** Create the owned job and bind the page to it before making the paid call. The trusted workflow checks sign-in, project access, an allowed endpoint, input length and an available spending allowance if your app offers one. Fix the endpoint and count server-side. Call **Run AI model** with endpoint `fal-ai/flux/schnell` and the JSON below, substituting the validated input with JSON-safe encoding. Save returned `endpoint_id`, `request_id` and `status` on that job. Disable duplicate clicks while submitting. For a shared paid app, enforce duplicate-request protection on the server as well. [Model input and output schema](https://fal.ai/models/fal-ai/flux/schnell/api)

   ```json
   {
     "prompt": "A vintage botanical illustration of a lemon tree on a navy book cover, square composition, no lettering",
     "image_size": "square",
     "num_images": 1
   }
   ```

4. **Check the same request.** **Check progress** calls the server action **Refresh AI job** with the job's saved endpoint and request ID. Show a pending message while the job is queued or in progress. When `result_ready` is true, bind the typed `image` result and save the first image URL for this single-image recipe. For `result_pending`, use `result_only=yes` only when `queue_status=completed` and `retryable=yes`. If completion is unconfirmed, use normal Refresh on the same job. Stop on a nonretryable parsing error and inspect the output shape. Respect `retry_after_seconds` when provided. A page timer is unnecessary.
5. **Persist the file.** A text URL in a Thing points to the provider's file. To keep a Bubble-hosted copy of this *public* cover with the testing plugin, use the Model Settings element's **Save generated file** action while the page is open. On **Generated file saved**, match `save_job_id` and `saved_source_url` to the selected job before writing `saved_file` on its owning record. Update the project cover only if the user selects it or the project still awaits that same job. After a reload, show the saved file first. For confidential media, validate a private delivery and storage route separately. A public URL can still be shared, even if the Thing is private.

**Where the code runs.** The page click can invoke a server-side plugin action. Bubble runs the private fal call on its server. For jobs that must continue when the browser closes, use internal backend workflows on a plan that supports them and persist progress between steps. Re-read the owned job server-side. Keep any external workflow endpoint authenticated and grant only the needed operations. The plugin's foreground file-copy action requires the page to stay open. [Frontend and backend workflows](https://manual.bubble.io/help-guides/logic/the-frontend-and-backend)

## Test and release

- **Saved sample.** Open the development preview while signed out and as two separate test users. Verify that the intended sample plays, one user cannot see or modify the other's job, and loading a project or refreshing a tab makes no billable request.
- **One authorized paid test.** Run the book-cover case with the owner's stated budget, double-click Create, then refresh and reopen the project. Verify one provider request, one saved image on the correct project and the same file after reload. Review the runtime result and Bubble's fresh issue check.
- **Failures.** Handle a missing key, invalid prompt, known provider failure, and interrupted result/file retrieval. If submit times out and no ID is returned, mark submission uncertain and reconcile provider history. A blind retry can charge twice. If result retrieval fails with a known ID, retry that read. If a file upload is uncertain, reconcile the job and storage before copying again.
- **Live.** Review Development first. Configure the private Live fal key and Live permissions in the editor, then deploy the app from Bubble. Check the deployed page with a saved sample. The development database's example records do not automatically become live records. This walkthrough targets web. A native mobile interface and native file handling need their own verification. [Bubble deployment and versions](https://manual.bubble.io/help-guides/maintaining-an-application/version-control)
