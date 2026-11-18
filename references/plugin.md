# BYOK plugin contract

The connector runs server actions through `@fal-ai/client`. The current local contract snapshot is **0.3.0**, with SDK **1.10.1**. It is a reference for this plugin, not a promise about whatever version a customer has installed.

Read [plugin-contract.json](plugin-contract.json) for stable action/field IDs, typed returns and the core Model Settings element. The file is a compact subset of the authored contract. The large model dropdown snapshot and business-intro pipeline are deliberately excluded. Runtime MCP action IDs and property names must be resolved from the installed app; these contract IDs are not MCP payloads.

| Action | Purpose | Key mapping |
| --- | --- | --- |
| Check SDK setup (`ADR`) | Check SDK loading and presence of a key without revealing it | `ok`, `sdk_version`, `key_configured`; this does not prove the key can generate |
| Search models (`ADZ`) | Discover models or load one endpoint's schema | `endpoint_id`, `include_schema` → `schema_json`; search only when needed |
| Run AI model (`AEV`) | Submit one compatible queued request | `endpoint_id`, `input_json`, optional `schema_json` → `request_id`, `status` |
| Refresh AI job (`AFN`) | Check/retrieve the same request | Saved `endpoint_id`, `request_id`; `result_only` for retrieval → typed media/text and status |
| Request cancellation (`ADB`) | Request cancellation of the same job | Saved `endpoint_id`, `request_id`; display the actual response |

An explicit nonempty `endpoint_id` overrides the plugin's static model dropdown. The schema/input must match that endpoint. Do not change the endpoint of an existing provider job during refresh.

## Inputs and controls

- Prefer a focused native form when the app needs one model; use the installed Model Settings element when its dynamic schema UI is useful and verified.
- Discover model fields from current [fal model documentation](https://fal.ai/models). Do not assume all text inputs are named `prompt`, all media fields accept lists, or all videos support the same duration/audio controls.
- Match the requested combination, not each setting in isolation: image input, 30-second duration, aspect ratio and generated sound must be supported together. If no selected endpoint supports it, choose a compatible endpoint or explain a proposed multi-job composition before changing the scope or cost. Do not silently shorten, loop or stretch the output and call it a 30-second generation.
- Validate JSON and required fields before submission. Treat a browser-supplied schema as a UX validation aid, not the app's server-side model policy.
- Use HTTPS URLs for Bubble-uploaded media that the provider is authorized to fetch. Confirm URL accessibility for the queue's expected timing. Private inputs need a separately validated authorized transfer route.
- fal keys belong in private plugin configuration (`fal_api_key`), with development/live values managed by the app owner. No key belongs in a page, custom state, database field, public API call or shared repository.

For Model Settings, bind `endpoint_id` and `schema_json` to the same chosen model. Its `submit` event uses `input_json` only when `is_valid` is true. Restore a previous job with `initial_input_json`; never bind that input to the element's own `input_json` output, which can overwrite edits. Discover the current native property types through MCP before wiring the element.

The element also offers a foreground **Save generated file** action for public media. Authorize the job/output, set upload limits and keep the page open. On completion, match `save_job_id` and `saved_source_url` to the intended asset before saving `saved_file`; the user may have selected a different job meanwhile. Persist `save_uncertain` and reconcile before retrying after a timeout or reload. This route is not private-file or background-transfer support.

## Outputs and scope

The generic refresh action returns images, video/audio/files, a media manifest, text and output JSON. Bind the actual type returned by the selected endpoint. A successful text-only result is not a failed media job. A model may return multiple files; store the outputs the user requested rather than silently taking only the first one.

This starter covers compatible queue-model workflows. Streaming, realtime sessions, training and unsupported native mobile UI require separate integrations. The standalone website-to-storyboard/export pipeline is outside this small starter. Free BYOK supplies no credit ledger, end-user wallet or guarantee that all catalog endpoints run unchanged.
