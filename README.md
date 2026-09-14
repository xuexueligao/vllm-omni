# WF-07 run artifacts

Run date: 2026-09-14. Code PR: https://github.com/vllm-project/vllm-omni/pull/7472

The videos and audio are model-generated examples. `generated.mp4` is the H3 response saved before frame upscale; `upscaled.mp4` is the result after RealESRGAN_x2plus, recombined with the generated audio and FPS.

## Recorded run

- Prompt ID: `a70b6b03-7644-4403-b61b-74894469bba5`; seed: `2026091402`.
- ComfyUI extension and E2E runner: `684a4e07c7c11f8553727bc5662886a345929487`, based on `58adeec05f151e18542323cb4644b009b83cfef3`. SHA-256 hashes for all six changed files are in `e2e-command.json`.
- H3 backend: existing service on `33fff2155e0bf9bc8688f7ffb92ef508efdf6954` plus the WF-07 worktree changes. The backend was not restarted from the newer submission base.
- H3 runtime: Python 3.12.14, vLLM 0.29.0+cu129, PyTorch 2.13.0+cu129, NVIDIA driver 570.190; two RTX 4090 GPUs. FL2VA with LightX2V v1.0 768p Turbo, 5 sigma points, video/audio shifts 6/3, TP 2, layerwise offload 8.
- ComfyUI: `cbbc9dab1f03d0d9a6caa8a8be7d77a7e37e1e44`, Python 3.10.19, PyTorch 2.9.0+cu128, PyAV 17.1.0; a separate RTX 4090 GPU.

The workflow uses the `duration` input and the H3 aspect-ratio/audio handling already merged in PR #7456. `duration=5.167` at 24 FPS converts to 124 requested frames. This code PR no longer changes the production decoder or H3 parameter nodes.

## Files

`workflow-run.json` is the UI graph for this run, including the seed. `submitted_prompt.json` records the API request and UI graph. `history.json` records ComfyUI execution and output filenames. `e2e-command.json` records the command, environment overrides and source hashes; `e2e-pytest.log` contains pytest output. The two PNG files are captures of the real ComfyUI page: the workflow and its upscaled-video preview.

The recorded pytest summary is `1 passed, 14 warnings in 152.12s`. The warnings are from torch.jit deprecation. The generation node did not reuse cached output. `validation.json` contains all measured values and both MP4 SHA-256 hashes.

| Field | Generated | Upscaled |
| --- | --- | --- |
| Dimensions | 1344 x 768 | 2688 x 1536 |
| Video | 124 frames, 24 FPS | 124 frames, 24 FPS |
| Video duration | 5.166667 s | 5.166667 s |
| Audio | AAC, 32 kHz stereo | AAC, 32 kHz stereo |
| Audio duration | 5.167 s | 5.167 s |

The measured audio lag was 0 ms in the full, early and late comparisons, with waveform correlation approximately 1.0. These measurements describe timing and audio preservation; they do not score visual quality or semantic audio/action synchronization.

Earlier artifacts remain available in this evidence branch's commit history. Reproduction and service setup are documented in the code PR under `apps/ComfyUI-vLLM-Omni/docs/wf07-h3-upscale.md`.
