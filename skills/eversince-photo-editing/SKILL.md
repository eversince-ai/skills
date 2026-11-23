---
name: eversince-photo-editing
description: "A photograph finished the way a photographer would: straighten, light, color, a filter or look, looks to choose from; one photo or a set."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-photo-editing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Finish a photograph the way a photographer would: the same picture, straighter, with better light
and color and a look that suits it. Change how the picture reads, never what is in it; the person who
took it should recognize it at once. Most photos need only a few moves. If the edit is obvious, it
went too far. New product shots from a reference are `skill-product-photography`; words and layout on a
picture are `skill-graphic-design`; matching the pictures in a cut is `skill-color-grading`.
A brief, a reference or a brand from the user overrides every default here.

## The plan

1. `inspect_media` {refs: the photos} for each one's shape and what is in it, and `inspect_color` {refs: the photos}: one read, each photo's scopes and `correction`.
2. `get_templates` {kind: "filter"} where a look is wanted.
3. `manage_canvases` {create: {from_ref: the first photo of each shape}}.
4. `edit_canvas` {canvas_id, operations: [patchLayer the picture layer with straighten, a crop where asked, and grade {its correction's exposure, gain and gamma, then highlights, shadows, contrast, saturation, preset, intensity}]}.
5. `edit_canvas` {canvas_id, if_revision, operations: [addSlide copyOf the first slide once per further photo of that shape]}, then one call with replaceImage on each copy's picture layer and patchLayer its own correction, straighten and crop.
6. `inspect_canvas` {canvas_id}: the one look, the set at once.
7. `publish_preview` {canvas_id}: returns the preview the set already has, its app_url the link to hand over.

## What the job starts from

The photograph as an item. Read what is wrong from the picture and the scopes. `inspect_media`
shows the picture. `inspect_color` gives the black and white points, the crushed and blown
fractions, the warm-cool and green-magenta bias, the saturation, and `correction`: a ready-to-apply
grade that brings the still to neutral exposure and removes a cast. `refs` takes up to twelve
photos in one call. For "like this", `inspect_color` with `reference` returns the gap as grade hints.

## The edit

Everything happens on the picture layer of a canvas opened on the photo. It is free and the original
stays untouched.

1. **The canvas.** `manage_canvases` create with `from_ref` opens the photo at its own size and
   shape as the one layer. Change size or shape only for a destination the user named.
2. **Straighten and crop.** `straighten` levels a tilted horizon or a leaning vertical, usually a
   degree or two, positive clockwise. Unasked, use `crop` only to trim a distracting edge (a stray
   elbow, a bright corner) and keep the photographer's framing. When the user asks for a crop, crop
   properly: subject on a third or centered on purpose, horizon level and off the middle, empty
   space removed.
3. **Light.** Start from `correction`, which aims at average exposure. A photo that is dark or
   bright on purpose (a moody studio, night, a bright beach) should keep its mood: apply part of the
   correction, often a third to a half, and judge it on `inspect_canvas`. Exposure and brightness
   multiply pixel values: 100 is about a stop, and the two together reach three times at most, so
   lift a darker photo with gamma and shadows too. Then bring back the bright and dark ends with
   highlights and shadows, then set contrast. `inspect_color` reports blown whites and crushed
   blacks as fractions. A clipped skin highlight cannot be recovered.
4. **Color.** Remove a cast with the wheels, gain and gamma per channel, as `correction` gives
   them. Temperature and tint shift hue but leave gray as gray, so use them for taste once the
   picture is neutral. `correction` is a starting point: if its gain neutralizes the neutral areas
   but the frame still looks colored, `correction.whole_frame.gain` is the stronger number; judge
   it on `inspect_canvas`. Saturation goes last and gently. Skin goes wrong first: a saturation
   move that looks right on the sky is already too much on a face. The three wheels (lift, gamma,
   gain) push shadows, mids and highlights toward a color, which is how a warm sunlit picture keeps
   cool shadows.
5. **A look.** `get_templates` kind filter lists every preset with how it looks and what it suits:
   everyday adjustments, color film, slide film, black and white, instant, camera and cinema
   emulations. Apply a preset on the grade with an intensity, usually well under full; the other
   knobs still work on top. On a corrected picture a look reads as a photo shot on that stock; on an
   uncorrected one it reads as a filter. Pick one look for what the picture is, not to show off.

## A set

Use one canvas per shape, so every photo stays whole and each shape is one preview. Photos of one shape
(all 3:2 landscape, all 4:5 portrait) become slides of one canvas at the first photo's size; a photo
of another shape starts a second canvas at its own size. Correct the first slide, then use
`addSlide` with `copyOf` and `replaceImage` on the picture layer for each further photo. This
keeps the grade, straighten and look, and resets the crop. Per photo, change only its own
`correction` (exposure and the wheels) and straighten, so the set reads as one shoot. Several looks
of one photo work the same way: copy the corrected slide once per look, give each copy its own
`preset`, and the user chooses from one preview.

## Not here

Clarity, texture, dehaze, sharpening, noise reduction, per-color hue and saturation (HSL) and lens
corrections are not available. Contrast and the three wheels cover most of what clarity and HSL do.
A noisy or soft photo can go through `upscale_media` (credits), which re-renders detail.

## Separating the subject

A photograph on a canvas takes a mask of `kind` `subject` or `background` on `edit_canvas`, found
in the picture itself, on cloud processing. An adjustment layer
with `clip: true` and a `background` mask does the before-and-after in one operation: the room down in
exposure and saturation while the subject keeps its look. The word behind them is the same three
layers: the picture, the word, the picture again masked to the subject. The subject is the people
near the camera; `at` `{x, y}`, fractions of what the layer's box shows, makes it whatever is at that
point (a product, a bottle, one person in a crowd), read off an `inspect_canvas` frame and pointed at
the middle of the thing's own mass. It always answers, a wall or the sky included, and says how much
of the picture the mask keeps. One point is one thing; two objects are two masks on two layers. A
picture with `straighten` set is refused, since the region would sit off the subject: a photo that
needs both is leveled by its crop, or keeps `straighten` at 0.

## When the user wants more

Each of these goes beyond a photographer's finish, and most cost credits or cloud processing:

- Darken the background or lift a face on its own: an adjustment layer with `clip` and a
  `subject` or `background` mask (the mask reads the photograph: cloud processing).
- A new background: `remove_background` (credits), the cutout over a color or a second picture.
- Remove a spot, a person or an object: `generate_image` with `edit_points` or `mask_ref`
  (credits). Compare it to the original at full size before it replaces anything.
- More resolution: `upscale_media` (credits), which re-renders detail.
- A new scene around the subject: `skill-product-photography`.

## The check

Judge a photo by how it looks. One call covers the set: `inspect_canvas` draws up to twelve slides
as they will export (`scale` returns them smaller). Compare them with the originals you already saw.

- The same photo, better. Check skin first: neutral where it should be, no orange or magenta drift.
- Nothing clipped that was not clipped in the source, unless chosen.
- The horizon is level, and the framing is the photographer's or the crop that was asked for.

## Handover

- One photo or a set: each checked against its original. It is in the app from its first media; the result's app_url is the link the user opens. For use elsewhere, a JPEG from `render_canvas` (quality near 0.9).
- Also possible: other looks to choose between, a black and white version, the background calmed or the face lifted, a distraction removed, a crop for a named platform, an upscale for print.
