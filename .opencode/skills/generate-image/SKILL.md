---
name: generate-image
description: |
  Use ONLY when the user asks you to generate, create, or make an image/picture with
  ComfyUI, or when a task requires image generation as part of a pipeline.
  Covers: "generiere ein Bild", "create an image", "make a picture", "generate for
  this scene", "render a character".

  Do NOT trigger for general ComfyUI tasks like installing nodes, downloading models,
  fixing workflows, or troubleshooting errors — those are covered by other skills.
---

# Image Generation

This skill controls how images are generated and saved to the project workspace.

## Workflow Selection (MUST follow this order)

1. **Call `list_packs`** to see which ready-made txt2img packs are available.
2. **Pick the best matching pack** for the desired model family (e.g.
   `z-image-turbo-txt2img` for Z-Image Turbo, `ernie-txt2img` for ERNIE, etc.).
   Prefer packs with `has_workflow: true`.
3. **Read the pack's ready workflow** with `read_pack_workflow(name)`. Use this as
   the foundation — do NOT build a workflow from scratch unless the pack workflow
   fails.
4. **Adapt** the workflow only if needed (change resolution, prompt node, etc.)
   using `modify_workflow` or by building a workflow based on the pack's node graph.
5. **Validate** with `validate_workflow` before enqueuing.
6. **Enqueue** with `enqueue_workflow`.

## Saving to Workspace

After successful generation:

1. Call `get_history(prompt_id)` to get the output filename.
2. Call **`get_image(filename, save_dir="/home/laptop/_dev/repositories/tavilo")`**
   to save the image directly to the project workspace root.
3. Confirm to the user where the image was saved.

## Prompt Language

The user may write prompts in German, English, or mixed. Pass the prompt
**verbatim** (untranslated, as-provided) to the ComfyUI workflow. Do NOT
translate or rewrite unless the user explicitly asks for a translation.

## Important Notes

- Always clear VRAM with `clear_vram` before switching between different model
  families.
- The project root (workspace save dir) is:
  `/home/laptop/_dev/repositories/tavilo`
- Use Z-Image Turbo with `--use-pytorch-cross-attention` launch flag (not
  SageAttention).
