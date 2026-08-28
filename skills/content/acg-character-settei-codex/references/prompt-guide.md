# Settei Prompt Guide

Use this reference to turn the normalized brief and Master Identity Summary into one built-in image-generation prompt. Keep the prompt focused on the sheet; do not write a full character biography.

## Base prompt

Replace bracketed fields with the user's actual details. Keep the invariant sentence intact and shorten optional lines when the brief is already detailed.

```text
Use case: stylized-concept
Asset type: one-page anime production character design reference sheet / settei sheet
Primary request: Create one clean landscape character sheet for the same character throughout.
Input images: Image A is the character identity reference [if supplied]. Image B is the layout/style reference for sheet composition only [if supplied]. If the tool's attached-image order cannot be changed, replace these labels with the actual image indices and roles.
Scene/backdrop: bright white or very light neutral paper background; no story scene
Subject: [Master Identity Summary]. Preserve the exact hair silhouette, face and eye features, body proportion, costume construction, shoes, accessory placement, and key colors in every panel.
Style/medium: Japanese anime model-sheet / settei aesthetic, clean thin linework, polished but readable, restrained cel shading and light paper-like accents
Composition/framing: horizontal 16:9-style page with full-body neutral standing front, side, and back turnaround views; optional 3/4 view only if space remains; compact expression row with at least four and preferably six head-and-shoulder expressions; detail callouts for hair, eyes, outfit, shoes, and accessories; four to eight color swatches
Lighting/mood: even studio-like reference lighting, calm and informational
Color palette: [the fixed palette from the brief]
Text (verbatim): use only short optional labels such as "FRONT", "SIDE", "BACK", "EXPRESSIONS", "DETAILS", and "PALETTE"; no paragraphs
Constraints: same character consistently in all views and expressions; keep the same outfit and palette across the entire sheet; prioritize identity and readable panel spacing; no watermark
Avoid: complex background, story action, poster composition, excessive effects, extra characters, unrequested props, redesigned costume, and long unreadable annotations
```

The required English anchors should remain recognizable in the prompt: `character design sheet`, `settei sheet`, `front, side, back views`, `same character consistently`, `expression sheet`, `detail callouts`, `color palette`, `clean white background`, `anime production design reference sheet`, and `full body turnaround`.

## Reference handling

Use one of these compact blocks instead of vaguely saying “use the images.”

### Identity reference only

```text
Image A is the character identity reference. Preserve the character's hairstyle,
facial features, eye color, silhouette, outfit structure, accessories, shoes, and
colors. Re-express this same character as a clean settei sheet; do not redesign her.
```

### Identity plus layout template

```text
Image A is the character identity reference. Image B is the layout/style reference
for panel arrangement, spacing, line weight, and light background only. Preserve
Image A's identity and costume exactly; borrow Image B's sheet organization without
copying its character. If the attached-image order is fixed, use the actual indices
instead of mislabeling either image.
```

### Multiple identity references

```text
Image A is the main identity anchor because it is the most complete/on-model view.
Images B and C are supporting identity references for details only. The layout
template, if present, is the next image and is composition-only. Resolve any
conflict in favor of Image A and the written brief; keep one coherent design.
```

## Sparse brief augmentation

If the user gives only a few traits, add production constraints rather than new story content:

- “Japanese anime settei/model sheet”
- “neutral standing poses for turnaround”
- “clean white background and readable panel spacing”
- “same character consistently across every panel”

Do not invent a precise eye color, accessory, pattern, weapon, name, age, or palette that the user did not imply. If an unspecified detail is necessary for a coherent image, choose a quiet generic option and mention that it was inferred only if it materially affects identity.

## Targeted correction prompts

Use the smallest correction that fixes the QA failure. Always repeat the identity invariants.

### Missing back view

```text
Edit the existing settei sheet. Add one accurate full-body back view in the open
turnaround area. Preserve the exact hair silhouette, face/eye design, body
proportion, outfit layers, closures, shoes, accessories, and palette from the
existing sheet and Image A identity reference. Keep the front and side views,
expression row, detail callouts, palette, white background, and spacing unchanged.
Change only the missing back-view panel.
```

### Costume drift

```text
Edit the existing settei sheet. Correct the side and back costumes so they match
the front costume construction exactly: [one-sentence outfit invariant]. Preserve
the character's hair, face, eyes, proportions, accessories, shoes, expressions,
details, palette, and clean layout. Change only the inconsistent costume panels.
```

### Missing expressions/details/palette

```text
Edit the existing settei sheet. Add the missing [expression row / hair and outfit
detail callouts / four-to-eight color swatches] in the unused support area. Preserve
all full-body views, the exact identity, the fixed outfit, and the white settei
layout. Change only the missing support content.
```

Never use a correction prompt to request a new pose, new outfit, new art direction, or a second character at the same time as a QA fix.
