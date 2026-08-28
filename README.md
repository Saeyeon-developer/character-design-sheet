# Character Sheet

`character-sheet` is an agent skill for generating a single production-oriented character sheet from a written brief and optional reference images. It is optimized for game, anime, and cartoon characters.

The skill creates one readable landscape image containing consistent front, side, and back views, facial expressions, design details, and a color palette. It prioritizes cross-view identity consistency over decorative presentation.

## Features

- Generates a complete character sheet from a text brief
- Uses one or more character images as identity references
- Accepts a separate layout image without mixing it into the character identity
- Includes full-body front, side, and back views
- Includes four to six expressions
- Adds hair, eye, costume, shoe, and accessory callouts
- Adds four to eight key color swatches
- Performs a visual QA pass and, when needed, one targeted correction

## Compatibility

This skill is optimized for Codex because its default workflow uses Codex's `imagegen` skill and built-in image generation tool.

Other agents, including Claude and custom agents, can still use the instructions in [`SKILL.md`](skills/content/character-sheet/SKILL.md), but they must map the generation step to either:

- an image-generation skill or tool available in that agent environment; or
- an external image-generation API configured and authorized for that agent.

Tool names, reference-image attachment methods, and edit workflows differ between agents. Non-Codex agents should preserve the prompt structure and QA requirements while adapting those tool-specific steps.

## Best-fit use cases

Use this skill for:

- game character model sheets
- anime production character sheets
- cartoon character turnarounds
- original-character reference sheets
- expression and costume reference pages

The workflow is designed around stylized characters. It can be used for photorealistic people, but facial likeness, anatomy, clothing continuity, and cross-view consistency may be less reliable. A photography- or identity-specialized workflow may be a better choice for realistic human sheets.

Do not use this skill for a single illustration, story scene, poster, or general concept-art request that does not need multiple views.

## Installation

Copy the skill directory into the location your agent uses for skills, or make the agent load the included `SKILL.md` directly:

```text
skills/content/character-sheet/
```

For Codex, install or link that directory as a discoverable skill and invoke it as `$character-sheet`.

## Quick start

```text
$character-sheet
Create a game-ready character sheet for a calm teenage shrine guardian.
She has long straight black hair, a red ribbon, and a white-and-navy layered outfit.
Include full-body front, side, and back views, six expressions, costume details,
and a color palette on a clean white background.
```

With a character reference:

```text
$character-sheet
Use the attached image as the character identity reference.
Preserve the hairstyle, face, eye color, silhouette, outfit construction,
accessories, shoes, and colors. Create one character sheet with front, side,
and back views, six expressions, detail callouts, and a color palette.
```

When supplying both character and layout references, label their roles clearly:

```text
Image A: character identity reference. Preserve this design.
Image B: layout reference only. Use its panel organization, not its character.
```

## Expected output

The default deliverable is one landscape raster image with a white or very light neutral background. It should contain:

1. full-body `FRONT`, `SIDE`, and `BACK` views;
2. four to six readable expressions;
3. hair, eye, costume, shoe, and accessory details; and
4. four to eight key color swatches.

If space is limited, the required turnaround views and identity consistency take priority over an optional three-quarter view, long labels, or decoration.

## Repository contents

- [Skill instructions](skills/content/character-sheet/SKILL.md)
- [Skill usage guide](skills/content/character-sheet/README.md)
- [Character brief template](skills/content/character-sheet/references/brief-template.md)
- [Image prompt guide](skills/content/character-sheet/references/prompt-guide.md)
- [Example output](examples/character-sheet-test.png)
- [CodeRabbit configuration](.coderabbit.yaml)

## Example output

This example combines front, side, and back views with six expressions, detail callouts, and a color palette.

![Example game, anime, or cartoon character sheet](examples/character-sheet-test.png)
