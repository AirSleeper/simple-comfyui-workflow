# Simple ComfyUI Workflow

A collection of streamlined ComfyUI workflows for text-to-image generation and advanced image editing with ControlNet support.

## 🚀 Quick Start

This repository contains ready-to-use ComfyUI workflows that enable you to:
- Generate high-quality images from text prompts
- Control image generation with ControlNet for precise spatial guidance
- Create animated videos from text or images

Simply load any `.json` workflow file into ComfyUI and start generating!

## 📋 Overview

This repository contains pre-configured ComfyUI workflow files optimized for:
- **Text-to-Image Generation**: High-quality image generation from text prompts
- **Text-to-Image with ControlNet**: Enhanced image generation with spatial control using ControlNet models
- **Video Animation**: Dynamic video generation and animation workflows

## 📁 Available Workflows

### Image Generation

- **`image_z_image_turbo.json`**
  - Fast text-to-image generation using Turbo models
  - Optimized for quick inference times (~1-5 seconds)
  - Ideal for rapid prototyping and testing

- **`image_z_image_turbo_controlnet.json`**
  - Text-to-image generation with ControlNet spatial control
  - Combines Turbo model speed with precise image guidance
  - Great for controlled image generation based on edge maps, poses, or other control inputs

- **`image_flux2_klein_image_edit_4b_distilled.json`**
  - Advanced image editing workflow using Flux2 Klein models
  - Distilled 4B variant for efficient processing
  - Supports inpainting and image manipulation tasks

### Video Generation

- **`video_wan2_2_14B_animate.json`**
  - Video animation and generation workflow
  - Uses WAN2 2 14B model for high-quality video output
  - Perfect for creating animated sequences from prompts or images

## 📖 How to Use

1. **Install ComfyUI** - Follow [official instructions](https://github.com/comfyanonymous/ComfyUI)
2. **Clone this repository** into your ComfyUI workflows directory
3. **Open ComfyUI** web interface
4. **Load a workflow** - Click Load and select any `.json` file from this repo
5. **Configure parameters** - Adjust prompts, models, and settings as needed
6. **Generate** - Click Queue Prompt to start generation

## 💡 Tips

- Use detailed and descriptive text prompts for better results
- For ControlNet workflows, prepare control maps (edge detection, pose, depth, etc.)
- Ensure all required models are downloaded to your ComfyUI directory
- Monitor GPU memory usage; adjust batch sizes if needed on lower-end hardware

## 🔗 Resources

- [ComfyUI Repository](https://github.com/comfyanonymous/ComfyUI)
- [ControlNet Guide](https://github.com/lllyasviel/ControlNet)

## 📄 License

This project is provided for educational and creative use.