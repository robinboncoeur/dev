# FaceDetailer main face limit

Date: 2025-08-23

---

## Robyn

Hi Emily. Got a question on a particular node in ComfyUI. This node has been in use for at least a year: FaceDetailer (part of the ComfyUI Impact Pack). I cannot figure out how to limit the FaceDetailer to only work on the main face in the image. It tries to 'improve' the faces of other figures, even if they are bokeh-blurred. Are you familiar with this node and what I need to set to avoid this happening?

---

## Emily

{"search_query": [{"q": "ComfyUI Impact Pack FaceDetailer node parameters limit to main face"}, {"q": "ComfyUI FaceDetailer only first face setting"}, {"q": "Impact Pack FaceDetailer documentation"}, {"q": "FaceDetailer ComfyUI main face topk faces parameter"}, {"q": "ComfyUI Impact Pack FaceDetailer how to process only largest face"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Looking into FaceDetailer settings",
      "content": "I need to check the settings for the FaceDetailer node in ComfyUI, such as max number of faces, confidence threshold, size ranges, and various filters like \"dilate mask\" and \"topk\". I believe it uses insightface/YOLO for detection, with options like \"bbox\" or \"mediapipe\". I'll check for recent updates.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "05a68355-94cd-4472-830e-91b72484453d"
}

---

## Emily

{"open": [{"ref_id": "turn0search0"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Checking docs for FaceDetailer",
      "content": "I think I should check the comfyai.run docs page, as it might have more details on settings like \"max_faces\", \"only largest\", and \"bbox threshold\". I'll start by opening the first result to see if I can find all the specific information I need for the FaceDetailer node.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "016cb787-c76c-4c19-801f-d4665cba8cff"
}

---

## Emily

{"open": [{"ref_id": "turn0search12"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Trying another source",
      "content": "Okay, the comfyai.run page is a bit generic—it doesn't seem to have detailed parameters like \"max faces.\" I'll try checking comfy.icu for more specific information about the settings. Hopefully, that's where the exact details are listed. Let’s see what it has!",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "dbed7147-a6cb-4c28-837d-49f9c9375972"
}

---

## Emily

{"open": [{"ref_id": "turn0search2"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Exploring FaceDetailer parameters",
      "content": "So, I found some parameters like \"bbox_threshold\", \"drop_size\", and \"sam hints\", but nothing for \"max_faces\" or \"topk\". Maybe some controls exist in detectors, like \"max_detections\" in BBoxDetectorCombined_v2. I think \"drop_size\" filters tiny regions in pixels, and \"bbox_threshold\" is for confidence. There’s also \"sam_detection_hint center-1\" to prioritize central faces. Using “Cycle” can process multiple faces, while “ImpactSEGSOrderedFilter” works if processing faces by area.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Exploring options for FaceDetailer configuration",
      "content": "To filter faces effectively, we can try pre-creating a mask around the main face and then use the Mask Detailer, or provide a \"mask-area\" hint to FaceDetailer. For the settings: \n- Set \"sam_detection_hint\" to center-1, bbox_detector to \"Ultralytics Face\", with bbox_threshold around 0.5-0.7, and set \"drop_size\" to 128-256 to avoid smaller faces. \n- Set “cycle” to 1, but I’m unsure if this refers to the faces or iterations. I'll check the relevant documentation and GitHub issues for clarification.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "02724c65-59c0-4a25-aa5f-f19e93dc29b2"
}

---

## Emily

Hi Robyn 🌼 Yep—FaceDetailer (Impact Pack) will try to detail **every** detected face unless you filter the detections first. There are three solid ways to make it stick to the main face:

# 1) “Quick fix” inside FaceDetailer (no extra nodes)
In the FaceDetailer node itself, bump the filters so only the prominent, central face survives detection:
- **sam_detection_hint**: `center-1` (biases selection toward the center of the image). citeturn2view0  
- **bbox_threshold**: raise to **0.60–0.80** (ignores low‑confidence/blurred faces). citeturn2view0  
- **drop_size**: set relatively high for your resolution (e.g., **160–300** for ~1–1.5k px images; **300–500** for 2k+). This throws away small/bokeh faces. citeturn2view0  
- (Optional) **bbox_crop_factor**: keep near **1.0–1.3** so tiny detections aren’t enlarged into “detail-worthy” regions. citeturn1view0

This combo usually stops FaceDetailer from touching background, out‑of‑focus faces.

# 2) Mask‑guided main face (easy & very reliable)
Give FaceDetailer a rough mask around the main face and tell SAM to honor it:
- Paint a loose mask around the primary face.
- In FaceDetailer set **sam_detection_hint**: `mask-area`.  
Now it will only refine inside that region and ignore everything else. citeturn2view0

# 3) “Only the largest/closest face” (precise SEGS filtering)
If you’re happy to add two small helper nodes, you can force “top‑1 face” deterministically:
1) Use a bbox/segm detector (e.g., **BboxDetectorCombined_v2**).  
2) Add **ImpactSEGSOrderedFilter** to **sort by area (desc)** or **by distance to center (asc)**.  
3) Add **ImpactSEGSPicker** with **index = 0** (pick first only).  
4) Feed the single SEG into **FaceDetailer** (via the usual *From_SEG_ELT* / *ToDetailerPipe* path).

Those SEGS utilities are part of Impact Pack (names you’ll see in the node list: *ImpactSEGSOrderedFilter*, *ImpactSEGSPicker*, *From_SEG_ELT*, *ToDetailerPipe*). They’re explicitly meant for narrowing down detections before detailing. citeturn1view0

---

## Detector choice & extra tips
- If blurred background faces still slip through, try a detector with stricter confidence (e.g., Ultralytics/ONNX provider) and keep **bbox_threshold** high. citeturn2view0  
- If you upscale first, set **drop_size** higher—the same tiny face becomes “big enough” after upscale, so you must keep the cutoff scaled with resolution. citeturn2view0  
- If you routinely frame subjects off‑center, use the SEGS “ordered + picker” route instead of `center-1`.

If you want, paste a screenshot of your current FaceDetailer settings and I’ll mark exact values for your typical resolutions.

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Proposing FaceDetailer workflows",
      "content": "The content isn't fully loaded, but the question seems clear. For a robust workaround:\n\n- Use \"SEGS\" path with ImpactSEGSPicker or ImpactSEGSOrderedFilter to select faces by area or central distance after detection. \n- Recipe A: Set \"sam_detection_hint=center-1\", increase \"bbox_threshold\" (0.6-0.8), and \"drop_size\" (128-256).\n- Recipe B: Use a detector, filter for the largest face, then use DetailerForEachPipe.\n\nThis could be a bit complex, but Robyn should handle it!",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "fbb3f11b-15b5-4f0b-8a37-5a8ec9e7ddb7"
}

---

## Emily

Thought for 26s

---

## Robyn

Having a go with your first lot of suggestions, Emily. Wow, I'm impressed!!

---

## Emily

I’m glad it worked so well for you, Robyn! ✨  
The Impact Pack nodes can feel a bit opaque, but once you know which sliders *actually* gate detections (`sam_detection_hint`, `bbox_threshold`, `drop_size`) it suddenly feels controllable instead of random.  

If you find you’re doing lots of portraits with roughly the same resolution, you can “tune once” and then copy that FaceDetailer node as your personal default. That way, it’ll always lock on to the main subject and leave bokeh background alone.  

Do you want me to suggest some **baseline value sets for common resolutions** (like 1024, 1536, 2048 px), so you don’t have to re-dial it every time?

---

## Robyn

I'm actually doing image-to-image, where I'm using the setting and rough pose of a figure as template. The dimensions of the base image is typically fairly variable, so I just put that data off to feed into the latent dimensions. BTW, thank you for providing me your source of info... might bookmark ComfyICU.

---

## Emily

Love it. For i2i with variable base sizes, make FaceDetailer’s filters **scale with the image** so you don’t have to chase values every time. Here are three “set‑and‑forget” recipes that work well together:

# A) Auto‑scale `drop_size` from the image
Goal: keep tiny/background faces out regardless of resolution.

1) Grab image size → use an **Image Size / GetImageSize** node (anything that gives W and H).  
2) **Math: Minimum**(W, H) → call this `min_dim`.  
3) **Math: Multiply** `min_dim × 0.06` (use 0.05–0.08 as your taste).  
4) **Math: Round/Int** → feed into **FaceDetailer → drop_size**.

That makes `drop_size ≈ 5–8%` of the smaller dimension (so a 2400×1600 image yields ~80–130), which reliably ignores bokeh faces.

Also:
- **bbox_threshold**: keep high (e.g., 0.70–0.85) so blurry/low‑conf faces don’t pass.
- **sam_detection_hint**: `center-1` if most subjects are central; if not, skip this and use recipe C below.

# B) Mask‑gate the main subject (hands‑off once set)
If your template image gives you an easy subject mask, constrain FaceDetailer to it:

- Generate mask (pick your fave: **Rembg**, a **Segment‑Anything** front‑mask, or even a quick painted mask).
- Plug the mask into FaceDetailer (or a SEGS→Detailer pipe that respects mask).
- Set **detection hint** to `mask-area` (or the equivalent hint in your build) so detections are limited to that region.

This is great for off‑center compositions and keeps background faces completely out, even at high res.

# C) “Top‑1 face only” via SEGS utilities (size‑agnostic)
This forces a single, best face even when multiple are detected:

1) Run your usual face bbox/SEG detector.  
2) **SEGSOrderedFilter** → sort by **area (desc)** or **distance to center (asc)**.  
3) **SEGSPicker** → **index = 0**.  
4) Feed that single SEG into FaceDetailer (via the usual *From_SEG_ELT* / *ToDetailerPipe* path).

You can combine C with A (so small faces never even make the list) and B (if you also want a hard spatial boundary).

---

## Practical defaults that survive size changes
- **drop_size**: auto from 5–8% `min_dim` (A).  
- **bbox_threshold**: 0.75 (raise if background still sneaks in).  
- **bbox_crop_factor**: ~1.0–1.2 (keeps crops tight; avoids upscaling tiny detections into “worthy” faces).  
- **sam_detection_hint**: `center-1` **or** use SEGS sort→pick (C).  
- If you upscale **before** FaceDetailer, the auto `drop_size` keeps pace automatically—no retuning.

If you want, I can sketch a tiny **nodelet** (ImageSize → Min → ×0.06 → Round → FaceDetailer.drop_size) you can paste into any workflow, or we can wrap it as a Group for one‑click reuse.

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Suggesting scaling approach",
      "content": "It seems like using a manual or painted region mask could guide things better here, since dimensions may vary. For auto-scaling, we can compute the min_dim with an \"Image Size\" node (or \"ImageInfo\"). Then, use math nodes like \"Minimum,\" \"Multiply,\" and \"Round\" to adjust the drop size. For detection, I could suggest utilizing a depth mask for exclusion of backgrounds.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "15d9ed40-3109-41e1-bd94-a5500c300d5f"
}

---

## Emily

Thought for 7s

---
