---
name: character-sheet
description: Create a one-page game, anime, or cartoon character sheet with consistent front, side, and back views, expressions, design details, and a color palette from a text brief and optional identity or layout references. Do not use for a single illustration or unrelated concept art.
---

# Character Sheet

Create one landscape raster character sheet that reads as production reference material. This workflow is optimized for stylized game, anime, and cartoon characters.

Prioritize:

1. character identity consistency across every panel;
2. complete front, side, and back turnaround coverage;
3. readable expressions, details, and palette areas; and
4. decorative polish.

Photorealistic people are a best-effort use case. Realistic facial likeness, anatomy, clothing continuity, and exact identity across views may be less reliable than with a photography- or identity-specialized model.

## When to use

Use this skill for requests such as:

- "Make a game character model sheet."
- "Create an anime character sheet with front, side, and back views."
- "Include an expression sheet and costume details."
- "Turn this cartoon character reference into a production turnaround."

Do not use it when the user only wants one illustration, a story scene, a poster, or general concept-art exploration without a multi-view sheet.

## Image-generation backend

In Codex, load and follow the `imagegen` skill, then use Codex's built-in image generation tool. Generate the complete sheet in one call whenever possible. For a correction, edit the generated sheet and preserve all correct panels.

In a non-Codex environment, map the generation and edit steps to either:

- an image-generation skill or tool provided by that agent; or
- an external image-generation API that is configured and authorized for that environment.

Adapt tool names, image-attachment syntax, and edit parameters without changing the identity, composition, or QA requirements below. If no image-generation backend is available, explain that the agent can prepare a prompt but cannot produce the required raster image. Never claim that an image was generated when only a prompt was created.

Read [references/prompt-guide.md](references/prompt-guide.md) before composing the image prompt. Read [references/brief-template.md](references/brief-template.md) when the request is sparse or the user asks for a fill-in form.

## Inputs and reference roles

Require at least one usable text brief or character identity reference before generation. A layout reference alone is insufficient; if it is the only input, ask the user for a text brief or character reference instead of inventing an identity.

Accept any of these combinations:

1. **Text brief only.** Normalize the description into a compact identity summary. Infer only quiet, neutral production details needed to make the sheet coherent.
2. **One character reference.** Use it as the identity anchor. Preserve hair, face, eyes, silhouette, costume construction, accessories, shoes, and colors.
3. **Several character references.** Choose the most complete and on-model image as the main identity anchor. Use the others to resolve missing details; do not average conflicting designs.
4. **Layout reference with identity input.** Use it only for panel hierarchy, spacing, line treatment, paper/background treatment, and annotation style.

When both character and layout references exist, identify their roles explicitly:

```text
Image A: character identity reference. Preserve this design.
Image B: layout reference only. Use its organization, not its character.
```

Identity has priority over layout. If the references conflict, preserve the character design and adapt the layout.

In Codex, follow the `imagegen` skill's current rules for supplying local or conversational images. Inspect local image files before generation. In other environments, use the equivalent reference-image mechanism and ensure every intended image reaches the image model.

## Workflow

### 1. Normalize the request

Extract, when available:

- age cue and gender or presentation;
- personality, mood, world, and genre;
- hair shape, length, fringe, color, and accessories;
- face shape, eye color and shape, markings, and defining features;
- body proportions and overall silhouette;
- outfit layers, construction, colors, patterns, closures, shoes, and accessories;
- requested expressions, optional three-quarter view, props, and exclusions; and
- the role of each reference image.

Do not invent a name, weapon, logo, story prop, or elaborate setting unless the user asks for it. If the brief is short but usable, strengthen the production wording internally instead of asking non-critical questions. Ask only when a missing detail would materially change the identity or output.

### 2. Fix the identity anchor

Create a short internal **Master Identity Summary** containing:

- face and head: age cue, face shape, eye color, hair silhouette, fringe, and identifying marks;
- silhouette: height or proportion cue and distinctive outline;
- costume construction: exact layers, collar, sleeves, closures, hem, shoes, and accessories;
- palette: four to eight key colors; and
- baseline personality read without changing the physical design.

Keep it to a few concrete sentences. Repeat its most important invariants in the image prompt.

### 3. Compose one image prompt

Use the structure in the prompt guide. Request:

- a single clean landscape character design sheet;
- full-body front, side, and back views in neutral standing poses;
- an optional three-quarter view only if space remains;
- at least four readable expressions, targeting six when space allows;
- hair, eye, costume, shoe, and accessory detail callouts;
- four to eight color swatches;
- the same character and costume construction in every panel; and
- a white or very light neutral background, clean linework, restrained shading, clear hierarchy, and minimal decoration.

Use only short labels such as `FRONT`, `SIDE`, `BACK`, `EXPRESSIONS`, `DETAILS`, and `PALETTE`. Do not rely on paragraphs or long annotations rendering accurately inside the image.

### 4. Generate the complete sheet

Generate the full sheet as one image. Do not create the front, side, back, and expression sections in independent calls because separate generations increase identity drift.

### 5. Perform visual QA

Inspect the generated image itself. Mark each item as present, weak, or missing:

- three distinct full-body views: front, side, and back;
- consistent hair, face, eyes, proportions, outfit construction, shoes, accessories, and palette;
- at least four readable expressions;
- hair, eye, outfit, shoe, and accessory detail areas;
- four to eight color swatches;
- a clean character-sheet hierarchy rather than a story scene or poster; and
- no unrequested characters, props, logos, or watermarks.

Identity drift is more severe than a missing optional three-quarter view. If the page is crowded, remove the optional view, shorten labels, or reduce decoration before removing a required turnaround view or identity anchor.

### 6. Make at most one targeted correction

If any required QA condition is weak, missing, or otherwise fails, or identity drift is obvious, make one targeted correction pass. This includes required structure and prohibited-content checks such as a poster-like composition, unrequested characters or props, logos, or watermarks.

Prefer an image edit that preserves all correct panels and the existing layout. Use regeneration only when the backend supports reference-constrained regeneration that can preserve those correct elements. Otherwise, do not replace the sheet with an unconstrained regeneration; deliver the best result with a concise note about the remaining limitation. Name only the highest-impact correction and repeat the identity invariants.

Do not start a new visual concept, run an unbounded retry loop, or generate a separate replacement panel. If the correction still fails, deliver the best result with a concise note about the remaining limitation.

## Output contract

When an image-generation backend is available, deliver one final character-sheet raster image and show it inline when the environment supports inline media. If the user requests a project asset, save the selected output in the workspace without overwriting an existing file and report its path.

For a generated image, also report briefly:

- whether the required views and identity consistency passed QA;
- whether the correction pass was used; and
- any remaining limitation, especially an illegible label or small panel.

When no image-generation backend is available, return the prepared final prompt only and state explicitly that no raster image was generated. Do not claim image delivery or report visual QA and correction results as completed.

Do not add a separate character bible, code pipeline, database, or external-service setup unless the user asks for it.
