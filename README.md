# ComfyUI Workflow Collection

A set of pre-configured ComfyUI workflows covering text-to-image, image editing, image upscaling, face swapping, text generation, and image-to-video tasks.

## Quick Start

1. **Install ComfyUI** — Follow the [official guide](https://github.com/comfyanonymous/ComfyUI)
2. **Download required models** — See model links and storage paths below
3. **Load a workflow** — In the ComfyUI web interface, click Load and select any `.json` file
4. **Configure parameters** — Adjust prompts, models, and sampling settings as needed
5. **Run generation** — Click Queue Prompt to start

## Workflow Overview

| Category | File | Core Model | Description |
|----------|------|------------|-------------|
| Text-to-Image | `Z-image_turbo_TextToImg.json` | Z-Image Turbo | Fast text-to-image, 8-step generation, supports LoRA stacking |
| Text-to-Image | `image_z_image_int8.json` | Z-Image (int8) | int8 quantized version, VRAM-friendly, supports resolution selector and LoRA |
| Text-to-Image + ControlNet | `Z-image_turbo_TextToImg_ControlNet.json` | Z-Image Turbo + ControlNet Union 2.1 | ControlNet spatial guidance + Qwen3.5 auto-prompt + SeedVR2 upscale integrated |
| Image Editing | `image_qwen_image_edit_2511_int8.json` | Qwen-Image-Edit 2511 (int8) | Qwen-based image editing with Lightning 4-step acceleration LoRA |
| Image Scaling | `img_scale_edit.json` | None (node-only) | Simple image scaling utility |
| Face Swap | `insightface_faceswap.json` | InsightFace (inswapper_128) | ReActor face swap with CodeFormer face restoration |
| Image Upscale | `utility_seedvr2_7b_int8_upscale_image.json` | SeedVR2 7B (int8) | High-quality image super-resolution, supports 2x/4x upscale |
| Image Upscale | `utility_z_image_turbo_2k_upscaler.app.json` | Z-Image Turbo + RealESRGAN | Two-stage upscale: RealESRGAN upscaling + Z-Image Turbo refinement |
| Text Generation | `llm_qwen3_5_text_gen.json` | Qwen3.5 4B | Image-to-text / text generation, useful for auto prompt generation |
| Image-to-Video | `video_ltx2_3_i2v.json` | LTX-Video 2.3 | Image-to-Video generation |

---

## Workflow Details

### Text-to-Image

#### `Z-image_turbo_TextToImg.json`

Z-Image Turbo fast text-to-image workflow.

- **Model**: `z-image-turbo-fp8-e4m3fn.safetensors` (fp8; bf16 variant also available)
- **Text Encoder**: `qwen_3_4b.safetensors` (lumina2 type)
- **VAE**: `ae.safetensors`
- **Sampling**: 8 steps / CFG 1 / euler / normal
- **LoRA Support**: Pre-configured with Detail Slider, body shape adjustment, and other LoRAs (toggle on/off as needed)
- **Default Resolution**: 1024x1024

#### `image_z_image_int8.json`

Z-Image int8 quantized text-to-image workflow with lower VRAM usage.

- **Model**: `z_image_int8_convrot.safetensors`
- **Text Encoder**: `qwen_3_4b.safetensors` (lumina2 type)
- **VAE**: `ae.safetensors`
- **Sampling**: 30-50 steps / CFG 3-5 / euler / simple
- **Feature**: Built-in `ResolutionSelector` node supporting multiple aspect ratios and megapixel options
- **LoRA Support**: Pre-configured with style LoRA (e.g., `Kook_Zimage_瑶光.safetensors`)

### Text-to-Image + ControlNet

#### `Z-image_turbo_TextToImg_ControlNet.json`

The most feature-rich all-in-one workflow, integrating ControlNet, auto-prompt generation, and image upscaling.

- **Main Model**: `z_image_turbo_nvfp4.safetensors` (nvfp4; bf16 variant also available)
- **ControlNet**: `Z-Image-Turbo-Fun-Controlnet-Union-2.1.safetensors`
- **Preprocessor**: DepthAnythingV2 (depth map), replaceable with other preprocessors
- **Auto Prompt**: Uses Qwen3.5 4B (`qwen3.5_4b_bf16.safetensors`) to generate prompts from the input image
- **Upscale (optional)**: Embedded SeedVR2 7B subgraph, supports 2x super-resolution
- **LoRA**: Multiple pre-configured LoRAs can be stacked
- **Sampling**: 8 steps / CFG 1 / res_multistep / simple / denoise 0.8
- **Pipeline**: Load Image → Preprocess → ControlNet Guidance → Text-to-Image → (optional) Upscale
<img width="1137" height="797" alt="螢幕擷取畫面 2026-09-22 130102" src="https://github.com/user-attachments/assets/90df1863-1b6e-4ef7-8561-5e0d061d93cb" />

### Image Editing

#### `image_qwen_image_edit_2511_int8.json`

Qwen-Image-Edit 2511 based image editing workflow.

- **Model**: `qwen_image_edit_2511_int8_convrot.safetensors`
- **VAE**: `qwen_image_vae.safetensors`
- **Text Encoder**: `qwen_2.5_vl_7b_fp8_scaled.safetensors`
- **Lightning LoRA**: `Qwen-Image-Edit-2511-Lightning-4steps-V1.0-bf16.safetensors` (4-step fast generation)
- **Style LoRA**: `anime2real2511_25.safetensors` (anime-to-realistic style transfer)
- **Feature**: Built-in switch nodes to toggle between Lightning 4-step mode (CFG 1, 8 steps) and standard mode (CFG 4, 40 steps)
- **Sampling Reference**:

  | Mode | Steps | CFG |
  |------|-------|-----|
  | Standard | 40 | 4.0 |
  | Lightning 4-step | 8 | 1.0 |

#### `img_scale_edit.json`

Simple image scaling utility workflow.

- **Function**: Load Image → Scale by factor → Save
- **Scale Method**: nearest-exact (changeable to lanczos, etc.)
- **Default Scale Factor**: 0.5 (downscale to half)
- **No models required**: Pure node operation, ready to use

### Face Swap

#### `insightface_faceswap.json`

InsightFace ReActor based face swap workflow.

- **Swap Model**: `inswapper_128.onnx`
- **Face Detection**: `retinaface_resnet50`
- **Face Restoration**: `codeformer-v0.1.0.pth` (CodeFormer)
- **Inputs**: Target image (face to replace) + Source face image
- **Parameter**: `codeformer_weight` controls similarity (0-1, higher values = closer to source face)
- **Output**: Face-swapped image

### Image Upscale

#### `utility_seedvr2_7b_int8_upscale_image.json`

SeedVR2 7B int8 high-quality image super-resolution workflow.

- **Model**: `seedvr2_7b_int8_convrot.safetensors`
- **VAE**: `seedvr2_ema_vae_fp16.safetensors`
- **Scale Factor**: Default 2x (adjustable to 4x)
- **Feature**: Encapsulated as a subgraph with tiled VAE encode/decode (tile_size 512, overlap 128)
- **Color Correction**: Optional none / post-processing color correction
- **Comparison**: Built-in ImageCompare node for before/after comparison

#### `utility_z_image_turbo_2k_upscaler.app.json`

Z-Image Turbo + RealESRGAN two-stage upscale workflow (App linear mode).

- **Upscale Model**: `RealESRGAN_x4plus.safetensors` (Stage 1: 4x upscaling)
- **Refinement Model**: `z_image_int8_convrot.safetensors` (Stage 2: detail enhancement)
- **Text Encoder**: `qwen_3_4b.safetensors`
- **VAE**: `ae.safetensors`
- **Denoise Parameter Guide**:

  | Style | Value Range | Effect |
  |-------|-------------|--------|
  | Subtle | 0.15 - 0.25 | Output stays close to input |
  | High Creativity | 0.25 - 0.35 | More reinterpretation & details |

  > Denoise above 0.35 may introduce artifacts. Use a detailed prompt for more stable outputs at high denoise values.

### Text Generation

#### `llm_qwen3_5_text_gen.json`

Qwen3.5 4B text generation workflow supporting image-to-text (image description / prompt reverse-engineering).

- **Model**: `qwen3.5_4b_bf16.safetensors`
- **Type**: stable_diffusion (CLIP loader type)
- **Function**: Load Image → Qwen3.5 generates text description → Preview output
- **Parameters**: Max length 512 / temperature 0.7 / top_k 64 / top_p 0.95 / repetition_penalty 1.05
- **Default Prompt**: Instructs the model to describe the image in extreme detail, suitable for generating AI image generation prompts

### Image-to-Video

#### `video_ltx2_3_i2v.json`

LTX-Video 2.3 Image-to-Video workflow.

- **Function**: Transforms a static image into a dynamic video
- **Model**: LTX-Video series
- **Complexity**: High — multi-node orchestration with adjustable video parameters

---

## Model Storage Paths

All models should be placed under the `models/` directory in your ComfyUI installation:

```
ComfyUI/
├── models/
│   ├── diffusion_models/
│   │   ├── z-image-turbo-fp8-e4m3fn.safetensors
│   │   ├── z_image_turbo_nvfp4.safetensors          (for ControlNet workflow)
│   │   ├── z_image_turbo_bf16.safetensors           (optional, bf16 variant)
│   │   ├── z_image_int8_convrot.safetensors
│   │   ├── qwen_image_edit_2511_int8_convrot.safetensors
│   │   ├── seedvr2_7b_int8_convrot.safetensors
│   │   └── ltx-video model files
│   ├── text_encoders/
│   │   ├── qwen_3_4b.safetensors
│   │   ├── qwen_2.5_vl_7b_fp8_scaled.safetensors
│   │   └── qwen3.5_4b_bf16.safetensors
│   ├── vae/
│   │   ├── ae.safetensors
│   │   ├── qwen_image_vae.safetensors
│   │   └── seedvr2_ema_vae_fp16.safetensors
│   ├── loras/
│   │   ├── Qwen-Image-Edit-2511-Lightning-4steps-V1.0-bf16.safetensors
│   │   ├── anime2real2511_25.safetensors
│   │   ├── Z-Detail-Slider.safetensors
│   │   └── ... (other style/feature LoRAs)
│   ├── upscale_models/
│   │   └── RealESRGAN_x4plus.safetensors
│   ├── model_patches/ (or corresponding directory)
│   │   └── Z-Image-Turbo-Fun-Controlnet-Union-2.1.safetensors
│   ├── insightface/
│   │   ├── inswapper_128.onnx
│   │   └── retinaface_resnet50 (model files)
│   ├── facerestore_models/
│   │   └── codeformer-v0.1.0.pth
│   └── unet/ (if using legacy directory structure)
│       └── ...
```

## Model Download Links

| Model | Source |
|-------|--------|
| Z-Image Turbo | [Comfy-Org/z_image_turbo](https://huggingface.co/Comfy-Org/z_image_turbo) |
| Z-Image (int8) | [Comfy-Org/z_image](https://huggingface.co/Comfy-Org/z_image) |
| Qwen-Image-Edit 2511 | [Comfy-Org/Qwen-Image-Edit_ComfyUI](https://huggingface.co/Comfy-Org/Qwen-Image-Edit_ComfyUI) |
| Qwen-Image VAE | [Comfy-Org/Qwen-Image_ComfyUI](https://huggingface.co/Comfy-Org/Qwen-Image_ComfyUI) |
| Qwen 2.5 VL 7B (text encoder) | [Comfy-Org/HunyuanVideo_1.5_repackaged](https://huggingface.co/Comfy-Org/HunyuanVideo_1.5_repackaged) (hosted under HunyuanVideo repo) |
| Qwen 3.5 4B (text encoder) | [Comfy-Org/Qwen3.5](https://huggingface.co/Comfy-Org/Qwen3.5) |
| SeedVR2 7B | [Comfy-Org/SeedVR2](https://huggingface.co/Comfy-Org/SeedVR2) |
| RealESRGAN x4plus | [Comfy-Org/Real-ESRGAN_repackaged](https://huggingface.co/Comfy-Org/Real-ESRGAN_repackaged) |
| Qwen-Image-Edit Lightning LoRA | [lightx2v/Qwen-Image-Edit-2511-Lightning](https://huggingface.co/lightx2v/Qwen-Image-Edit-2511-Lightning) |

## Tips

- **Low VRAM**: Use int8 quantized workflows first (e.g., `image_z_image_int8.json`)
- **Fast iteration**: Z-Image Turbo requires only 8 steps, ideal for rapid prototyping
- **Precise control**: ControlNet workflows with depth/edge maps provide accurate spatial guidance
- **Upscale choice**: SeedVR2 for high-quality upscaling; Z-Image Turbo upscaler for adding details during upscaling
- **Auto prompts**: Use `llm_qwen3_5_text_gen.json` to auto-generate prompts from images, then feed into text-to-image workflows
- **Update ComfyUI**: Some newer models require the latest ComfyUI version — [update first](https://docs.comfy.org/installation/update_comfyui)

## Resources

- [ComfyUI Repository](https://github.com/comfyanonymous/ComfyUI)
- [ComfyUI Documentation](https://docs.comfy.org)
- [ComfyUI Workflow Templates](https://github.com/Comfy-Org/workflow_templates)
- [ControlNet Project](https://github.com/lllyasviel/ControlNet)
- [InsightFace ReActor](https://github.com/Gourieff/ComfyUI-ReActor)

## License<img width="1137" height="797" alt="螢幕擷取畫面 2026-09-22 130102" src="https://github.com/user-attachments/assets/547eccf9-a07c-4a1b-8e82-be00c98ac42a" />


This workflow collection is provided for educational and creative use. Each model follows its respective license agreement.
