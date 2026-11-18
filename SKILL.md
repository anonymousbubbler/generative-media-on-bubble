---
name: generative-media-on-bubble
description: Prototype fal media features in ChatGPT, capture a user-accepted recipe, then wire it into an existing Bubble app with the Wise Bubbler BYOK plugin and Bubble MCP. Also build read-only showcases from saved samples.
---

# Generative media on Bubble

Prove the requested media experience with the user, then build it inside their app. Preserve its design, authentication, tenants, data relationships and unrelated functionality. This free starter adds no managed subscription, credits or end-user billing.

Use the focused guides when needed: [fal setup](docs/fal-setup.md), [choosing models](docs/model-selection.md), [ready-to-use agent prompts](docs/agent-prompts.md), [plugin and storage](docs/plugin-and-storage.md), and [Bubble integration](docs/bubble-integration.md).

## Prototype in ChatGPT, then build in Bubble

Default to one representative end-to-end prototype through the connected fal tools in ChatGPT. Discover the actual tools and current model contract. This skill supplies instructions, not a ChatGPT connector, hosted runtime or provider credentials. Prototype requests use the connected fal account. The app later uses its owner's separately configured private fal key; never copy connector credentials into Bubble or request them in chat.

Before paid tests, establish the user's existing authorization, budget and allowed iteration scope. Generate the smallest representative complete result. Inspect the actual image, video or audio with the user, and revise the specific failing part within that scope until the user accepts it or the budget/authorization ends. A successful job, an unreviewed attachment or silence is not acceptance. When the user requires a native continuous take, verify one actual model output meets the duration and continuity requirement; do not substitute stitched clips, loops, freeze extensions, time expansion or animated stills. Report remaining gaps and ask for the next decision rather than running unlimited repairs.

Capture the accepted result using [the validated recipe record](references/validated-recipe.md): exact endpoints, prompts, complete effective settings, reference files and roles, ordered steps, actual outputs, observed constraints, cost evidence and the user's acceptance. Reuse an already approved recipe; do not spend credits restarting its creative review. Honor an explicit request to wire first or use another order. Complete useful app inspection or fixture wiring while access or approval is missing, without labeling an untested recipe validated.

Then implement that recipe through the app's BYOK plugin and Bubble MCP. Before the Bubble test, compare its outgoing payload and step order against the accepted record. Fix accidental differences in endpoints, prompts, defaults, file roles, duration, resolution or audio; never silently substitute a different model. Deliberate changes need revalidation only for affected steps. A fresh paid integration test still needs budget coverage; accepting a creative result does not authorize unlimited test runs.

## Choose the requested mode

**Static showcase:** reuse accepted saved media with its prompt and actual settings. Do not regenerate it just to build the page. Visitors may play, pause and seek. Add no submit, polling or generation workflows; no new key or job database is needed just to display a sample. Keep illustrative controls read-only and label them clearly. Verify playback, download links and the absence of provider calls.

**Live generation:** let authorized users submit new jobs through the installed plugin. Follow the ownership, limits, recovery and storage steps below. A static sample verifies playback, not a fresh plugin generation. Do not turn a static demo into a live spending feature unless requested.

## Inspect the app

Read [the MCP workflow](references/bubble-mcp.md), discover connected tools and read their required guides. Verify the app and development version, relevant pages, reusables, plugins, data types and privacy rules. Read [the plugin contract](references/plugin.md) only for needed actions. Do not inspect secret values or unrelated customer records.

Use the app to resolve routine choices. Ask at most three consequential questions when needed: where the feature belongs, what media/model is wanted and who can access results. Reuse suitable components and their real IDs; never copy the author's app IDs or assume action prefixes. Preserve a savepoint or scoped before-state and a concise installation record.

## Implement one complete path

For live generation, follow [the lifecycle and storage guide](references/workflow.md). Reuse or create a job with direct ownership, saved input/model, provider request ID, status and media relationship. A job tracks one request and its lifecycle; linked media records track the retained result or results. Reuse the app's existing creation/project records and relationships where suitable, and link rather than duplicate them. One job may produce multiple media files. Keep transient UI state out of permanent records. Keep fal keys in private server-side plugin settings. Derive refresh/cancel targets from authorized saved jobs; enforce access plus model/parameter limits on the server. Disabled page controls are not a security boundary.

Load the endpoint's current schema and confirm cost/limits before mapping controls. Match the accepted recipe's effective settings. Camera/style directions may be prompt text, not parameters. For a shortlist, use a Bubble option set with one compatible default per feature. When the user wants the full catalog, use model records in the database and follow the [catalog and default instructions](docs/model-selection.md). Reveal advanced controls only when useful. A catalog listing is discovery, not execution proof.

Submit once and save the returned request ID promptly. Default to a manual Refresh button with no polling or recurring workflows. Preserve uncertain submissions; never resubmit merely because a response was lost. Retry result reads and file copies against the same known job. Distinguish a provider URL from retained file bytes and a saved database relationship.

If a key, plugin or tool capability is missing, name the dependent stage and complete useful permitted work. Do not replace missing MCP capabilities with hidden editor calls, another customer's IDs or weaker privacy. Honor existing budgets and publishing authorization; configuration does not itself authorize paid inference.

## Verify and deliver

Read back definitions, run a fresh issue check and inspect the preview. Check recipe-to-payload parity before submission and compare the actual result with the accepted example afterward. For live generation, test validation, idle behavior, manual refresh, failed/uncertain submissions, storage, reload/resume and relevant access boundaries. Distinguish fixtures from paid tests.

Deliver preview/editor URLs, created or reused components, accepted recipe, installation record and remaining limits. A saved URL is not a retained file; a draft is not a live deployment; a guide is not proof of universal compatibility.
