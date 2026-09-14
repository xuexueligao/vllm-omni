# WF-07 run artifacts

Run date: 2026-09-14. Code PR: https://github.com/vllm-project/vllm-omni/pull/7472

The videos and audio are model-generated examples. `generated.mp4` is the H3 response saved before frame upscale; `upscaled.mp4` is the result after RealESRGAN_x2plus, recombined with the generated audio and FPS.

## Recorded run

- Prompt ID: `5f1db575-0ee9-454f-a619-67b0695b3880`; seed: `2026091401`.
- ComfyUI extension and E2E runner: `9724dbaf3cfbda8cd7704e67c0cdf2516c0de518`, based on `6753bbd18c9a1f45cfebcec3eacdd2f1e5ea859b`. The run preceded the commit; the runner and test SHA-256 hashes are in `e2e-command.json`.
- H3 backend: existing service on `33fff2155e0bf9bc8688f7ffb92ef508efdf6954` plus the WF-07 worktree changes. The backend was not restarted from the newer submission base.
- H3 runtime: Python 3.12.14, vLLM 0.29.0+cu129, PyTorch 2.13.0+cu129, NVIDIA driver 570.190; two RTX 4090 GPUs. FL2VA with LightX2V v1.0 768p Turbo, 5 sigma points, video/audio shifts 6/3, TP 2, layerwise offload 8.
- ComfyUI: `cbbc9dab1f03d0d9a6caa8a8be7d77a7e37e1e44`, Python 3.10.19, PyTorch 2.9.0+cu128, PyAV 17.1.0; a separate RTX 4090 GPU.

## Files

`workflow-run.json` is the UI graph for this run, including the seed. `submitted_prompt.json` records the API request and UI graph. `history.json` records ComfyUI execution and output filenames. `e2e-command.json` records the command, environment overrides and source hashes; `e2e-pytest.log` contains pytest output. The two PNG files are captures of the real ComfyUI page: the workflow and its upscaled-video preview.

The recorded pytest summary is `1 passed, 14 warnings in 147.27s`. The warnings are from torch.jit deprecation. The generation node did not reuse cached output. `validation.json` contains all measured values and both MP4 SHA-256 hashes.

| Field | Generated | Upscaled |
| --- | --- | --- |
| Dimensions | 1344 x 768 | 2688 x 1536 |
| Video | 124 frames, 24 FPS | 124 frames, 24 FPS |
| Video duration | 5.166667 s | 5.166667 s |
| Audio | AAC, 32 kHz stereo | AAC, 32 kHz stereo |
| Audio duration | 5.167 s | 5.167 s |

The measured audio lag was 0 ms in the full, early and late comparisons, with waveform correlation 1.0. These measurements describe timing and audio preservation; they do not score visual quality or semantic audio/action synchronization.

Earlier artifacts remain available in this evidence branch's commit history. Reproduction and service setup are documented in the code PR under `apps/ComfyUI-vLLM-Omni/docs/wf07-h3-upscale.md`.
