# Character Sheet Prompt Guide

Use this reference to turn the normalized brief and Master Identity Summary into one image-generation prompt. Keep the prompt focused on the sheet rather than a full character biography.

## Base prompt

Replace bracketed fields with the user's details. Keep the identity constraint explicit and shorten optional lines when the brief is already detailed.

```text
Use case: stylized game, anime, or cartoon production reference
Asset type: one-page character design sheet
Primary request: Create one clean landscape character sheet showing the same character throughout.
Input images: Image A is the character identity reference [if supplied]. Image B is the layout reference only [if supplied]. If the tool fixes image order, replace these labels with the actual indices and roles.
Scene/backdrop: bright white or very light neutral paper background; no story scene
Subject: [Master Identity Summary]. Preserve the exact hair silhouette, facial and eye features, body proportions, costume construction, shoes, accessory placement, and key colors in every panel.
Style/medium: [game / anime / cartoon] production model-sheet aesthetic, clean linework, polished but readable, restrained shading
Composition/framing: horizontal page with full-body neutral standing front, side, and back turnaround views; optional three-quarter view only if space remains; compact expression row with at least four and preferably six head-and-shoulder expressions; detail callouts for hair, eyes, outfit, shoes, and accessories; four to eight color swatches
Lighting/mood: even studio-like reference lighting, calm and informational
Color palette: [fixed palette from the brief]
Text (verbatim): use only short optional labels such as "FRONT", "SIDE", "BACK", "EXPRESSIONS", "DETAILS", and "PALETTE"; no paragraphs
Constraints: same character consistently in all views and expressions; same outfit and palette across the entire sheet; prioritize identity and readable panel spacing; no watermark
Avoid: complex background, story action, poster composition, excessive effects, extra characters, unrequested props, redesigned costume, and long annotations
```

Keep these anchors recognizable in the prompt: `character design sheet`, `front, side, back views`, `same character consistently`, `expression sheet`, `detail callouts`, `color palette`, `clean white background`, `production design reference`, and `full-body turnaround`.

## Reference handling

Use one of these compact blocks instead of vaguely asking the image model to use the images.

### Identity reference only

```text
Image A is the character identity reference. Preserve the character's hairstyle,
facial features, eye color, silhouette, outfit structure, accessories, shoes, and
colors. Re-express the same character as a clean production character sheet;
do not redesign the character.
```

### Identity and layout references

```text
Image A is the character identity reference. Image B is the layout reference for
panel arrangement, spacing, line treatment, and light background only. Preserve
Image A's identity and costume exactly. Borrow Image B's organization without
copying its character. If image order is fixed, use the actual indices and roles.
```

### Multiple identity references

```text
Image A is the main identity anchor because it is the most complete and on-model
view. Images B and C are supporting identity references for missing details only.
The layout reference, if present, is the next image and controls composition only.
Resolve conflicts in favor of Image A and the written brief; keep one coherent design.
```

## Sparse brief augmentation

If the user supplies only a few traits, add production constraints rather than new story content:

- "game, anime, or cartoon production model sheet"
- "neutral standing poses for the turnaround"
- "clean white background and readable panel spacing"
- "same character consistently across every panel"

Do not invent a distinctive eye color, accessory, pattern, weapon, name, age, or palette that the user did not imply. If an unspecified detail is necessary for a coherent image, choose a quiet generic option. Mention the inference only when it materially affects identity.

## Targeted correction prompts

Use the smallest correction that fixes the QA failure. Always repeat the identity invariants.

### Missing back view

```text
Edit the existing character sheet. Add one accurate full-body back view in the open
turnaround area. Preserve the exact hair silhouette, face and eye design, body
proportions, outfit layers, closures, shoes, accessories, and palette from the
existing sheet and Image A identity reference. Keep the front and side views,
expression row, detail callouts, palette, white background, and spacing unchanged.
Change only the missing back-view panel.
```

### Costume drift

```text
Edit the existing character sheet. Correct the side and back costumes so they match
the front costume construction exactly: [one-sentence outfit invariant]. Preserve
the character's hair, face, eyes, proportions, accessories, shoes, expressions,
details, palette, and clean layout. Change only the inconsistent costume panels.
```

### Missing expressions, details, or palette

```text
Edit the existing character sheet. Add the missing [expression row / hair and outfit
detail callouts / four-to-eight color swatches] in the unused support area. Preserve
all full-body views, the exact identity, the fixed outfit, and the clean white layout.
Change only the missing support content.
```

Never combine a QA correction with a new pose, outfit, art direction, or second character.
