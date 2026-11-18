# Carry an accepted ChatGPT prototype into Bubble

Use this record after the user reviews and accepts an actual output. Save it in the customer's project, such as `.fal-bubble/recipe.json`, without secrets or unrelated customer data. It prevents app settings from drifting away from the tested recipe.

| Record | Include |
| --- | --- |
| Goal and acceptance | Requested experience; actual output the user accepted, when, and any accepted limitations. Keep status `unaccepted` until that decision exists. |
| Ordered pipeline | Each exact endpoint, complete effective input JSON, exact prompt, upstream dependencies, output field/path and assembly operation. Include schema date/source and defaults that affected the test. |
| Reference files | Retained file or authorized URL, source, hash when available, and role: identity, first frame, last frame or soundtrack. Record crops/transforms; an artwork crop is not an extracted video frame. |
| Inspection | Retained accepted output; measured dimensions/duration/audio and relevant visual findings. Separate requested settings from observed results. Technical success is not creative approval. |
| Cost and limits | Pre-run estimate, actual charge when available, attempts, remaining authorized budget and constraints encountered. Mark unavailable billing evidence unknown; an estimate is not a charge. |
| Bubble mapping | Installed plugin version; actual action/field mappings; dynamic user inputs versus fixed recipe values; job ownership, storage and manual refresh. |

Include each billed request and edit needed for the accepted result. Approval of a reference still is not approval of an unreviewed motion clip. For a native one-take requirement, record the single generation and observed continuous duration; a 30-second file made from edits does not pass that check. Do not restart an accepted prototype merely because app wiring begins.

## Acceptance check before the app test

1. Compare the final serialized Bubble request and step order with the accepted record: endpoint, prompt, effective defaults, reference roles/order, duration, resolution and audio must match.
2. Exempt only fields intentionally exposed as user choices; enforce their allowed values on the server. A copied file can have a new URL, but verify identical content and the same role. Do not substitute an unrelated file when a reference is unavailable.
3. Exercise fixtures or saved outputs first. If a fresh paid integration test is covered by the budget, submit once and compare the actual output with the accepted example. Verify manual refresh, retained file, reload/resume and user access.
4. Record whether the integration preserved the recipe and worked end-to-end. If a schema or intentional setting changes, explain the difference and revalidate only the affected stage within authorization.

ChatGPT's connected fal account and the Bubble owner's private fal key are separate and can have different balances or entitlements. Keep the app key in private server-side plugin settings. Neither credential belongs in this record, a prompt, page state, database field or public archive.
