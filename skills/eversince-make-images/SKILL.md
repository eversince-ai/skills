---
name: eversince-make-images
description: "Thumbnails, carousels, post images, posters, edited photos, product shots and AI pictures: how each is made and its skill."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-make-images"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill covers images: a thumbnail or cover, a carousel or slide deck, a post image, a poster or banner, a set of edited photos, product shots, an AI-generated picture. It lists the three ways an image is made here, the skill for each kind, and the tools behind them.

Everything below is optional; what the user asked and what the agent knows about them decide what is made. The skills are starting points, not the only way to do the work; an image no skill describes is made with the tools directly.

## Three ways an image is made

- **Composed on a canvas**: layers (pictures, text, shapes, the brand's logo) placed on one or more slides, editable later. `manage_canvases` create, `edit_canvas` for every change, `get_canvas` to read it, `inspect_canvas` to see it drawn, `render_canvas` for files (PNG, JPEG, PDF). A carousel or deck is one canvas with a slide per page. Every layer's fields: `skill-canvas-operations`.
- **Edited from a photo**: straighten, light, color, crop, a look (`skill-photo-editing`); another shape: a crop on the canvas (free), or the scene extended so the whole picture stays (`reframe_image`, credits); the background removed (`remove_background`); more resolution (`upscale_media`).
- **Generated**: `generate_image` from a description or from reference pictures, in credits (`list_models` for each model's settings and price, `estimate_cost` for a quote). Words can also be set on a canvas, where they stay editable.

Pictures can come from the user's library (`search_library`, or `search_footage` and `extract_media` for a frame inside a video), an online shop (`import_store_products`), stock (`search_stock`) or generation, as the work needs.

## Which skill for which image

- **A thumbnail, poster or cover**, judged at the size it runs: `skill-making-thumbnails`.
- **A carousel or a deck**: `skill-making-carousels`.
- **Photos of a product**: `skill-product-photography`.
- **A photograph finished**: `skill-photo-editing`.
- **An image ad**, headline and product: `skill-image-ads`.
- **Type, color and composition for any of them**: `skill-graphic-design`.
- **A brand's colors, fonts and look, set once for every piece**: `skill-brand-style`.
- **A ground, texture, gradient, pattern, light or custom mark drawn in code** (an SVG layer, vector in every export): `skill-canvas-drawings`.
- **A character kept the same across generated pictures**: `skill-ai-influencer`.

## Showing and checking

A canvas shows in the app with `publish_preview`; generated pictures show as they land. `inspect_canvas` draws a canvas (cloud processing, except through the desktop app), worth it where text placement or layout is uncertain. Changes asked are made on the same canvas; a version is another slide or a duplicate canvas. A generated picture is checked for what models slip in: a color cast (a yellow or blue tint across the frame), logos and small text warped, a place too clean and symmetrical to be real. A small fix is a one-line image-to-image edit on that picture, not a new generation. A picture that will become a video's first frame is judged for its light before it is animated: light that is wrong in the still is wrong in every second of the video.

## Proportion

The image asked for comes first; versions, other sizes and variants are offered in a line. Generation, upscaling, background removal and reframing use credits; editing a canvas is free.

## Handover

- Each image in the app.
- Also possible: the same design at other sizes, a set of variants, the frame pulled from a video as a thumbnail, a carousel from a long video, the background removed for a cutout.
