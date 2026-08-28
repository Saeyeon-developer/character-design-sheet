---
name: acg-character-settei-codex
description: Use when the user asks for a one-page anime or manga character settei/design sheet with front, side, back, expressions, details, and a color palette, from a text brief and optional identity or layout references. Do not use for a single character illustration or unrelated concept art.
---

# ACG Character Settei with Codex Image Generation

Create one landscape raster character design sheet that reads like Japanese anime production reference material. The priority order is:

1. Character identity consistency across every panel.
2. Complete front/side/back turnaround coverage.
3. Readable expressions, details, and palette areas.
4. Decorative polish.

This is a thin workflow skill, not a character-bible or web-app builder.

## When to use

Use for requests such as:

- "Make a character settei sheet / character sheet."
- "Create an animation design sheet with front, side, and back views."
- "Include an expression sheet and costume details."
- "Turn this character reference into an anime production model sheet."

Do not use when the user only wants one illustration, a story scene, a poster, or a general concept-art exploration without a multi-view sheet.

## Image-generation boundary

Use the built-in image generation tool supplied by the `imagegen` skill (`image_gen`; it may appear in the tool list as `image_gen__imagegen`). Generate the sheet in one built-in call whenever possible.

- Do not call Gemini, `google-genai`, a hosted image service, or any other external image API.
- Do not use the `imagegen` CLI fallback or ask for an API key for this workflow.
- If the built-in image tool is unavailable, explain that the requested deliverable cannot be generated in this run; do not silently switch backends.
- Treat a supplied character image as a generation reference unless the user explicitly asks to edit that image. Treat a supplied template as a layout reference, never as the identity source.

Read [references/prompt-guide.md](references/prompt-guide.md) before composing the generation prompt. Read [references/brief-template.md](references/brief-template.md) when the request is sparse or the user wants a fill-in form.

## Inputs and reference roles

Accept any of these combinations:

1. **Text brief only.** Normalize the description into a compact identity summary and infer only neutral production details needed to fill the sheet.
2. **One character reference.** Use it as the identity anchor: preserve hair, face, eyes, silhouette, costume construction, accessories, shoes, and colors.
3. **Several character references.** Choose the most recent, complete, and on-model image as the main identity anchor. Use the others only to resolve missing details; do not average conflicting designs.
4. **Template image.** Use it only for panel hierarchy, spacing, line weight, paper/background treatment, and annotation style.

When both reference types exist and you can control the input order, pass the main identity reference first and the layout reference second, then identify them explicitly in the prompt:

- `Image A: character identity reference.`
- `Image B: layout/style reference for sheet composition only.`

Identity has priority over layout. If a layout reference conflicts with the character reference, keep the character design and adapt the layout.

If several identity references are present, use `Image A` for the main anchor, list the remaining identity references as supporting references, and put the template after them. If attached images cannot be reordered, label each image according to its actual tool order instead of forcing the A/B wording; accurate role labels matter more than a fixed index.

For attached images that have no local paths, pass the smallest `num_last_images_to_include` value that includes every intended reference, up to the tool limit. For local files, inspect them with `view_image` first, then use `referenced_image_paths`. Never pass both reference mechanisms in one call. Repeat the image roles in the prompt so the model does not confuse identity and layout.

## Workflow

### 1. Parse and normalize the request

Extract, when available:

- age cue (child, teen, adult) and gender/presentation;
- personality, mood, and world/genre;
- hair shape, length, fringe, color, and hair accessories;
- face shape, eye color/shape, markings, and defining facial features;
- body proportion and overall silhouette;
- outfit layers and construction, colors, patterns, fasteners, shoes, and accessories;
- requested expressions, optional 3/4 view, props, and exclusions;
- identity-reference images and layout-reference images.

Do not invent a name, weapon, logo, story prop, or elaborate setting unless the user asks for it. If the description is short but usable, strengthen the production wording internally instead of stopping for non-critical questions. Ask only when a missing detail would change the character's identity or the requested output materially.

### 2. Write the master identity summary

Before generating, keep a short internal **Master Identity Summary**. It is the fixed anchor for every panel, not a new deliverable. Include:

- **Face/head:** age cue, face shape, eye color, hair silhouette, fringe, and identifying marks.
- **Silhouette:** height/proportion cue, shoulder/waist/hem shape, and distinctive outline.
- **Costume construction:** exact layers, collar/sleeves/closures, hem length, shoes, and accessories.
- **Palette:** four to eight key colors, with approximate light/dark roles when useful.
- **Read:** personality and emotional baseline without changing the physical design.

Compress this to a few concrete sentences. Repeat the most important invariants in the image prompt; do not bury them in a long character biography.

### 3. Compose one settei prompt

Use the labeled prompt structure in the prompt guide. The prompt must request:

- a **character design sheet / settei sheet** for anime production reference;
- a single clean landscape page, preferably 16:9 when the tool supports that choice;
- full-body **front, side, and back views** in neutral standing poses;
- an optional 3/4 view only after the three required views have room;
- a compact **expression sheet** with at least four expressions, targeting six when legible (neutral, smile, angry, surprised, sad, embarrassed or equivalent);
- hair, eye, costume, shoe, and accessory **detail callouts**;
- a four-to-eight-swatch **color palette**;
- the same character and the same costume construction in every view;
- white or very light neutral paper background, clean linework, restrained shading, clear panel hierarchy, and minimal decoration.

Prefer positive, concrete layout instructions. Use short labels such as `FRONT`, `SIDE`, `BACK`, `EXPRESSIONS`, `DETAILS`, and `PALETTE` only when labels help. Do not depend on long Japanese annotations or paragraphs being rendered accurately; the drawings and swatches carry the information.

### 4. Generate the first sheet

Call the built-in image tool once with the complete prompt and the labeled references. Keep the output centered on one sheet; do not generate separate front, side, back, and expression images because independent calls increase identity drift.

### 5. Perform a visual QA pass

Inspect the generated image itself. Mark each item as present, weak, or missing:

- three distinct full-body views: front, side, back;
- identical hair, face, eyes, body proportions, outfit structure, shoes, accessories, and palette across the views;
- at least four readable expressions;
- hair/eyes/outfit/shoes/accessory detail areas;
- four to eight color swatches;
- clean background and a settei/model-sheet hierarchy rather than a story scene or poster;
- no unrequested characters, props, logos, or watermarks.

Identity drift is a higher-severity defect than a missing optional 3/4 view. If the page is crowded, remove the optional 3/4 view or shorten labels before removing a required view or an identity anchor.

### 6. Make at most one targeted correction

If a required element is missing or identity drift is obvious, make one edit/regeneration pass. State one highest-impact correction clearly, for example:

> Add the missing full-body back view and preserve the exact hair silhouette, eye color, outfit layers, shoes, accessories, and palette from the existing sheet and identity reference. Change only the missing back-view panel; keep all other panels and the clean white layout unchanged.

For an edit, use the generated sheet as the edit target and re-include the identity reference when available. Preserve all correct panels and the fixed identity summary. Do not start a new visual concept, run an unbounded retry loop, or create separate replacement images. If the single correction still fails, deliver the best result with a concise QA note.

## Output contract

Deliver exactly one final character-settei raster image, shown inline. When the user asked for a project-bound asset, move/copy the selected built-in output into the workspace without overwriting an existing file, and report the absolute saved path. Also report briefly:

- whether the result passed the required-view/identity QA;
- whether the one correction pass was used;
- any remaining limitation, especially if labels or a small panel are not fully legible.

Do not add a separate character bible, code pipeline, database, or external-service setup unless the user asks for it.
