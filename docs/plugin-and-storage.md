# How the Bubble plugin runs a generation and saves its files

Use this reference to wire **Generate**, **Check result**, and **Save** in a Bubble web app. The bring-your-own-key (BYOK) connector sends an authorized request to fal, retrieves the finished result, and gives your workflow an opportunity to copy each file into storage owned by the app. Those are three separate operations. A failed copy can be retried on the original generation.

## What runs where

| Component | Responsibility |
| --- | --- |
| ChatGPT and the building skill | Test a sample with the creator, record the approved recipe, then use Bubble MCP to wire the target app. A ChatGPT connection to fal belongs to that prototyping session. |
| Bubble page | Collect inputs, show job progress, display saved results and optionally copy public files in the foreground. The browser receives only the fields the app exposes. |
| Bubble server-side plugin action | Use the app owner's private `fal_api_key` with the official JavaScript SDK. An authorized button workflow can invoke this action. The credential stays on the server. |
| fal | Run the requested endpoint, track the queue request and return the model output. fal bills the account associated with the key. |
| Your Bubble app | Decide who may submit, which models and limits they can use, who owns a job and which outputs to retain. Database fields record the job and file URLs. |

This guide covers the authored **0.3.0 BYOK contract**, built with `@fal-ai/client@1.10.1`. Check your installation for these action and field names. This repository supplies the integration guide and contract. Installing the Bubble connector is a separate step. A marketplace release or access to the full plugin source requires its own confirmed listing or distribution terms. [fal key authentication](https://fal.ai/docs/documentation/model-apis/authentication/key-based)

## From button click to saved job

1. Enter the app owner's fal key in the installed plugin's private setting for the intended development or live environment. Run **Check SDK setup**. Its `key_configured` and `sdk_version` outputs confirm configuration and SDK loading. A successful fal request confirms that the key has the necessary permissions.
2. Use a fixed approved endpoint for a single-purpose app. For a shortlist, populate the picker from a Bubble option set and select its compatible default. For a full catalog, populate model records in the database as described in [Model choices and defaults](model-selection.md). Use **Search models** to discover endpoints by query or category. It returns `endpoint_ids`, `display_names`, `model_urls`, `next_cursor` and `has_more`. Request `include_schema=yes` with one exact `endpoint_id` when a model is selected.
3. Bind the chosen endpoint and its matching schema to **Model Settings**, or build native Bubble controls for the specific recipe. The element publishes `is_valid`, `validation_errors` and `input_json`. **Validate and generate** validates the controls and emits **Generate requested**. The app workflow invokes the server action.
4. Check the signed-in user's access, model policy and input limits on the server. Create a `Generation` record with owner, endpoint, input snapshot and submission status. Prevent duplicate submissions for the same intended generation, including clicks from a second tab.
5. Call **Run AI model** once with the validated `input_json`. A nonempty dynamic `endpoint_id` overrides the action's model dropdown. Save the returned `request_id`, `endpoint_id` and `status` immediately on that record.
6. **Check result** invokes **Refresh AI job** using the stored endpoint and request IDs. Check ownership again when reading or cancelling. Set the results UI from the returned fields, then use the storage steps below for media the user wants to keep.

The plugin applies a 256 KiB input-JSON limit, a 2 MiB queue-response limit and a 12-second network-operation budget. These govern the API exchange. A longer video job continues on fal after submission returns. A manual refresh button costs no idle polling. The optional Model Settings watcher starts at four seconds and backs off to thirty seconds, stopping after twenty requests or five minutes. [fal queue API](https://fal.ai/docs/documentation/model-apis/inference/queue)

### Map the recipe's inputs

Use the selected endpoint's actual OpenAPI schema and the settings that worked in the prototype. Text-to-video may accept only a prompt. Image-to-video may require a start image, distinguish it from an end image, or accept a separate reference-image array. Use the exact field names, order, types and supported combinations for that endpoint. Likewise, confirm that the chosen model supports the requested duration, dimensions, aspect ratio and audio together. [fal model directory](https://fal.ai/models)

For example, the original **Generate image** action is a preset for `fal-ai/flux/schnell`. It takes a prompt, width, height, optional seed and one to four images. Width and height default to 1024 × 768 and each must be 256–1440, divisible by eight. **Edit Bubble image** uses `fal-ai/flux-kontext/dev` with an edit instruction and one public image URL. These bounds belong to the legacy preset. The generic action follows the selected model's schema. **Get job status** and **Get generated images** support the original image workflows.

For a product-photo app, a **Run AI model** action targeting `fal-ai/flux/schnell` could submit this input. Save the returned request ID on the product's generation record, then display **First image** from a successful refresh.

```json
{
  "prompt": "A cobalt ceramic coffee mug on a pale oak table, soft side light, plain background",
  "image_size": {"width": 1024, "height": 768},
  "num_images": 1
}
```

A Bubble Picture Uploader or File Uploader returns a file URL. Model Settings can use an existing uploader through `media_url` and a matching JSON pointer such as `/image_url` or `/reference_images/0`. Its own media control uses Bubble's native upload facility when uploads are enabled. A Bubble URL starting with `//` is normalized to HTTPS by the media control. fal must be able to fetch the file when the job runs. Configure an authorized transfer separately for private inputs.

## Interpret the queue response

| What you receive | What the app should do |
| --- | --- |
| Submit returns `in_queue`, `in_progress` or `completed` with a request ID | Save the ID. Use **Refresh AI job** to obtain the result even when submission already says completed. |
| Refresh returns `in_queue` or `in_progress` | Keep the saved job pending. Honor `retry_after_seconds` when supplied. |
| `succeeded` and `result_ready=yes` | Store the result metadata. Copy the files the user needs. |
| `result_pending` | Keep the same request. If `queue_status=completed` and retrieval is retryable, use **Read result only**. If completion is unconfirmed, use normal refresh. A nonretryable parsing error needs inspection of the model's output shape. |
| `failed` | Inspect `error_code`, `error_message`, `http_status` and `retryable`. A temporary read failure can be retried on the same ID. A terminal model failure belongs on the original job record. |
| `unknown_submission` or `submission_uncertain=yes` | The submission may already be running and billable. Preserve any ID and reconcile with the owner's fal request history before allowing a replacement purchase. |
| **Request cancellation** | Send the original two IDs. `cancellation_requested` acknowledges the request. For `already_completed`, retrieve the result. Investigate `not_found`. Cancellation and billing settlement are separate. |

Refresh exposes `image` and `images` as Bubble image values, `video` and `audio` as file values, their lists `videos` and `audios`, and `files` for all recognized media. `media_json` records the kind, source path, URL, and available metadata such as dimensions, duration, MIME type and byte size. `text` handles supported text outputs. `output_json` preserves the raw result for model-specific shapes. Use the requested list for batch results. A text-only result can succeed with zero files. For video, verify actual playback duration and dimensions before promising a specific delivery format.

## Where the bytes live

| Saved value | Where the media bytes live | Where to find it |
| --- | --- | --- |
| Fresh result from fal | At the provider's returned URL, subject to its retention settings | The completed job's typed result and source URL |
| Provider URL written to a Bubble image or file field | Still at the provider | The Bubble record contains a link |
| File successfully copied to Bubble | In the app's Bubble file storage | **Data → File manager** in the correct development/live environment |
| Copied file attached in your workflow | In Bubble file storage, with the Bubble-hosted URL on the owning record | The image/file field on `Generation` or a related asset record |

Bubble image and file fields store URL references. Clearing the field leaves the uploaded file in storage. If your app supports deletion, remove the uploaded file and clear the record reference as part of that operation. Keep the original provider URL in a separate field when you need to reconcile a copy. [Bubble file storage and privacy](https://manual.bubble.io/help-guides/data/files) · [fal media retention](https://fal.ai/docs/documentation/model-apis/media-expiration)

### Copy a small public image

In a Bubble workflow that creates or changes an owned thing, set its image field to **Result of Refresh AI job's First image:saved to Bubble Storage**. Check that the result is ready, the image exists and that record still needs its copy. The original image demo used this native operator, called `saved_s3` in Bubble's serialized expression. The operator supports files up to **50 MB**. Saving a raw provider URL skips the copy. [Bubble file and image operators](https://manual.bubble.io/core-resources/data/operations-and-comparisons)

### Copy a public video, audio file or larger image

1. Enable **File uploads** and configure **Max file size** on the Model Settings web element. After successful retrieval, call its **Save generated file** element action with the owning Bubble job ID, selected provider source URL and an appropriate filename. Use the video/audio URL that corresponds to the selected output.
2. Keep the page open. The element downloads the bytes in the browser, creates a `File`, checks `instance.canUploadFile`, and calls `instance.uploadFile`. The source must allow a direct HTTPS browser fetch with CORS and no credentials or redirect. The action's download guard defaults to **128 MiB**, configurable from **1–256 MiB**. The element upload limit and available browser memory can lower the practical ceiling.
3. Handle **Generated file saved**. Match **Saved job ID** (`save_job_id`) and **Original source URL** (`saved_source_url`) to the owned job and asset. Write **Saved Bubble file** (`saved_file`) to its file/image field. Display that saved field on later visits. A multi-file result needs one matched save per file.
4. On **Generated file save failed**, preserve the provider URL and completed request. Inspect `save_error` and `save_uncertain`. A callback timeout may still leave a real upload in Bubble. Reconcile the upload before retrying, especially after reloading the page. Retrying the copy uses the original result and incurs no second inference request.

This foreground method was exercised in the development demo for an image, video and audio file, including a page reload against the same Bubble URLs. A separate browser-transfer test copied a 55 MiB public MP4 and played the stored file. The generic copy does not require a custom `uploadContent` server action. Background transfer while the tab is closed needs its own implementation. The separate narrated website-intro workflow had provider-hosted final MP4s in its recorded installed test. Its automatic final-export copy was prepared separately.

**Inspect business website** and **Advance business intro video** are specialized actions for that narrated flow. The first takes a public site and brief. The second advances a saved project through storyboard choice, narration, composition and output verification. Save each returned project checkpoint before moving on. A resumed project should recover the existing provider job; replaying a fresh submission could incur another charge.

### Private files

The plugin's foreground copy creates a public upload. Restricting who can read the database record hides its URL through that record. A person who already knows the public URL can still fetch the bytes. For private uploads, configure Bubble's private-file setting, attach the upload to an existing owned thing, and grant appropriate users **View attached files**. Verify direct file access with an unauthorized account. The generic BYOK save action has no attachment parameter. For private generation inputs and private outputs, provide a separately tested authorized transfer route. [Bubble private files](https://manual.bubble.io/help-guides/data/files)

## Where the managed connector differs

BYOK submits with the app owner's fal key and stores the fal request ID. The managed connector authenticates to a service that holds the provider key, meters the subscribing app's usage, and returns a customer-scoped managed request ID. Its Bubble workflows use the managed actions and an idempotency key derived from the owning Bubble generation. Use the same connector that submitted a job to retrieve it.

Both routes require the app to choose its durable file destination and record the stored URL. The managed service's usage ledger tracks spend. It does not by itself copy media into a customer's Bubble File Manager or make the files private.
