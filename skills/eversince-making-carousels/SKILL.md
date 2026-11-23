---
name: eversince-making-carousels
description: "A carousel for a feed, or a deck for a room: one idea and one visual per slide, a cover that stops the scroll, one design."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-making-carousels"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Slides on a canvas. A deck is an argument someone presents. Each slide carries one claim the
presenter could say aloud and one visual that proves it, and the headlines read in order make the
whole case. A carousel follows the same rules in a feed: one idea per slide, the first slide the
hook, the last the ask, one look across all of them. A slide should make sense without explanation.
A deck that plays on its own, narrated, is `skill-explainer-videos`; pictures cut to music are
`skill-music-videos`. A brief, reference or brand from the user overrides every default here.

## The plan

1. `get_templates` {kind: "canvas"}; one that fits is `manage_canvases` {create: {from_canvas: its id, name}} and steps 2 and 3 are skipped (addSlide copyOf or removeSlide to match the slide count, then add the ask slide if it has none and fix any fixed slide counter, such as "1 / 3", to the new count) while step 4 runs on its slides; otherwise `manage_canvases` {create: {width: 1080, height: 1440, name: the carousel's title}} for Instagram, or the destination of another platform.
2. `edit_canvas` {canvas_id: from step 1, operations: [setCanvas {patch: {background: "#RRGGBB"}}; addLayer text the cover
   line, preset heading (the presets are heading, subheading and body); addLayer shape or image for the
   cover visual; addLayer text the slide mark]} on the first slide.
3. `edit_canvas` {canvas_id: from step 1, if_revision: revision_after from step 2, operations: [addSlide
   copyOf the first slide's id from step 2's slides, once per remaining slide]}; its added list names
   every copied layer with its slide, so no read is needed.
4. `edit_canvas` {canvas_id: from step 1, if_revision: revision_after from step 3, operations: [patchLayer
   on each copied text layer with that slide's claim; replaceImage where a slide has its own picture, then patchLayer its `crop` on the subject (a template's picture is center-cropped);
   addLayer chart where a slide carries figures]}.
5. `inspect_canvas` {canvas_id: from step 1, scale: 0.5, grid: false}: the one check, twelve slides a call.
6. `publish_preview` {canvas_id: from step 1}: returns the preview the set already has, its app_url the link to hand over; `render_canvas` {canvas_id, format: jpeg} for the files to post
   only for posting the files elsewhere, in order, or {format: pdf} for one document.

## What the job starts from

Who sees it, and what they should decide or do at the end: that is why every slide exists. Where it
is seen: a room screen, a shared screen on a call, a file read without a presenter (the headlines
carry more there), or a phone in a feed. What exists: the numbers, the product, a brand template, an
earlier deck, screenshots. Take figures from the user or a source; do not invent one to fill a slide.

## The story

- Put the conclusion first, at the front of the deck and at the top of each slide. Attention fades
  as people read down, and many never reach the end.
- Make every headline a point, a full sentence someone could agree or disagree with ("Returns fell
  by a third after the packaging changed", not "Returns update"). The slide should still say
  something with its visual covered.
- One point and one picture per slide. Wanting a second chart means wanting another slide.
- The headlines alone, read in order, make the argument. A gap is a missing slide; a headline the
  argument does not need is a slide to cut.
- Show the working behind a big number on the slide: customers times what each pays, orders times
  the cut taken.
- Put backup and methodology on hidden appendix slides at the end (`setSlide` with `hidden`), so
  the deck runs without them and they are there when someone asks.
- To land a point one part at a time, use successive slides, each a copy of the last with the next
  part added. A canvas has no reveal inside a slide.

## The carousel

- Instagram takes up to 20 slides; most carousels that work have six to ten. The first is a cover
  with the promise in a few words, the last is the ask (follow, save, the link), and each slide ends
  on a reason to swipe. Instagram may show a carousel again from slide 2 to someone who scrolled
  past, so slide 2 needs a hook too.
- Size: every slide shares the first slide's ratio. Since late 2025 Instagram's profile grid shows
  posts at 3:4, so 1080 by 1440 (`manage_canvases` create with width and height) fills the most
  space; 4:5 (instagram-post, 1080 by 1350) still works. LinkedIn takes a carousel as a PDF
  document.
- One visual system: the same ground, the same type at the same sizes, a slide number or progress
  mark in the same corner. Variety comes from the content, not the layout.
- Phone sizes: keep everything the reader must read above the feed floor in `skill-graphic-design`
  (headlines from 90px and body from 32px at 1080 wide). The slide is seen on a phone at arm's length.
- From existing content (a video, a thread, an article, a transcript), keep the claims and drop the
  prose: one claim per slide, quoted figures, the source on the last slide. From a video, the stored
  words (`transcribe_media` `refs` `format: "lines"`) give the claims and its frames the pictures.

## Building it

- Start from a template: `get_templates` kind canvas lists Eversince's image templates (carousels
  for tips, quotes, before and after, a product launch, a recipe, travel, LinkedIn, a photo essay),
  then the workspace's own. Copy one that fits with `manage_canvases` create `from_canvas` and
  replace its words and pictures to get a finished design. The copy lists its sample pictures as
  `placeholder: true`, image and shape layers alike, and names its destination. Otherwise
  `manage_canvases` create at the destination: presentation (1920 by 1080) for a deck, instagram-post (1080 by 1350) or linkedin-post (1200 square) for a
  carousel. `get_canvas` with `destinations` lists the named sizes. A canvas holds up to fifty
  slides.
- `edit_canvas`: build the first slide properly, then `addSlide` with `copyOf` for every slide
  that shares its layout, and `patchLayer` the ids the response returns to change the words. Every
  slide is a copy of one design, so the set stays consistent. A text layer with a box wraps at the
  box's width; one without grows with its longest line. A line break is `\n`.
- Figures as charts: a chart layer plots from rows of numbers, so the figure stays exact and
  editable. When the numbers exist, use a chart, not a picture of one.
- Pictures, in this order: the user's own screenshots, product shots and photos, placed by ref;
  frames from their video (`extract_media` mode frame at the moment a slide's claim is said, found
  with `search_footage` or the transcript); stock photos, illustrations and icons (`search_stock`
  kind image, illustration or vector, then `import_media` url); a generated image when the user
  wants one, in credits. Show anything with words in it (a screenshot, a chart, a post, code) whole
  at its own proportions, fitted inside its frame, and check its four edges for cut-off words, logos
  or controls. Crop to fill only for atmosphere.
- Room type: at 1920 by 1080, headlines 72 to 96px, body 48px, nothing the audience must read below
  40px. The rest of type, color and composition is in `skill-graphic-design`.
- When converting something that exists (a page, an older deck, a document), keep its visual
  language unless the ask is a redesign. Break it into claims and slides; where a page revealed as
  it scrolled, each step becomes the next slide.
- A canvas has no notes field. Give what the presenter says beyond the slide (the story behind the
  number, the transition, the answer to the obvious question) beside the slide numbers, or as a
  document to present from.

## The check

Judge the slides by how they look. `inspect_canvas` covers twelve a call: it draws the real slides
and notes words cut off, off the slide, overlapping, too small or too faint. Use `scale` to see a
carousel at phone size. The notes can miss a heading over an image, so look at each slide's picture,
not only the notes. Then read once as the audience would:
every headline is a claim and the headlines alone make the argument; one point and one visual per
slide, nothing clipped or crowded; nothing the audience must read under the floor for where it is
seen; every figure traced to its source, the working shown; the main run makes sense with hidden
slides closed; a carousel's cover promises, each slide earns the swipe, the last asks.

## Delivering it

The set is in the app from its first media, and every result carries its app_url. For files used
elsewhere, `render_canvas` exports the slides to the library as PNG or JPEG images, twelve a call
(render a longer deck twelve at a time, all of it), or with `format: "pdf"` the whole canvas as
one PDF, a page per slide at the canvas's size, for a LinkedIn document carousel or a shared deck.
Presenting (full screen, next slide, a timer) happens in the editor. Keep a recurring deck (a
monthly review, a pitch that keeps changing) as a template with `manage_canvases`, so the next one
starts from its design.

## Handover

- A carousel: one size across the slides (a template's own, which can differ from the platform's; built blank, 1080 by 1440 for Instagram unless another platform or size is named; `manage_canvases` create with from_canvas and a destination resizes), every slide drawn by `inspect_canvas` and looked at, nothing noted or seen wrong, in the app as a swipeable set from its first media; images in order or one PDF from `render_canvas` for posting.
- Also possible: the same carousel at another platform's size, a PDF for a LinkedIn document post, a cover as a standalone post, a version in another language, the carousel kept as a template for the next one.
- A deck: 1920 by 1080 or the brand template's size, nothing the room must read below 40px, in the app, and a PDF when a file is wanted.
- More than twelve slides: inspect and render twelve a call until every slide is covered.
