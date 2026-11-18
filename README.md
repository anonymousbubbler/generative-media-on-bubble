# Generative media on Bubble

Use an AI assistant and Bubble MCP to add image, video or audio generation to an existing Bubble app. Start with a feature your users need: a shop owner uploads a photo and gets three listing images, a teacher saves narration to a lesson, or a designer turns two frames into a product video. The guides show the assistant how to fit that feature into the app's existing pages, permissions and records.

**Try the saved demos.** Watch a cinematic shot in Shiggsfield, compare product photos in OpenCanvas, or play two narrations in Soundloom. These Bubble pages use saved files and cost visitors no generation credits. [Open the gallery](https://fal-plugin-test.bubbleapps.io/version-test/studio_gallery) · [See the 15-second video recipe](examples/flux-pullback/README.md)

## Start with this prompt

> Read this repository's SKILL.md. I want [feature] on [page or existing record] in my Bubble development app. Inspect how the app already handles users, projects and files. Show me which records and workflows you would reuse. Recommend a small set of compatible models and choose a default. Prototype one result with me in ChatGPT within my agreed budget, then wire the accepted recipe into the app. Save the result on the right record and show me it still works after reload.

Download the repository from **Code → Download ZIP** or clone it. Give your assistant the folder, or attach the relevant Markdown guides and ask it to read the root skill. Connect Bubble MCP to an account with editor access. Connect fal separately in ChatGPT if you want to prototype there. The running Bubble app uses its own server-side key. [Skill](SKILL.md) · [Bubble setup and first integration](docs/bubble-integration.md) · [fal key setup](docs/fal-setup.md)

**Install the free web plugin.** The Wise Bubbler published **Generative Media with fal AI v1.0.0** under MIT on November 18, 2026. Sign in to Bubble, install the plugin in your app, then enter your own fal key in its private settings. An agent using Bubble MCP can install it by ID `1789043147994x526977149156458500`. fal bills your account for generation. [Install on Bubble](https://bubble.io/plugin/generative-media-with-fal-ai-1789043147994x526977149156458500) · [Set up the key](docs/fal-setup.md)

## What should the agent read?

| You want to… | Start here |
| --- | --- |
| Create a fal key, put it in the correct environment, understand who pays | [fal and credentials](docs/fal-setup.md) |
| Connect Bubble MCP, inspect an app, and build one feature end to end | [Bubble integration](docs/bubble-integration.md) |
| Offer a few models with a sensible default, or browse a full catalog | [Model choices and defaults](docs/model-selection.md) |
| Decide when to create a job, update a record, keep variants or retry a failed save | [Copyable agent prompts](docs/agent-prompts.md) |
| Look up plugin actions, model inputs, result types, file limits and privacy | [Plugin and storage reference](docs/plugin-and-storage.md) |
| Understand how to map an approved result into real Bubble controls | [FLUX example](examples/flux-pullback/README.md) |

**Where do the files live?** A file uploaded through Bubble's uploader starts in that app's file storage. A newly generated file starts on fal. To retain it, the workflow copies the file into Bubble and saves the returned Bubble file reference on the owning record. You can then find it under **Data → File manager** in the matching Development or Live environment. A database field can also hold a remote URL, so seeing an image in a field alone doesn't tell you who hosts it. [Storage guide](docs/plugin-and-storage.md#where-the-bytes-live)

By **The Wise Bubbler**. This repository contains instructions and public example settings. Each target app needs its own workflow and privacy checks, and fal charges the account that submits a live generation.
