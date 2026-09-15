# WF-07: H3 + SeedVR2

Run recorded on 2026-09-15. The workflow generates H3 video and audio, applies SeedVR2 3B FP16 to the frames, and saves the original and upscaled MP4s.

- [Workflow JSON](workflow-run.json) and [API JSON](workflow-run.api.json)
- [Original video](generated.mp4), [SeedVR2 output](upscaled.mp4), and [comparison preview](comparison.mp4)
- [Media measurements](validation.json), [ComfyUI history](history.json), and [run metadata](run.json)

The comparison first shows both full frames fitted to equal panels, then matching central crops. The original crop is enlarged 2x with bicubic interpolation for display; the SeedVR2 crop uses native pixels. The original audio repeats for the two segments. Use the separate MP4s to view each output at its original resolution.

The H3 request uses seed 2026091502, 1344 x 768, 24 FPS, and duration 5.167 seconds. SeedVR2 uses seed 42, short edge 1536, five-frame batches, one overlapping frame, uniform batch padding, and LAB color correction. The workflow JSON contains the model, offload, and VAE tiling settings.
