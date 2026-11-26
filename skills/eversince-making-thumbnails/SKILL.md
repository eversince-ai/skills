---
name: eversince-making-thumbnails
description: "The one image that sells a video or post: a thumbnail, poster or cover, judged at the size it runs. An ad with copy: image ads."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-making-thumbnails"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

The one image that sells a video, a podcast or a post: a thumbnail beside a title, a poster seen across a
room, a cover the size of a stamp in a list. The image should raise a question the title answers.
Art that only shows what the content is gets passed over. Where people find the art by browsing a
category (a cover on a shelf, a tile in a directory), it should first read as that kind of thing, in
the category's conventions; curiosity is what sets it apart from its neighbors. Exaggeration is fine,
but a promise the content does not keep buys one click and loses the audience that gave it. For an
image that sells a product with copy and layout, use `skill-image-ads`. A brief, reference or brand the user gives overrides every default here.

## The plan

1. `inspect_media` {ref: the video, mode: "overview"}, then {ref, mode: "window", window: {start,
   end}} over the strongest stretch, to choose the frame by its expression; `extract_media` {ref:
   the video, mode: "frame", at_time: the moment} for the frame as an item.
2. `manage_canvases` {create: {destination: "youtube-thumbnail"}}.
3. `edit_canvas` {canvas_id: from step 2, operations: [addLayer {layer: {type: "image", ref: the
   frame, box: {x: 0, y: 0, w: 1280, h: 720}, grade: {brightness, saturation} below 0, effects: {blur}}} as
   the ground; addLayer the same picture in the same box as the subject; addLayer {layer: {type:
   "shape", shape: "rect", box: the half the words take, fill: {gradient: {kind: "linear", angle (0 runs up, 90 right),
   stops: dark at the outer edge with opacity falling to 0}}}}; addLayer {layer: {type: "text",
   text: a few words, up to about five, from the video's point (none: no text layer), size: to fit the box with the longest word whole (120 to 180 where it fits), weight: the face's heaviest, color, shadow
   or a thick outline {width, color}, box: on the third the subject leaves, clear of the bottom-right corner}}]}.
4. `edit_canvas` {canvas_id: from step 2, operations: [patchLayer {layerId: the subject layer
   from step 3, patch: {crop: on the head and shoulders, mask: {kind: "subject"}, effects: {glow:
   {blur, color, opacity}}}}; patchLayer the ground to a tighter crop, shifted]}: cloud processing. Where the
   subject mask is not wanted, the subject layer is left out of step 3 and the ground stays sharp
   under the gradient.
5. `inspect_canvas` {canvas_id: from step 2, scale: 0.25}: the one look, the size
   test, the count and the question test; a second at 0.125 where the image is for phones.
6. `publish_preview` {canvas_id: from step 2}; `render_canvas` {canvas_id, format: "jpeg"} only for uploading it.

A new ground, when wanted: `remove_background` {ref: the frame, idempotency_key} (credits), the cutout placed in
step 3 over a shape in one flat color in place of the two pictures. A built scene, when wanted:
`generate_image` {type: "image-to-image", source_refs: [the frame], prompt: the concept as a
mechanism, the person kept exactly as shown, idempotency_key}, placed in step 3 in place of the
frame.

## What the job starts from

Start from what actually happens in the content and its single most compelling fact or moment. Know
the title it sits beside, fixed or still open (if open, shape art and title together). Know where it
runs, since the format sets the rules: a thumbnail is seen 160 to 360 pixels wide in a crowded row, a
poster across a room before anyone reads it up close, a cover as a postage stamp in a list. Check
what exists: real frames or photos from the content, the faces in it, a look earlier art set.

## The concept

Settle the concept before choosing or generating any picture; a prompt written from the title
produces a depiction. Sketch a few concepts with different devices (the strong ones often combine
two), then compose one.

- **The face.** One person, large, showing real emotion. Strong for talking content, because
  faces stop eyes.
- **The moment.** One action frozen mid-happening, when the content has a real peak that reads
  without context.
- **Before and after.** The transformation split in one frame, when change is the story.
- **Scale contrast.** Something giant against something tiny, when the surprise is size, quantity or
  stakes.
- **The object as metaphor.** The idea made literal as a thing, for abstract topics with no natural
  picture.
- **The world.** The environment as the hero, with one small subject in it: travel, games, film.
- **The forbidden glimpse.** Partly hidden, cropped or turned away, when curiosity about what is not
  shown is the hook.
- **Type as the image.** The letterforms carry the tone, when the phrase is the hook or the title has
  to dominate anyway.

When the user brings a reference as "like this one", find the mechanism doing the job first (one
figure tiny against a huge flat field, a single saturated accent in a desaturated frame). That is
usually what makes it work. How close to go is the user's call.

## What a thumbnail is built from

In vidIQ's study of 500 breakout videos across 30 niches (mid-2026), 72% of thumbnails carried
overlay text, 69% a human face and 56% high contrast. That describes winners; it does not prove
cause: across 507,478 videos, Eden found faces on 84% of breakouts and 82% of underperformers, so a face is a choice, not the default. Among thumbnails that carry text, vidIQ's median is five words. For technical topics the thing itself is often the hook instead of a face.

A frame with a heading on it is a screenshot, not a thumbnail. Design from three large elements at
most: a subject, the words, and one supporting thing (an object, an arrow, a second figure to
measure against). Drop a fourth.

- **Choose the subject for expression.** Pick the frame where the face shows the video's emotion
  (surprise, doubt, delight, effort), eyes open and lit, not the sharpest or the first frame. Crop
  until the head and shoulders fill 40 to 60% of the frame's height, on one third of the frame,
  looking at the camera or toward the words. With no person, one object takes that place at that size.
- **Separate the subject from the ground.** At 160 pixels wide, a face on a busy room becomes one
  texture. Make the ground quieter than the subject: a clear step darker or lighter, less saturated,
  softer, or replaced by a flat color or a simple gradient. A thin light edge on the subject holds it
  at small size; a heavy glow or a thick outline reads as noise. Check it in black and white: where
  subject and ground share a value, color alone does not separate them.
- **Use one saturated accent on a controlled ground.** A pair far apart in hue and value (yellow on
  deep blue, white on red, orange beside teal), for the words or the supporting thing and nowhere
  else. The page around it is white in light mode and near-black in dark mode, so a pure white or
  pure black edge dissolves into one of them; a mid or saturated ground holds on both.
- **Leave the surface its furniture.** A video row stamps the duration over the bottom-right corner
  and draws a progress bar along the bottom edge; a Shorts shelf and a cover list crop the sides. Put
  the subject on one third and the words on the other, with nothing that matters in the bottom-right
  eighth or the bottom 10%.
- **A channel has a system.** The same face treatment, typeface and palette across a channel's
  thumbnails let a viewer spot it in a row. The concept changes per video; the system holds. The
  channel's existing thumbnails, if the user has them, set it.

## Words on the art

Where the title sits outside the frame (a thumbnail in a row, a card in a feed), put two to four
words on the art that add to the title: the stake, the number, the reaction, the part of the
question the title leaves open. Take them from the content's own point: a line said in it, its
title, or what the user says it is about. Footage with no speech and no stated point gets no words,
or the user's title if one is given; do not invent a line to fill space. Where the art carries its
own title (a poster, a book, an album, a podcast tile), the title dominates: build the frame with
room for it before generating anything, and size it to hold at the smallest place the art runs.

Set thumbnail words large and heavy: one or two lines, the face's heaviest weight, letters about a
sixth of the frame's height, filling a third to a half of the width. Size the words to fit their
box with the longest word whole, never broken mid-word; that usually means fewer and shorter words.
Size 120 to 180 on a 1280 by 720 slide where they fit; a long word at 130 is already wider than half
the frame. Raw footage does not hold them; a thick dark `outline`, a `shadow`, a `background`
box, or a dark band behind them holds them at small size. A thin outline (width 6) does not show. Use one color, with
one word in the accent if that word carries the point. Words are text layers on the canvas, editable
at any size. Keep them off the subject's face. Type craft is in `skill-graphic-design`.

## Making it

- **The picture.** A photograph, an illustration or a frame from the content, placed in the delivery
  frame with no generation. For a video, `inspect_media` overview reads it as a contact sheet,
  window shows the candidate seconds close enough to judge the expression, and `extract_media`
  frame pulls the chosen moment out as an item. Cropping on the canvas is free; extending the frame
  with `reframe_image` costs credits.
- **The canvas.** `manage_canvases` create at the destination (youtube-thumbnail is 1280 by 720,
  podcast-cover 3000 square, instagram-post 1080 by 1350; `get_canvas` with `destinations` lists
  them). Place the picture by ref with `edit_canvas` in a box covering the slide, its `crop` set
  on the subject.
- **Separation, three routes, the first free.** A ground treatment on the canvas: a `grade` on the
  picture, and a shape with a gradient fill (dark on the words' side, clear on the subject's)
  between the picture and the words. A sharp subject over a softened ground, cloud processing: the
  picture twice, the upper with `mask` {kind: "subject"} and a thin light `effects` glow as its
  edge, the lower blurred. If the lower repeats the subject in place it shows as a halo, so crop it
  tighter and shift it, or `grade` {brightness, saturation} (-100..100; `skill-canvas-operations`)
  low enough to lose the outline, or use a flat color or gradient. Judge the edge at `scale` 0.5
  or larger. The subject on a new ground, in credits, offered to the user: `remove_background` on
  the photo, the cutout over a flat color, a gradient or a second picture.
- **A built scene**, when the user wants one, in credits: generate at the delivery ratio from the
  start, with a real face as a reference on every generation that includes it. Run three
  explorations on a fast model and the winner on a model that holds faces (`list_models`). What
  image models get wrong is in `skill-product-photography`.
- **Two or three versions** to choose from, unless the user asked for one: each a slide on the same
  canvas (`edit_canvas` addSlide {copyOf}), each a different concept. YouTube may crop a 16:9
  thumbnail on phones, so keep the face and the words away from the far edges in every version.
- **Variants for a test** differ by concept (the face against the object, words against none), not
  by a shade or a word. YouTube compares up to three thumbnails on one video and picks by the share
  of watch time each earns, so a variant that wins clicks with a promise the video does not keep
  loses the test.

## The check, cold and at final size

- At the size it runs, in one call: `inspect_canvas` with `scale` (0.25 is a thumbnail as a
  desktop row shows it, 0.125 as a phone list does) returns the bare picture at that size. Add
  `grid: true` only to read a position off it. Its notes, measured at full size, name words that
  are cut off, overlapping, too small or too faint; no file is needed to look. At that size: the
  subject is found at once and its expression reads, the words read in one glance, subject and
  ground are two clear values, and the bottom-right corner is empty.
- The count: three elements or fewer, one clearly first.
- The question test. Title outside the frame: with the title covered, the image makes a stranger
  want to know something, and the title pays off what it opened. Title in the art: with the words
  unreadable, the image still pulls. A failure here is the concept, not the rendering.
- The honesty test: the content delivers what the art promises.

## Handover

- A video thumbnail: 1280 by 720 (destination youtube-thumbnail), the check passed on the scaled picture, shown in the app from its first media; a JPEG from `render_canvas` for uploading it.
- Also possible: two or three variants by concept for a test, the subject cut out over a new ground, a built scene around the frame, versions for Shorts and the channel page, the same thumbnail in other languages.
- A poster or a cover: the size its surface takes (`get_canvas` destinations, or the width and height named), checked at the size it is first seen, and in the app.
- Variants for a test: one size across them, each differing by concept, each in the app.
