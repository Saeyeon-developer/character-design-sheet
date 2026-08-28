# Character Sheet Skill

This directory contains the portable instructions for the `character-sheet` skill. The skill turns a text brief and optional reference images into one production-oriented character sheet for a game, anime, or cartoon character.

## What it produces

The default output is one landscape raster image containing:

- full-body front, side, and back views of the same character;
- four to six expressions;
- hair, eye, costume, shoe, and accessory callouts;
- four to eight key color swatches; and
- a clean white or light neutral background with short labels.

The workflow keeps the same face, hairstyle, proportions, costume construction, accessories, shoes, and palette across every panel.

## Agent compatibility

The skill is optimized for Codex. In Codex, it uses the `imagegen` skill and Codex's built-in image generation tool for both initial generation and targeted corrections.

Codex-specific tool names are not portable. Claude, custom agents, and other environments must perform the image step with either their own image-generation skill/tool or an external image-generation API that has been configured and authorized for that environment. They should translate reference-image attachment and image-edit operations to their available interface while keeping the identity, layout, and QA rules in `SKILL.md`.

If an agent has no image-generation capability, it can prepare the final prompt but cannot produce the required raster image.

## Intended scope

This skill is optimized for stylized game, anime, and cartoon character sheets. It is a good fit for production turnarounds, original-character references, expression sheets, and costume reference pages.

Photorealistic human sheets are supported only on a best-effort basis. Realistic facial likeness, anatomy, fabric behavior, and exact identity across views may require a photography- or identity-specialized model and workflow.

Do not route single illustrations, posters, story scenes, or open-ended concept exploration to this skill unless the user also requests a multi-view character sheet.

## How to use it

1. Make this directory available to the agent's skill loader, or direct the agent to read `SKILL.md`.
2. Invoke `$character-sheet` in Codex, or explicitly ask another agent to use the `character-sheet` instructions.
3. Provide a character brief and identify the role of every attached image.
4. Review the generated sheet for required views and identity consistency.

Minimal prompt:

```text
$character-sheet
Create a cartoon character sheet for an adventurous young mechanic with short
copper hair, round goggles, a blue utility jumpsuit, yellow gloves, and work boots.
Include front, side, and back views, six expressions, outfit detail callouts,
and a color palette. Use a clean white background.
```

Reference-based prompt:

```text
$character-sheet
Use Image A as the character identity reference and Image B as the layout reference
only. Preserve Image A's face, hair, proportions, outfit, accessories, shoes, and
colors. Produce one game character sheet with front, side, and back views,
six expressions, detail callouts, and palette swatches.
```

## Inputs

A useful brief can include:

- age cue and presentation;
- personality, genre, and visual tone;
- face, eyes, hair, and identifying marks;
- body proportions and silhouette;
- costume layers, closures, patterns, shoes, and accessories;
- key colors;
- desired expressions and details; and
- elements that must be preserved or omitted.

A character reference controls identity. A layout reference controls only panel hierarchy, spacing, line treatment, background, and annotation style. When the two conflict, identity wins.

## Files

- [`SKILL.md`](SKILL.md): agent workflow and output contract
- [`references/brief-template.md`](references/brief-template.md): fill-in character brief
- [`references/prompt-guide.md`](references/prompt-guide.md): generation and correction prompt patterns
