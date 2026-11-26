---
name: eversince-product-photography
description: "Photos of a product from the real product, finished or generated: packshot, cutout, lifestyle, detail, scale, family."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-product-photography"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Product photography generated from a real image of the product. The product must stay the
product: if the label, proportions or materials drift, the shot is worthless however good the light.
Where real photos already contain the shot, finish them (a clean edge, a corrected ground, a grade
into the set) and generate only what was never photographed. Correcting an existing photograph is
`skill-photo-editing`; a still with a headline and a layout is `skill-image-ads`. A brief,
reference or brand from the user outranks every default here.

## The plan: a packshot and a few settings from the product's photos

1. Find the best real photos: `search_library` or `get_items`, then `inspect_media` {refs} to pick the cleanest, label readable
2. The packshot: `remove_background` {ref} (credits) for a clean cutout, then `manage_canvases` {create: {destination}} with the cutout on a plain ground and a soft shadow (`effects` shadow); or `generate_image` {type: "image-to-image", source_refs: [the photo], prompt: the ground and light, "the product stays exactly as shown"}
3. Each setting: `generate_image` {type: "image-to-image", source_refs: [the photo], reference_refs: [a photo of the scene or the look, where there is one], prompt: one fixed light-and-grade block plus this shot's angle and place}; `get_jobs` returns the image
4. Check each against the real photo: `extract_media` crop on the label in both, compared side by side
5. `publish_preview` for a canvas; generated images land in the library and in the app

## What the job starts from

Start from the best real images of the product: the cleanest shot, any angle coverage, the label
readable. Generate from these, never from a description. With no photograph at all (a product still
in design, artwork without a made unit), first make one canonical image of it and use that as the
reference from then on. Generate each variant from its own photograph: a color or finish generated
from another variant's picture comes back plausible and wrong, with nothing in the picture to show
it. A brand kit, if the workspace has one, fixes the colors, fonts and words around the set.

## The shot list

Pick each shot for a reason, not one of each by habit.

- **Packshot.** The product alone, studio light, clean background, wherever it must simply be legible.
- **Cutout.** Clean edges on a transparent ground, for placing onto other things.
- **Lifestyle.** The product in a real place, in use or in reach. Viewers read everything in the
  frame as part of the purchase, so a prop that could plausibly be in the box (a stand, a lid, a
  second unit) is either in the box or out of the shot.
- **Detail.** Close on the material, texture, mechanism or label, where quality or ingredients make
  the case.
- **Scale.** The product beside something that shows its size, where photographs mislead.
- **Family.** The range or multipack in one frame, sizes on one baseline at one angle and one camera
  distance. A range shot one unit at a time and assembled afterward compares nothing.

Angles: straight-on at eye level is catalog-honest and flat. Three-quarter shows volume and two
faces of the pack; it is the angle that sells. Top-down suits layouts and sets but kills height. A
low angle looks monumental and distorts small products. Keep one light and one grade across a set so
it reads as one shoot; vary angle and distance, not the world. The smallest set for a listing is a
packshot and a cutout; add a lifestyle shot where the destination shows one.

## What generated images get wrong

These are corrections, not preferences. `list_models` carries each model's own guidance and what it supports.

| The default | Why it fails | Instead |
|---|---|---|
| Describing the product's shape, colors or label text in the prompt | The reference already carries it; describing it invites a redraw, and the label comes back subtly wrong | Name the product plainly, pass the real image, say the product stays exactly as shown |
| Editing without a keep-list | Unrelated regions drift on every pass, usually the label | State what must not change on every edit |
| Plain prompts for studio work | Generic words give generic light | Photographic language: lens, light quality, surface |
| A reflective or transparent product treated like a box | Glass, chrome and clear plastic render as plastic, with reflections that show nothing of the scene | Name the material and what its surface shows (the room's light, the surface it stands on), and check the reflections in the result |
| One prompt re-described per shot of a set | Each generation re-decides the world; six shots come back as six shoots | One fixed block for light and grade; vary only angle, distance and scene |
| A lifestyle scene from words alone | A scene with no photographic anchor comes back as stock | A reference for the scene too: a look image or a real photo of the setting |
| Packaging loosely described | Empty pack space fills with invented badges, seals and award text | State what the pack carries, and check the result for anything that was never on it |
| Describing a real face | The likeness drifts per generation | The real photo as a reference on every generation that includes the person |
| A busy scene with everything named | Detail reads as noise at a tenth of the size | One subject, one background idea, empty space left empty |
| Regenerating to change the crop | A new generation re-decides everything | A tighter crop is `extract_media` crop, free; a wider frame is `reframe_image`, whose output is a new generation |
| Text described rather than quoted | Comes back paraphrased or misspelled | Exact words in double quotes with the typography described separately, or the words as a text layer on a canvas |

## Making it

- Pass the product image as the source on every generation (`generate_image`, `source_refs` for
  the image the model works from, `reference_refs` for a scene or look). Fix a weak source (low
  resolution, busy background) first, on credits: `remove_background` for a clean cutout,
  `upscale_media` before detail work. An upscale re-renders fine detail, so read the label text again after.
- To change only some spots, use `edit_points` on `generate_image` (on models that support it);
  read the points off `inspect_media` with `grid`.
- Explore composition on a fast model, then commit on one that holds fine detail and label text
  (`list_models`). Fix a set's light-and-grade block once and generate every shot from it.
- Ratios: a tighter crop is `extract_media` crop; a wider frame is `reframe_image`, which needs the
  same fidelity check as any generation. For a ratio neither reaches, generate at that ratio from the start.
- A listing image with a callout, a banner or a carousel card goes on a canvas, with type from
  `skill-graphic-design` (`manage_canvases` at the destination's size: amazon-main, shopify-product,
  etsy-listing, instagram-post and the rest `get_canvas` lists with `destinations`). Place the shot
  by ref and the words as text layers. It is in the app from its first media; `render_canvas` makes a file for use elsewhere.

## A whole catalog

`import_store_products` brings a shop's products in from its URL (Shopify, WooCommerce, any shop
publishing product data). Each product's images are filed by the store's categories, described in
the shop's own words, with Product and Product ID fields, so `get_items` returns one product's photos
as a set. `dry_run` first returns the count and whether it fits the storage left. The import is a
job; importing again fetches only what is new. For several products, run the chosen shot list over each set.

## A store listing

The media a product's store pages need, as one consistent set: a new product, a seasonal refresh, a promotion, a catalog brought in line. The store and the user decide what the listing takes: one main image, a gallery, a video (a demo, a product film, or the images arranged as a video), a GIF.

Read the store's rules (each image's size, shape, background and file type, a video's length and format, what a main image may not carry) before making anything. `get_canvas` destinations has them where it lists the store (amazon-main 2000 square, shopify-product 2048 square, etsy-listing 2000 by 2500, and the rest); otherwise use what the user states or the store's current page. With no store named and nobody answering, make the set square at 2048 (shopify-product), a default named in the handover. A white ground is a rule only where a store has one, not a default. Amazon's main image, as sellers apply it: pure white (255, 255, 255), the product filling about 85% of the frame, no text, badges or watermark. A store that requires the real product in its main image (Amazon does) gets a photograph of it there, cut out and placed; a generated scene goes in the gallery.

Use what the user wants from: their own photographs, brought in from wherever they are (`import_media`, `import_store_products`); finishing with light, color and one grade (`skill-photo-editing`); layout, text and badges on a canvas where the store allows them (`skill-graphic-design`); the shots above from the real product; a video from footage or the shots (`skill-product-videos`), a demo (`skill-screen-recording-editing`), a GIF (`render_timeline` format gif). Before handover, check each store's rules on the result (size and shape, a main image's ground in its corners with `inspect_color`, nothing the store refuses). An image that breaks a rule is named in the handover.

## The check

- Fidelity at full size: compare the label region of the reference and the result side by side
  (`inspect_media` on both; proportions, color, cap, seams). Even the best models get a product
  exactly right in only about a quarter of generations (Photoroom's 2026 benchmark of 850
  products), so plan two to four takes per shot and keep only those that pass. If none passes, go
  back to the reference, not to a patch prompt.
- Does it sit in the scene: a contact shadow where the product meets the surface, light on the
  product from the same direction as the scene's, reflections on glass or metal showing that room.
  If not, it looks composited; go back to the scene reference.
- Against its use: the cutout on a colored ground, the scale shot at the size it runs, the packshot
  against the destination's background and frame rules.

## Handover

- A single shot: passed the fidelity check at full size, at the ratio its use takes.
- Also possible: a cutout or an upscale of each photo; generated lifestyle, scale or seasonal shots from the real product; a whole catalog run the same way; a store listing as a set (images, video, GIF), as above.
- A catalog: every product run. Name any product with no usable photograph instead of generating it from a description.
