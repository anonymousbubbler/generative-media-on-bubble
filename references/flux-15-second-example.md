# Last One Standing: native FLUX 15-second example

[Open the Bubble example page](https://fal-plugin-test.bubbleapps.io/version-test/video_studio). Visitors can watch saved media without generating or spending credits. [Exact submitted inputs and QA record](flux-15-second-inputs.json) accompany this example.

This version uses **one native FLUX 3 Max image-to-video generation at normal speed**. Finishing removes the first video frame and raises the native mono soundtrack by 10 dB. It does not reverse or slow the picture, join clips, animate a still, paste a face layer or use the superseded 30-second rendering code.

## Model, prompt and references

Exact endpoint: `blackforestlabs/flux-3-max/image-to-video`.

The request uses `duration: 15`, `aspect_ratio: "16:9"`, `resolution: "720p"`, `reasoning_level: "high"` and `generate_audio: true`. The JSON contains the complete exact prompt. The observed output seed is 1052570381; it is recorded as output evidence, not added to the originally submitted inputs.

| Reference | Role | Submitted field |
| --- | --- | --- |
| [Last One Standing opening poster](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794947935704x147251021077931420/Last-One-Standing-Poster.jpg) | Starting composition | `image_url` |
| [Approved fallen-signs wide artwork](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794940082026x226337063916927970/fallen-brands-pool.jpg) | Ending composition | `end_image_url` |

Preserve these source roles. This is image-to-video with two reference images, not text-to-video with an unrelated new cast. The generated shot supplies the subject and clothing movement; no local face compositing is applied to this edition.

The fictional adult woman visibly breathes while holding a lowered blood-streaked prop knife. The camera pulls back to five fallen signboards with dry glass eyes and a shared pool of stage blood. The requested names are Higgsfield, OpenArt, Runway, Pika and Luma. This is fictional competitive parody; the company names do not imply endorsement.

## Actual output and finishing

The [original provider output](https://v3b.fal.media/files/b/0ab2f241/-z1FDsJ0Ht8_VOyXQuH4e_alligator.mp4) contains 361 frames at 24 fps: approximately 15.04 seconds, at **1280 × 704**. Those are the observed dimensions, despite the requested `16:9` / `720p` settings.

The delivered file removes only the first video frame, leaving 360 frames at unchanged 24 fps and **15.00 seconds**. The native mono sound receives **+10 dB gain**. No reversal, retiming, freeze extension, still animation or clip stitching is part of the finishing. [Play the retained finished file](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794996575690x415569682279433300/Last-One-Standing-FLUX-15s.mp4) or [view its poster](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794996578607x544861900337411300/Last-One-Standing-FLUX-Poster.jpg). The file is named `Last-One-Standing-FLUX-15s.mp4`; its SHA-256 is recorded in the input/QA JSON.

[Inspect the four-frame timeline](https://eb5440aa38b3699aeb0dfa3b06d2198e.cdn.bubble.io/f1794996960001x348953365241079400/Last-One-Standing-FLUX-Timeline.jpg).

## What the review found

The whole subject moves; the shoulder rise and fall are clearest around 4–5.5 seconds. The knife remains in her hand, although its tip is briefly cropped later. The bloodied knife and pool are obvious in the opening. All five names are correctly spelled in approximately the final five seconds. The camera speeds up near 10 seconds but remains continuous, with no apparent cut. The finished native mono audio measured −24.9 dB mean and −9.0 dB peak.

Technical review does not claim the customer's visual approval. A static page can present the saved example and these limitations; do not treat it as acceptance of a different requested feature or a fresh customer's plugin installation.

## Reproduce the accepted workflow in Bubble

First prototype and review actual output in ChatGPT with connected fal, following [the skill](../SKILL.md). Record the accepted [recipe](validated-recipe.md), including the two reference roles and any finishing. Use existing approval instead of restarting tests. Keep fresh paid requests within the user's authorized budget.

After acceptance, inspect the target app and map the exact endpoint, input JSON and source roles through the installed BYOK plugin and Bubble MCP. Compare the serialized app request to the accepted record before the preview test. Keep the app owner's fal key in private server-side plugin settings, separately from ChatGPT's fal connection. Use owned saved jobs, retained output files and manual Refresh.

If the app must produce the exactly finished 15.00-second version with the audio adjustment, implement those as explicit processing steps; the generic queue plugin does not automatically trim frames or adjust sound. A saved showcase can reuse the retained final file with no generation workflow. Retain permitted reference files rather than depending indefinitely on provider URLs. Model availability, schemas and pricing must be checked before a new run; this package does not establish an actual charge for the example.
