# ComfyUI Content Pipeline — AI-Powered Broadcast & OTT Workflows

Production-oriented ComfyUI workflows for automated content generation in broadcast and OTT environments. Built to demonstrate how generative AI integrates into real-world media production pipelines.

![ComfyUI](https://img.shields.io/badge/ComfyUI-Node--Based-blue)
![SDXL](https://img.shields.io/badge/Model-SDXL_1.0-green)
![ControlNet](https://img.shields.io/badge/ControlNet-Canny-orange)
![IPAdapter](https://img.shields.io/badge/IPAdapter-Plus-purple)

---

## Workflow 1: Genre Mood Board Generator

**Problem:** A single show/film concept needs to be marketed differently to different audience segments — action fans, romance viewers, thriller enthusiasts. Traditionally, this means separate photoshoots or manual design work per genre treatment.

**Solution:** One base image → ControlNet locks the composition → genre-specific prompts change only the mood, lighting, and color palette → 5 unique posters in minutes.

### How It Works

```
Base Image → Canny Edge Extraction → ControlNet (composition lock)
                                          ↓
                         ┌────────────────┼────────────────┐
                         ↓                ↓                ↓
                    Action Prompt    Romance Prompt    Horror Prompt  ...
                         ↓                ↓                ↓
                    Action Poster    Romance Poster    Horror Poster
```

- **ControlNet Canny** ensures identical character pose and framing across all variants
- Only the atmospheric elements change: color grading, lighting direction, background mood
- Same principle as CSV-driven promo versioning — but with AI generation at the core

### Output

<!-- Replace with your actual output images -->
![Genre Posters](workflow_1_genre_poster/workflow_screenshot.jpg)

| Action | Romance | Horror | Drama | Noir |
|--------|---------|--------|-------|------|
| High contrast, fiery tones | Soft golden warmth | Cold desaturated blues | Muted earth tones | High contrast B&W |

### Technical Details

- **Base Model:** SDXL 1.0
- **ControlNet:** Canny edge detection (strength: 0.65)
- **Resolution:** 1024×576 (16:9 broadcast ratio)
- **Workflow file:** [`genre_poster_workflow.json`](workflow_1_genre_poster/genre_poster_workflow.json)

---

## Workflow 2: Character Reference Sheet Generator

**Problem:** Character consistency is the #1 challenge in AI-assisted content production. Generating the same character across different angles, poses, and costumes — while maintaining facial identity — is critical for pre-visualization and storyboarding.

**Solution:** IP-Adapter locks the character's facial identity from a single reference portrait, then generates consistent multi-angle views and costume variations.

### How It Works

```
Character Description → Base Portrait (front view)
                              ↓
                     IP-Adapter (identity lock)
                              ↓
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
        Three-Quarter      Side Profile    Costume Swap
        (weight: 0.55)    (weight: 0.50)  (weight: 0.40)
              ↓               ↓               ↓
         Consistent face across all views
```

- **IP-Adapter Plus** extracts facial identity embedding from the reference portrait
- Weight reduction per stage allows pose/outfit changes while preserving face
- Lower weight = more prompt influence, higher weight = stricter face lock

### Output

<!-- Replace with your actual output images -->
![Character Sheet](workflow_2_character_sheet/workflow_screenshot.jpg)

| Front View | Three-Quarter | Side Profile | Costume Variation |
|-----------|---------------|-------------|-------------------|
| Reference portrait | 45° turned | 90° profile | Different outfit, same face |

### Technical Details

- **Base Model:** SDXL 1.0
- **IP-Adapter:** Plus (ViT-H) with variable weights per view
- **CLIP Vision:** ViT-H-14
- **Resolution:** 768×1024 (portrait orientation)
- **Workflow file:** [`character_sheet_workflow.json`](workflow_2_character_sheet/character_sheet_workflow.json)

---

## Production Context

These workflows are designed with broadcast/OTT production in mind:

- **Workflow 1** mirrors the logic of CSV-driven promo versioning systems — same content, multiple treatments, automated output. The traditional version of this pipeline (built in After Effects + ExtendScript) handles 1,000+ variants/month in production and was featured in an [Adobe India case study](https://github.com/prasadpradhan/ae-promo-versioning).

- **Workflow 2** addresses the pre-visualization bottleneck in rapid content production (micro dramas, short-form series) where character consistency across episodes is essential but traditional methods are too slow.

Both workflows are built to be **reproducible** — import the JSON files into any ComfyUI installation with the required models and custom nodes, and they run as-is.

---

## Requirements

### Models
| Model | Type | Download |
|-------|------|----------|
| SDXL 1.0 | Checkpoint | [HuggingFace](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0) |
| ControlNet Canny SDXL | ControlNet | [HuggingFace](https://huggingface.co/diffusers/controlnet-canny-sdxl-1.0) |
| IP-Adapter Plus SDXL | IP-Adapter | [HuggingFace](https://huggingface.co/h94/IP-Adapter) |
| CLIP-ViT-H-14 | CLIP Vision | [HuggingFace](https://huggingface.co/h94/IP-Adapter) |

### Custom Nodes
- ComfyUI_IPAdapter_plus
- ComfyUI-Impact-Pack
- ComfyUI_essentials
- rgthree-comfy

---

## Running on Cloud GPU

Both workflows were developed and tested on RunPod (RTX A4500, 20GB VRAM). Total cloud compute cost for building and testing both workflows: under $3.

To run on RunPod:
1. Deploy a ComfyUI template pod (RTX 3090/A4500 or better)
2. Download models to `/workspace/runpod-slim/ComfyUI/models/`
3. Install custom nodes via ComfyUI Manager
4. Import the workflow JSON files
5. Generate

---

## Related Projects

- [**AE Promo Versioning Pipeline**](https://github.com/prasadpradhan/ae-promo-versioning) — The production After Effects automation system that inspired Workflow 1's multi-variant approach
- [**AE Pipeline Assistant**](https://github.com/prasadpradhan/ae-pipeline-assistant) — LangChain RAG chatbot trained on the AE pipeline's codebase and documentation

---

## Author

**Prasad Pradhan**
Broadcast Automation Engineer | Creative Technology | AI Pipeline Engineering

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://linkedin.com/in/prasadpradhan21)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black)](https://github.com/prasadpradhan)

---

## License

MIT
