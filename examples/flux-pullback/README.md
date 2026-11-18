# A 15-second cinematic pullback

Watch [the saved Bubble demo](https://fal-plugin-test.bubbleapps.io/version-test/video_studio) or [the finished video](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794996575690x415569682279433300/Last-One-Standing-FLUX-15s.mp4). Viewing these files creates no generation request and spends no fal credits. The demo is a development preview, not evidence of a fresh plugin installation.

This example uses one [FLUX 3 Max image-to-video request](https://fal.ai/models/blackforestlabs/flux-3-max/image-to-video/api). It starts close to a fictional actress and pulls back to five fallen company signboards. It contains a blood-streaked prop knife and stage blood; no attack is shown. The scene is fictional. It uses real company names on the props; they do not imply endorsement.

## What to reuse

- [Exact submitted prompt, inputs and QA record](../../references/flux-15-second-inputs.json).
- [Opening reference](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794947935704x147251021077931420/Last-One-Standing-Poster.jpg): `image_url`.
- [Ending reference](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794940082026x226337063916927970/fallen-brands-pool.jpg): `end_image_url`.
- [Original provider video](https://v3b.fal.media/files/b/0ab2f241/-z1FDsJ0Ht8_VOyXQuH4e_alligator.mp4) and [four-frame timeline](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794996960001x348953365241079400/Last-One-Standing-FLUX-Timeline.jpg).

Requested settings were numeric `duration: 15`, `resolution: "720p"`, `aspect_ratio: "16:9"`, `reasoning_level: "high"` and `generate_audio: true`. The original request did not set a seed; `1052570381` is the observed output seed. Submit only the JSON's `input` object, not its QA metadata.

Both videos play at normal speed. The provider returned 361 frames at 24 fps, approximately 15.04 seconds, at **1280 × 704**. The finished sample removes the first video frame and raises the native mono soundtrack by 10 dB: 360 frames and **15.00 seconds**. There is no speed change, reverse playback, stitched shot or still animation. See the [full example record](../../references/flux-15-second-example.md) for observed limitations. The camera accelerates near 10 seconds; all five names are correct in approximately the last five seconds. Technical QA is not a claim of customer visual approval.

## Try the workflow in ChatGPT first

Load the repository's [SKILL.md](../../SKILL.md) in your assistant and connect fal. Ask:

> Use the generative-media-on-bubble skill to prototype this pullback. Read the exact FLUX recipe and preserve the two reference-image roles. Show the current price estimate and confirm my permitted test budget before a paid request. Inspect the actual video with me. After I accept the result, wire that recipe into my existing Bubble app through its BYOK plugin and Bubble MCP, with owned saved results and manual Refresh.

Already accepted this recipe? Reuse that approval and continue with integration; do not pay to repeat the creative review. If you only want a showcase, ask for a read-only player using the saved finished file. The skill does not include a fal account, provider credits, a ChatGPT connector or a hosted runtime.

## Wire the accepted recipe into an existing app

Use the [integration guide](../../docs/bubble-integration.md) and [Bubble MCP reference](../../references/bubble-mcp.md) to inspect the existing app and installed [BYOK plugin](../../references/plugin.md). Reuse its generation form, current user, existing job and media types where suitable. Resolve the installed action IDs, map the recipe's `model` to `endpoint_id`, and serialize only its `input` object as `input_json`. Keep the app owner's key in private server-side plugin settings: it is separate from ChatGPT's connected fal credentials.

Before a paid preview test, compare the outgoing payload with the [accepted recipe record](../../references/validated-recipe.md): endpoint, complete prompt, reference roles, duration, resolution, audio and any finishing steps must match. Follow the [job, privacy and storage workflow](../../references/workflow.md) to save ownership and request IDs, refresh manually, retain returned media, and resume after a reload. A saved provider URL alone is not durable file storage. Retain any source images your app is authorized to use.

Bind the returned request ID and status to the owned job. Refresh that saved request manually and bind the actual returned video to the result player. Retain the file through an authorized storage route, and link its media record to the job and the app's existing project or creation record. Do not create competing records for the same result; see [plugin and storage](../../docs/plugin-and-storage.md).

For a public static demo, reuse the finished file and show its prompt/settings read-only. Add no generation or recurring polling workflow. Verify playback and links anonymously, then test live generation separately only when requested and covered by the user's budget.
