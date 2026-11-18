# One reusable generation flow

Use the app's existing Project, Job and Asset equivalents where they fit. Do not add billing, a gallery, teams or new pages unless the requested feature needs them.

## Data and access

A job needs direct owner/tenant, endpoint, input snapshot, provider request ID, lifecycle status, error, and output references. Keep an approved reference-image relationship when the next step depends on it. A media record may hold the original provider URL, stored Bubble file, kind, owner, source job and storage status. Reuse fields; do not create duplicates of Bubble's built-in unique ID or timestamps.

Apply default-deny privacy and a specific owner/tenant rule appropriate to the app. Enforce submit, refresh, cancel and write access on the server as well as in the UI. Do not trust a request ID supplied independently of an owned job. Protect your app owner's fal account with a server-controlled model allowlist, parameter limits and any requested per-user allowance; the BYOK plugin itself is not an end-user credit system.

## Submit → refresh → store

| Stage | What to do |
| --- | --- |
| Ready | Validate required inputs, exact endpoint schema, permissions and configured limits. Show the selected settings. |
| Submitting | Persist the job and immutable input, then invoke Run AI model once. Disable duplicate clicks. A page state alone does not prevent competing browser tabs. |
| Queued/running | Save the returned endpoint/request ID immediately. Show one manual Refresh action; it reads the saved job. |
| Unknown submission | Preserve the job/input and uncertainty. Reconcile with the existing provider request if possible. Do not automatically generate again. |
| Result pending | Provider completion and result retrieval are separate. Retry retrieval for the same ID; do not mark the generation failed solely because downloading the response timed out. |
| Status/read error | Preserve the request ID and last known generation state. Record the read error separately and offer a manual read retry when appropriate. A failed plugin read is not proof that generation failed; use the response's error details and confirmed provider outcome. |
| Generation succeeded | Save typed output metadata and distinguish it from durable storage. Show the available result. |
| Storage pending/failed | Retry copying the existing result. Never regenerate merely because storage failed. |
| Stored | Mark stored only after the file copy and database relationship succeed. Reload and verify playback from the stored file. |
| Cancel requested | Report the acknowledgement accurately. Cancellation does not guarantee stoppage or a refund. |

Persist pending work so reload/resume uses the same job. Do not blindly replay a submission after an ambiguous response. The generic BYOK action does not promise exactly-once billing or atomic cross-tab locking; add a verified serialization mechanism if the requested app requires it.

## Storage

Saving a fal URL in a text/file field does not copy its bytes into Bubble storage. Provider links may expire. For a suitable public file, use Bubble's supported copy/upload route, then save the resulting Bubble file on the owned record. Recheck current size limits before implementation; the native `saved to Bubble Storage` operator has historically been limited to 50 MB. Larger files and private/background transfers require a validated alternative.

Keep “generated”, “copied” and “saved to record” separate. Repeated completed refreshes should reuse the verified stored file. A private database record does not make a publicly accessible file URL private. For private media, validate attachment/file privacy and retrieval as an unauthorized user too.

## Small acceptance pass

Before a paid request, confirm:

- Existing authorization covers the selected model, number of jobs and estimated cost; record repair attempts against the same budget.
- The fal key is confined to private server-side plugin settings and absent from public assets, logs and shared files.
- The authenticated user can access the job and source files; server-enforced model and parameter limits apply independently of page controls.

Then exercise the feature:

- Initial page load and idle time make no generation or recurring status calls.
- Required fields and unsupported model settings fail before submission.
- One explicit generation creates one known job; refresh uses that same request.
- Lost/uncertain submission and result-pending fixtures do not resubmit.
- Failed storage retries the existing output, and reload resumes it.
- For personal jobs, a second user cannot read, cancel or overwrite them. For team jobs, test each permitted teammate role, cross-team denial and revoked membership against the app's actual policy.
- The chosen image/video/audio actually renders; stored media still works after reload.
- Narrow-screen form and player remain usable; Bubble's fresh issue check is clean for the changed scope.

Run paid inference only under a defined existing budget or new user authorization. Record the exact endpoint, settings, plugin version and evidence. A metadata lookup or saved sample does not verify fresh generation.

Current references: [fal queue API](https://fal.ai/docs/documentation/model-apis/inference/queue), [fal media retention](https://fal.ai/docs/documentation/model-apis/media-expiration), [Bubble files](https://manual.bubble.io/help-guides/data/files), [Bubble file operators](https://manual.bubble.io/core-resources/data/operations-and-comparisons#file-type).
