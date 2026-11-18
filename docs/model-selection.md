# Choose models for a Bubble feature

**Start with a few compatible models in a Bubble option set and preselect one.** Most users want an image, clip or voiceover and should be able to press Generate immediately. Put the picker under **Change model** if model choice is secondary. If builders specifically want the entire catalog, use a Bubble database type with search and pagination. Bubble downloads option sets as part of the app, so a catalog-sized option set slows every page and needs a deployment to change. [Bubble option sets](https://manual.bubble.io/help-guides/data/static-data/option-sets)

## A few choices in an option set

1. Inspect the feature first. A cover generator needs text-to-image endpoints. A start-and-end-frame video feature needs endpoints that accept both named frames. “Produces video” by itself doesn't establish input compatibility.
2. In **Data → Option sets**, create or reuse an option set such as **Image Model**. Use **Display** for the human label and add **endpoint_id** (text). Add **sort_order** (number) and **mode** (text or another option set) only if the app needs them. Set up each option's inputs from its verified model schema. Keep keys and other private values out of option sets.
3. In the dropdown use **Dynamic choices**, **Type of choices = Image Model**, **Choices source = All Image Models**, and **Option caption = Current option's Display**. Set **Default value** to the specific compatible option, for example **Get an option → Image Model → Quick draft**. For a photo app with separate modes, set one compatible default per mode. Restore a valid saved selection when editing an existing job. Use the feature's default on a new one.
4. Example options for a simple text-to-image feature are **Quick draft** → `fal-ai/flux/schnell` (default) and **Detailed draft** → `fal-ai/flux/dev`. Verify both endpoints against the current catalog and actual inputs before offering them. Look up model-specific controls on selection. Clear settings left over from a different model, such as unsupported image counts or duration, and preserve shared values such as the prompt where compatible. [Quick draft model](https://fal.ai/models/fal-ai/flux/schnell/api) · [Detailed draft model](https://fal.ai/models/fal-ai/flux/dev/api)
5. On Generate, the server resolves the permitted option and validates the parameters. Store its exact endpoint ID and effective input on the new job. Refresh reads the job's stored values even if the visitor changes the dropdown or the default changes later.

## All models or a frequently changing catalog

Use a **Media Model** data type. One record represents one full provider endpoint path. For example, image-to-video and text-to-video variants of the same family get separate rows. Reuse an equivalent existing data type.

| Field | Bubble type | Why keep it |
| --- | --- | --- |
| endpoint_id | text | Stable external key for matching catalog rows and submitting jobs. With multiple providers, match on provider plus endpoint. |
| display_name, category | text | Searchable label and initial filter for the requested media operation. |
| catalog_status, model_page | text | Provider availability and documentation used to inspect the current schema. |
| enabled_for_app | yes/no | App owner's decision that a model is ready to offer. A catalog listing alone establishes discoverability. |
| default_model on an existing feature/settings record | Media Model | Exactly one compatible default for that feature. Changing it applies to new jobs. |

**Preload from the catalog.** Ask the agent to run an owner-only import in the intended app environment if authorized. Use the plugin's **Search models** action or fal's public model catalog. Request an initial active-model page, map its rows, then follow each `next_cursor` while `has_more` is true. Importing metadata does not run models. Keep full input schemas out of the main listing and fetch the selected endpoint's schema when needed. [fal catalog API](https://fal.ai/docs/platform-apis/v1/models)

**Match and refresh.** Look up each row by exact endpoint ID. Create genuinely new records and update existing metadata in place. Keep app-owned defaults, labels/overrides and enablement separate from fields overwritten by a sync. Serialize import runs, checkpoint completed pages and replay a page safely after failure. Bubble's text field is not a uniqueness constraint. If duplicate rows are found, stop and reconcile references before merging them. Handle rate limits using the returned retry delay. Mark vanished endpoints for review only after a full successful scan. Keep old job snapshots and references intact.

**If using Bubble's CSV tools instead:** import a validated mapping into an empty table once. For subsequent bulk changes, Bubble's **Modify** operation expects its own unique IDs. Match existing rows by endpoint first, carry the Bubble IDs into the update set, and put genuinely new endpoints in a separate create set. Development and Live have separate records/IDs. CSV import availability depends on the Bubble plan. [Bubble Data tab](https://manual.bubble.io/core-resources/bubbles-interface/data-tab)

**Display and submit.** Use a server-constrained search or search box for the operation, enabled status and user-entered search. Load a page of results at a time. On submission, look up the allowed model again on the server and apply the app's price and input limits. If the saved default becomes unavailable, ask the owner for a replacement or use their explicit fallback. Show that change before generation. Opening the picker, refreshing the catalog and changing the selected model should never buy a generation.

## Prompt to give the agent

> Inspect the model choices already in this app. For my [feature], offer a small compatible option set with one default so Generate works immediately. Tell me which input fields differ between models. If I explicitly ask for the entire catalog, use a searchable Media Model database type, preload it in Development, match records by endpoint and preserve my enablement/defaults on refresh. Store the endpoint and settings actually used on each job. Demonstrate that switching the picker cannot change which job gets refreshed.
