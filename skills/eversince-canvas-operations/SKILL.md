---
name: eversince-canvas-operations
description: "Reference for edit_canvas operations: every layer kind and its fields, boxes, masks, grades, slides and limits."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-canvas-operations"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Reference for edit_canvas operations: every layer kind and its fields, boxes, masks, grades, slides and limits.

# The document
- Operations are typed on "type", applied in order as one transaction: every one is checked before any is applied, so a bad field is named by its position and nothing lands half done. At most 200 operations a call.
- Coordinates are slide pixels. A box is {x, y, w, h, rotation?} (width and height are accepted): top-left corner, size, degrees around its center; it also carries flipH and flipV, set with patchLayer.
- A background is a color "#RRGGBB" or {transparent: true}.
- Layer order in a slide is draw order, bottom first; there is no z-index.
- Limits: 200 layers a slide, 50 slides, 8192px a side.

# Operations
{type:"addLayer", layer:{…}, slideId?, index?, parentId?}
- index: place in the stack; left out, on top. parentId: inside a group. slideId: defaults to the first slide.
- The layer says only what it is; any other field it holds (name, opacity, blend, mask, effects, grade, box) can be set beside these.
{type:"addLayer", copyOf, slideId?, index?}: a copy of a layer with a fresh id, in the same box as the original (so a masked copy lines up), on slideId or the first slide, not the original's slide unless slideId names it. 
{type:"removeLayer", layerId}
{type:"moveLayer", layerId, index, parentId?}: index in its list; parentId null lifts it out of a group.
{type:"patchLayer", layerId, patch}: any of the layer's fields but id and kind, merged and validated whole; null clears a field, at any depth; a box patch merges into the box, so {box:{x:40}} moves it alone. A field the kind lacks is refused with the ones it has. On a text layer the patch also takes `text` (the words as one run in the layer's style), `size`, `weight` and `font` (a family name); `runs` keeps styled spans, which is what a translation writes back.
{type:"replaceImage", layerId, ref}: a new picture in the same box, the crop reset to cover it, centered on the picture's subject where one is known, else its center (crops in the response gives it and what it leaves out, as for an addLayer picture); a picture layer, or any closed shape, which it fills.
{type:"group", layerIds, name?}: two or more layers from the same list (one slide, one parent) / {type:"ungroup", layerId}
{type:"setCanvas", patch:{width?, height?, background?, dpi?, destination?, platforms?}}: the size of every slide; platforms is where the image goes, by platform name (TikTok, Instagram, YouTube, LinkedIn and the rest; a name not on the list is refused with the list), drawn as marks under it in the app. The same image at a new size is manage_canvases create with from_canvas, which lays it out again.
{type:"addSlide", slide:{name?, hidden?, background?, layers?:[…]}} or {type:"addSlide", copyOf:"<slideId>", index?}: index counts from 0; left out, after the last. A copy carries every layer whole under fresh ids the response names; the words on a copied slide change in the next call by those ids.
{type:"setSlide", slideId, patch:{name?, hidden?, background?}}: hidden leaves it out of an export; null clears name or background / {type:"removeSlide", slideId}: a canvas keeps its last slide / {type:"moveSlide", slideId, index}

# Layer kinds on addLayer
{type:"image", ref}: a library item; its pixels are read and the picture is fitted inside the slide, or, with a box, cropped to cover the box.
{type:"text", text, preset?:"heading"|"subheading"|"body", size?, font?, weight?, color?, align?, auto_width?}: the preset (heading unless named) sets size, weight, line height and tracking for this canvas. A box wraps at its width; no box grows with the longest line, centered; auto_width overrides either. font is a family from the catalog (get_templates kind font); an unknown family or weight is refused with the ones available.
{type:"shape", shape:"rect"|"rounded"|"ellipse"|"line"|"arrow"|"triangle"|"polygon"|"star"|"frame-circle"|"frame-square"|"frame-arch"|"frame-blob", fill?, stroke?, radius?}: a frame is a shape a picture is dropped into, drawn as a placeholder until one is. fill: {color} | {gradient} | {image:{…}} | {none}; left out, #111111.
{type:"icon", icon:"Star", color?} or {type:"icon", svg:"<svg …>", color?, name?}: a Lucide name in PascalCase, or a plain SVG drawing written by the caller, which is how a gradient a shape cannot hold, a pattern, a texture, grain or a noise field is made: gradients, patterns, masks, clip paths and filters (feTurbulence, feGaussianBlur, feDisplacementMap, feColorMatrix, feBlend and the rest) all draw. Refused: script, foreignObject, event handlers, @import, and any href or url() to something outside the drawing (#id references within it are fine); up to 200,000 characters. Stored and drawn as vector, so it is sharp at every export size; currentColor is painted with color. Lucide icons are drawn with stroke="currentColor" and no fill, so a strokeless shape in an icon's svg sets its own fill. Working recipes and the full contract: skill-canvas-drawings.
{type:"chart", chart:"bar"|"row"|"line"|"area"|"pie"|"donut", rows:[["","2025","2024"],["Q1","18","12"]], colors?, legend?, labels?}: first row names the series, first column the categories, the rest numbers, a header and one row at least; a pie or donut draws the first series. On the stored layer the chart is "kind", which is what patchLayer changes.
{type:"adjustment", grade:{…}}: grades every layer below it on the slide, or only the one below with clip:true; strength 0..1 mixes it. Wheels, tone sliders and LUT reach pictures; on words and shapes only its filters land.

# Masks
mask hides, never cuts.
- {kind:"shape", shape:"rect"|"ellipse", insets:{l,t,r,b} (fractions of the box from each edge), feather (0..1 of the shorter side), radius? (0..1 corner rounding, rect only), invert}
- {kind:"gradient", angle (0 up, clockwise), from, to (0..1 along the run; the run between them is the feather), invert}
- {kind:"subject"} or {kind:"background"}: a region of the photograph, found here and stored as the picture's own transparency (kind "image"): "subject" keeps it, "background" keeps everything else. On a picture layer it reads its own photograph; on an adjustment with clip:true, the picture below. The subject is the people near the camera, as one region; a small figure is refused unless at:{x,y} (fractions 0..1 of what the box shows) names what is there: a product, a bottle, one person in a crowd. It answers about the thing at the point, a wall or the sky included. The picture must already be on the canvas (a picture added in the same call is refused: add it, then patchLayer the mask), not straightened, and an adjustment needs clip:true directly above it. Stills only. Cloud processing.
- disabled:true keeps a mask switched off.
- A word behind someone is three layers: the photograph, the text above it, and on top an in-place copy of the photograph (addLayer {copyOf}) carrying {kind:"subject"}, so the person covers the word and the room shows from the photograph below. The copy keeps the original's box; a move, a crop or a grade on one is the same change on both. Sizing and placing the word against the head: skill-video-effects.

# Every layer's own fields (patchLayer any; null clears)
- name, visible, locked, opacity 0..1, box, mask, effects
- blend: "normal"|"multiply"|"screen"|"overlay"|"darken"|"lighten"|"soft-light"|"hard-light"|"difference"
- effects: {shadow?:{x, y, blur, color, opacity}, blur?: px, glow?:{blur, color, opacity}, duotone?:{dark, light}}: shadow, blur and glow follow the masked shape; duotone reaches a picture alone.

# Fields by kind
- image: crop {x, y, w, h} in the source's own pixels, the part the box shows, stretched to the box (addLayer with a box sets it to cover; replaceImage resets it) · straighten −45..45, degrees the picture turns inside its window, positive clockwise, the window staying put and the picture growing to keep every corner covered · grade, the same grade a timeline clip takes (skill-timeline-operations lists its fields and what each number does): a filter preset at an intensity (get_templates kind filter), the knobs, the wheels, a LUT. exposure doubles pixel values at 100 and triples them with brightness 100, which leaves a photo two stops under still dark: gamma [n, n, n] and shadows lift the rest, and an adjustment layer above adds a second grade. inspect_color correction.grade is the grade that corrects a photo's exposure and cast.
- text: runs [{text, bold?, italic?, color?, size?, underline?}] · font {family, weight, style:"normal"|"italic", size} · color · align "left"|"center"|"right"|"justify" · lineHeight (a multiple) · letterSpacing (in em, a share of the size: 0.1 is wide spaced capitals, −0.02 a tight headline) · textTransform "none"|"uppercase"|"lowercase"|"capitalize" · decoration "none"|"underline"|"strikethrough" · direction "auto"|"ltr"|"rtl" · autoWidth · verticalAlign "top"|"middle"|"bottom" · list "none"|"bullet"|"number" · background {color, padding, radius, opacity} (the lozenge behind each line: the face's full height, about 1.2 × size, plus padding on every side, so it draws past a box that is shorter) · outline {width, color} · shadow {x, y, blur, color, opacity}. Text in a fixed box wraps rather than shrinking, with no warning: size it to fit, and read the drawn slide.
- shape: shape (the stored kind: rect, ellipse, arch, blob, line, arrow, triangle, polygon, star; "rounded" and the frames are presets of these) · fill ({color} | {gradient:{kind:"linear"|"radial", angle, stops:[{at 0..1, color, opacity?}]}} | {image:{…}} | {none:true}) · stroke {width, color, dash, align:"inside"|"center"|"outside"} · radius (slide pixels, one number or [top-left, top-right, bottom-right, bottom-left]) · points 3..64 (a polygon's sides, a star's points) and inner 0.05..0.95 (a star's inner radius) · caps [start, end], each "none"|"round"|"square"|"arrow"|"circle" (a line or arrow) · a shape holding a picture carries that picture's crop, straighten and grade inside fill.image.
- adjustment: grade · clip · strength 0..1.
- chart: kind · rows · colors · legend · labels.
- icon: name · svg (the drawing itself; patchLayer replaces it whole) · color.
