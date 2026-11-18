# Prompts for adding generation to an existing Bubble app

Replace the bracketed words with your app's terms. Give the assistant your Bubble **editor URL**, the intended environment and the feature you want. A storefront's Product, a teacher's Lesson and a studio's Project can each own media. The assistant should find that existing relationship before adding new types.

## First ask the agent to map the app

> Read SKILL.md and inspect the Development version of [editor URL] with Bubble MCP. On [page], [role] should turn [input] into [output] and find it again on [existing record]. Show me which page, reusable element, data types, file fields, owner/team relationships, privacy rules and workflows already fit. Ask me only what you need to decide where the result belongs, who can view it and whether to keep previous versions. Tell me which records will be created versus updated on a click, a refresh and a save.

For a **single cover per book**, one stored image field on the Book plus an owned generation job can be enough. For **three variants per click**, use a related Media Asset per chosen output. For **a multi-step narrated video**, keep a parent Project/Recipe Run and a job per provider submission with explicit input and output relationships. Refreshing one job or reopening the editor never creates a new creative version.

## Decide when to write a record

| Moment | What the agent should write |
| --- | --- |
| Open form, type a prompt, switch models | Keep draft controls in the page unless the product explicitly offers saved drafts. Reuse the current owner/project relationship. |
| Click Generate | Authorize the owner/team, resolve a permitted model on the server and validate required inputs. Create one owned job for this intended request with the endpoint and effective settings. Persist the returned provider ID on that same job. |
| A response is lost or slow | Preserve the pending job and any known request ID. Record the ambiguity or last read error. Reconcile before a paid retry. |
| Click Refresh or return after reload | Load the authorized saved job by its Bubble ID and fetch using its stored endpoint and provider ID. Update only that job. A change of dropdown or selected project must not redirect the result. |
| fal returns three images | Write the result metadata once. If keeping all three, create or reuse three asset records linked to this job. Match a stable output position/path within that job (for example `images[1]`) on subsequent refreshes. Two jobs may use the same prompt or even yield the same source URL. |
| A file copy starts | Record copying/pending on the asset. Keep the source URL while transferring. Generate success and storage success have separate states. |
| Bubble confirms an upload | Match its job and source to the pending asset. Save the returned Bubble file/image value and mark that asset stored. Change a parent pointer such as `Book's selected_cover` only on explicit selection or if this is still the parent's intended pending job. An older upload finishing late must not replace a newer selection. A failed second copy must not clear a previously saved cover. |
| User clicks Regenerate or Use this version | Regenerate starts another paid job after authorization. Use this version updates the parent record's selected asset. Keep or remove earlier versions according to the product's retention rules. |

**Practical fields.** A job usually needs its owner or tenant, parent record, endpoint snapshot, input/settings snapshot or references, provider request ID, status and errors. A Media Asset usually needs its source job, stable output role/index, media kind, source URL, stored file and storage state. Use a direct owner/tenant field on each record when Bubble privacy rules or searches require it. Reuse built-in IDs and creation timestamps. For private apps, limit sensitive prompt/input data to what the product actually needs to retain.

## Three prompts for products

**Product photography**

> Add a “Create product images” button on our existing Product editor. Use the product photo already uploaded to Bubble and the verified image-edit endpoint. Let the merchant choose a background preset. Pick a compatible default. Keep three generated variants as Media Assets owned by that merchant and linked to the Product. Choosing “Use as hero” should change the Product's hero reference only after that variant has a stored Bubble image. Show that a second refresh leaves the variant count unchanged.

**Storyboard to video**

> Add “Animate this storyboard” to our existing Storyboard. Use its selected opening and ending frames in the correct model roles. Offer a short list of video models that support both frames, with one ready by default. Store the chosen duration and endpoint on a video job. Let the user come back later to check the same request. Copy the finished MP4 into Bubble and attach it to this storyboard's version. Show where the file and the original provider link are stored. Use the FLUX example as a reference for wiring.

**Lesson narration**

> Add a narration action to the Lesson editor. Let the teacher review their text and preview a voice selection before submitting. Save one audio job linked to the lesson and the language. If the teacher changes voice, create a new version. Keep the old recording playable until they select a successful replacement. Only authorized teachers can generate. Check whether student audio must be private before choosing the transfer method.

## Prompts for common implementation decisions

**Choose models and defaults**

> For [feature], use an option set for the two or three models we actually want to offer. Preselect one compatible default. If I later request all models, propose a database catalog keyed by provider and endpoint ID, a paginated metadata import and an enablement policy. Keep my past job records working even when a model is retired. [Model guide](model-selection.md)

**Persist actual media**

> When a model returns a URL, show me the provider URL field and the separate stored Bubble file field. For public assets use a supported file-copy method and wait for its success callback. Save to the owned job or output record and bind the gallery to the stored field. Use a test where two jobs finish out of order. Reload the page and show that the correct image, audio or video is still on each record. [File limits, callbacks and privacy](plugin-and-storage.md#where-the-bytes-live)

**Handle a lost connection and duplicate clicks**

> Test a double click, two tabs, a refresh during submission, a completed job whose result read times out, and a file copy that finishes after the user selects another job. Make a status read or storage retry reuse the existing job. If the plugin cannot guarantee exactly-once submission across tabs, explain the remaining race and propose a server-side reservation/serialization design before enabling spend for a shared app.

**Private files and tenancy**

> Our customers own confidential reference images. Trace exactly who can fetch the source while fal is queued, who can fetch the output by direct URL, and what the plugin's public foreground saver supports. Keep file visibility and database visibility separate. Use an authorized private transfer with a sufficient lifetime, or flag that specific dependency and leave the public saving path disabled for private work. Test with two users in different tenants.

**Ship a playback-only example**

> Embed our already approved file, two reference frames and the exact prompt in this demo. Let visitors play, pause and seek. Disable submission and background polling. Verify the player reads the saved Bubble URL in the target environment and uses no generation credits. Show me the preview link.

## Ask for evidence on handoff

Ask for the Development preview, exact endpoint and inputs, affected Bubble types and workflows, and one example of a job linked to its saved output. Verify two independent facts: the media loads after a reload, and the file belongs to the intended storage host/environment. For a private asset, also test direct access from a second unauthorized session. Record separately which checks used fixtures, existing saved examples or a fresh paid request. [Lifecycle and verification](../references/workflow.md)
