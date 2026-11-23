---
name: character-sheet-simple
description: Generate a single text-free character sheet with a straight-on neutral face close-up and matching front and rear body views cropped from the base of the neck to the feet. Use for minimal three-view references of one character.
---

# Character Sheet Simple

Create one wide landscape raster image containing exactly three consistent views of one character. The image is a clean visual reference, not an annotated design document.

## Required output

Use this fixed left-to-right composition:

1. **Left, about 40%:** one detailed, straight-on face close-up. Show the entire face and hairstyle with the head facing the camera, eyes looking straight ahead, and a neutral expression with a relaxed, closed mouth.
2. **Center, about 30%:** one straight-on front body view framed from the base of the neck down to both feet. Keep the shoulders and both feet fully visible.
3. **Right, about 30%:** one straight-on rear body view with the same crop, pose, scale, clothing, and equipment as the front view.

Crop both body views at the base of the neck. Keep the head, face, and upper neck outside those frames. The crop should read as the edge of a photograph.

Use a wide landscape canvas and a unified, plain neutral studio background. Keep the three areas visually readable without captions or decorative framing.

The generated image must contain no visible text of any kind. This includes titles, labels, captions, letters, numbers, logos, signatures, and watermarks.

Do not add:

- a side view, three-quarter full-body view, alternate pose, or extra character depiction;
- an expression grid or additional face thumbnails;
- a color palette, swatches, diagrams, detail callouts, measurement marks, or annotations;
- detached clothing, weapons, accessories, or other separately displayed props; or
- a story setting, environmental scene, ornamental border, or poster treatment.

User-requested worn gear or carried items may appear as part of the front and rear body views. Do not display them again as separate objects.

## Inputs

Require at least one usable text brief or character identity reference. A useful input establishes enough of the character's face, hair, body, clothing, palette, and style to produce a coherent identity.

- With a text brief, preserve the supplied traits and infer only quiet details necessary for coherence.
- With a character reference, treat it as the identity anchor. Use its rendering style only when the user has not supplied a separate style or layout reference. Always preserve the face, hair, proportions, skin tone, and other appearance traits the user wants fixed. Preserve the referenced clothing, colors, and equipment unless the user explicitly asks to replace or omit them.
- When the user wants only the character's appearance and body shape, treat referenced clothing and minor accessories as non-binding. Apply the requested replacement outfit or omissions, then keep that final design consistent across all three views.
- With multiple references, honor user-assigned roles first. Use an explicitly assigned identity reference as the primary identity anchor; if none exists, use the text brief; if neither exists, use the most complete role-unassigned character reference as a fallback. Never promote a layout, style, outfit, or supporting reference to the identity anchor.
- A layout-only reference does not define a character. If no character brief or identity reference exists, ask for one instead of inventing the subject.

Ask a question only when missing information would materially change the character. Do not invent a name, logo, weapon, accessory, or elaborate setting.

## Generation workflow

1. In Codex, load and follow the `imagegen` skill and use the built-in image generation tool by default. In a non-Codex environment, map generation and editing to an available image tool or an external image-generation API that is configured and authorized for that environment.
2. Inspect local reference images as required by the active backend, and label every supplied image by role in the generation prompt. Adapt tool names and image-attachment syntax without changing the identity, composition, or no-text requirements.
3. Form a compact internal identity anchor covering the face, hairstyle, build, clothing construction, key colors, and requested equipment.
4. Generate the complete three-panel sheet in one image-generation call. Do not generate or assemble the three views independently because that increases identity drift.
5. Preserve the user's requested or referenced visual style. If no style is supplied, use a clean production character-reference aesthetic with restrained, even studio lighting.

The generation prompt must explicitly repeat these invariants:

```text
Create one wide, text-free landscape reference image with exactly three views of
the same character. Left (40%): detailed straight-on face close-up with the entire
face and hairstyle visible, eyes looking straight ahead, and a neutral expression
with a relaxed, closed mouth. Center (30%): straight-on front body view. Right
(30%): straight-on rear body view. Frame both body views from the base of the neck
to the feet, with the shoulders and both feet visible. Use a clean camera crop at
the top of each body view, leaving the head, face, and upper neck out of frame.
Match the body views in pose, scale, build, clothing construction, colors, and
equipment. Use a unified plain neutral studio background. No text, letters,
numbers, labels, logos, watermark, side view, extra pose, expression grid, palette,
callout, detached prop, border, or story scene.
```

Add the user's identity and style details to this structure without weakening or expanding the three-view contract.

## Visual check and correction

Inspect the generated image before delivery. Confirm that:

- there are exactly three character depictions in the required order and proportions;
- the left panel shows the entire face and hairstyle straight on, with a neutral expression;
- the center view faces forward and the right view faces directly backward;
- both body views are cropped at the base of the neck, with shoulders and feet fully visible and no head, face, or upper neck in frame;
- both body views match in pose, scale, build, clothing, colors, and equipment, and the face close-up matches the character's identity; and
- no text, extra view, separate prop, callout, palette, or environmental scene appears.

If any required visual-check condition fails, including visible text, an extra depiction, an incorrect or missing view, a front/back mismatch, a close-up mismatch, an order or proportion error, or an identity, color, or equipment inconsistency, make at most one targeted image edit. Preserve all correct areas, identify only the failing area, and repeat the invariants above. Do not start a different visual concept or create replacement panels separately.

If the corrected image still has a limitation, deliver the best image and mention only that limitation briefly.

## Delivery

On success, show the final raster image inline when supported and add only a short completion notice. Do not include the generation prompt, character analysis, QA checklist, or other supporting text unless the user asks for it.

If the user requested a project asset, follow `imagegen` save-path rules in Codex and the active backend's configured storage and path-reporting rules elsewhere. Avoid overwriting an existing file and include the saved path in the brief notice.

If no image-generation backend is available or generation fails, report the failure concisely. Do not substitute a prompt or text-only character sheet and do not claim that an image was produced.
