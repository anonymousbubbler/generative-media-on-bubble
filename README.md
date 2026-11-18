# Generative media on Bubble

Use an AI assistant and Bubble MCP to add image, video or audio generation to an existing Bubble app. Start with a feature your users need: a shop owner uploads a photo and gets three listing images, a teacher saves narration to a lesson, or a designer turns two frames into a product video. The guides show the assistant how to fit that feature into the app's existing pages, permissions and records.

**Try the finished video.** The demo plays a saved 15-second FLUX 3 Max shot, shows both reference frames, and includes the exact generation prompt. Playing it uses no generation credits. [Open the Bubble demo](https://fal-plugin-test.bubbleapps.io/version-test/video_studio) · [Read the worked example](examples/flux-pullback/README.md)

## Start with this prompt

> Read this repository's SKILL.md. I want [feature] on [page or existing record] in my Bubble development app. Inspect how the app already handles users, projects and files. Show me which records and workflows you would reuse. Recommend a small set of compatible models and choose a default. Prototype one result with me in ChatGPT within my agreed budget, then wire the accepted recipe into the app. Save the result on the right record and show me it still works after reload.

Download the repository from **Code → Download ZIP** or clone it. Give your assistant the folder, or attach the relevant Markdown guides and ask it to read the root skill. Connect Bubble MCP to an account with editor access. Connect fal separately in ChatGPT if you want to prototype there. The running Bubble app uses its own server-side key. [Skill](SKILL.md) · [Bubble setup and first integration](docs/bubble-integration.md) · [fal key setup](docs/fal-setup.md)

**Plugin availability.** The Wise Bubbler bring-your-own-key plugin, **fal AI Reliable Queue**, is a testing draft as of November 18, 2026. It runs in the demo app. A public installable version could not be resolved in Bubble's marketplace. If you have testing access, use that installation. Other builders can use the skill, model guidance and playback example immediately; the plugin-specific generation steps require an accessible plugin version. Installing this repository gives the assistant instructions. Installing the connector in a Bubble app is a separate step.

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
