# Set up fal in a Bubble app

fal hosts generative models behind APIs. A model endpoint describes one particular operation, such as generating an image from text or turning two reference frames into a video. Your workflow sends the endpoint's inputs, saves the request ID, checks the queue, then retrieves the output. The cost, input fields, supported durations and shape of the result depend on the endpoint. [fal model directory](https://fal.ai/models) · [Queue API](https://fal.ai/docs/documentation/model-apis/inference/queue)

## Create the app owner's key

1. Sign in to fal and select the personal account or team that should pay for this app's generations. Check its billing/credits before running a test.
2. Open **Dashboard → API Keys**, create a key, name it for the app and environment, and choose the **API** preset for model inference. Keep the complete secret value, including its ID portion when supplied. Store it in your password manager. [Create a fal key](https://fal.ai/dashboard/keys) · [fal authentication guide](https://fal.ai/docs/documentation/model-apis/authentication/key-based)
3. Open **Plugins → Installed plugins → Generative Media with fal AI** in the Bubble editor. Paste the raw key into **fal_api_key - dev.** for Development. The SDK adds the authorization scheme. If both published and testing installations appear, configure the one your workflows use.
4. Preview the development app and run **Check SDK setup**. Look for a loaded SDK and a configured key. This action checks the setup locally; a successful budgeted generation is the check that the fal account actually accepts requests. Before deploying, set the separate **fal_api_key** value for Live. A separate key per environment lets you revoke one independently.
5. If the plugin is missing, sign in to Bubble and install it in the intended app. The signed-out listing shows the plugin details and a login link. An assistant using Bubble MCP can check the exact plugin ID `1789043147994x526977149156458500` and published version `1.0.0`. [Install on Bubble](https://bubble.io/plugin/generative-media-with-fal-ai-1789043147994x526977149156458500)

The app owner enters this secret directly in Bubble. Your end user can click **Generate** on a page because the button invokes a server-side action. Put the ownership, allowable models and spending checks in a trusted workflow. The browser, option sets, job records, prompt text, logs and GitHub should never receive the key. People using your app normally do not need a fal account. Their calls use the account configured by the app owner.

## Two places to test

| Where you generate | Credential used | What the test proves |
| --- | --- | --- |
| ChatGPT with the fal connection enabled | The fal account connected to that session | The chosen prompt, references and settings can produce the desired media. |
| A connected Bubble app | The private plugin key for that Bubble environment | That app can submit a job, retrieve its result and put it on the right owner's record. |

Bubble MCP is the assistant's editor connection. The deployed button uses the saved Bubble workflow and plugin. App visitors do not need a connection to ChatGPT or Bubble MCP. The static demo only plays a saved file.

## First test

Pick one supported endpoint, one output and a small agreed budget. Read its model page and schema. Use its exact endpoint ID, send a short prompt, then save the plugin's returned request ID. Click **Check progress** on that saved job. When it succeeds, verify the result type. An image should open, a video should play and an audio clip should be audible. Copy the chosen output to Bubble storage, reload, and confirm the saved reference is on the expected record. [Model selection](model-selection.md) · [Storage and result types](plugin-and-storage.md)

If **Check SDK setup** reports no key, check the selected plugin and environment. If fal rejects authentication, verify the team, the full key, and its permissions in the dashboard. For insufficient credit, check billing on the owning account. For invalid input, compare the payload with the selected model's schema. For a slow job, refresh the existing request. If a key is exposed, revoke it in fal, replace the corresponding private Bubble setting and examine that account's usage. [fal dashboard](https://fal.ai/dashboard) · [Queue and recovery](../references/workflow.md)
