# Canvas: drawings in code

Grounds, textures, gradients, patterns, light and custom marks drawn in code on a canvas: an SVG layer the caller writes, with complete recipes.

# What it is
A canvas layer of type "icon" carries a vector drawing. With `icon` it is a Lucide symbol; with `svg` it is whatever the caller writes, which makes it the layer for anything no other layer kind draws: a color field with grain, a painted or paper texture, a mesh of soft colors, a light band or a glow, stripes, dots or a grid, a vignette, an organic blob, a logo or a mark from the user's own SVG, a geometric ornament. It is drawn as vector, so it is sharp at every export size, and the preview and the export are the same renderer: what inspect_canvas shows is what the PNG, JPEG or PDF holds, through the desktop app and in the cloud alike.

```
{type:"addLayer", layer:{type:"icon", name:"grain", svg:"<svg xmlns=\"http://www.w3.org/2000/svg\" viewBox=\"0 0 1080 1350\">…</svg>", box:{x:0, y:0, w:1080, h:1350}, blend:"multiply", opacity:0.6}}
```

Every layer field applies on top of the drawing: box (with rotation and flips), opacity, blend, a mask, effects (shadow, blur, glow), an adjustment above it. patchLayer with a new `svg` replaces the drawing and keeps everything else, which is how a drawing is iterated.

# When to draw in code, and when not
- Draw in code: a ground that is more than one flat color or one two-stop gradient (several colors meeting softly, light falling across a field), any texture (grain, paper, canvas, brushed paint), any repeating pattern, a vignette, a shape with a noisy or torn edge, a mark the user supplies as SVG, an icon Lucide has not got, a diagram of lines and arrows.
- A shape layer instead: one flat color, one linear or radial gradient, a rounded rectangle, a line or arrow. The app's panel edits those; it cannot edit a drawing's insides.
- A text layer instead: any words. A drawing's text has no fonts from the catalog, no measuring, no translation.
- An image layer instead: a photograph, a painting, a generated picture. A drawing holds no pixels.
- A mask instead: hiding part of a layer.

# The contract
- Starts with `<svg`, carries a viewBox, up to 200,000 characters. Chromium draws it: every SVG 1.1 and SVG 2 drawing feature it supports works: paths, basic shapes, linearGradient, radialGradient, pattern, mask, clipPath, opacity, blend modes inside the SVG (`style="mix-blend-mode:multiply"`), and filters: feTurbulence, feGaussianBlur, feDisplacementMap, feColorMatrix, feComponentTransfer, feBlend, feComposite, feMorphology, feOffset, feMerge, feFlood, feDropShadow, feDiffuseLighting, feSpecularLighting, feConvolveMatrix, feTile.
- Refused, and the whole operation with it: `<script`, `<foreignObject`, event handlers (`onload=`), `@import`, and any `href` or `url()` that points outside the drawing (a web address, a file, a data URL). References inside it by `#id` are what gradients, patterns, masks and filters use, and they are fine. feImage needs an outside reference, so it is out; pixels come from an image layer under or over the drawing.
- `currentColor` is painted with the layer's `color`, so a one-color drawing (a mark, a pattern of one ink) recolors from the panel; a drawing that names its own colors ignores `color`.
- The drawing is fitted into its box by its viewBox, aspect kept (an `<img>` draws it). A full-bleed ground gives the viewBox the box's proportions (viewBox="0 0 1080 1350" for a 1080 by 1350 box), or sets `preserveAspectRatio="xMidYMid slice"` on the svg element to cover the box whatever its shape. A drawing meant to keep its shape (a mark) leaves the default and gets a box of its own proportions.
- Filters are computed at export size. feTurbulence with numOctaves above 4, or several filtered full-bleed layers on an 8192px canvas, slows the export; 2 to 4 octaves and one filtered ground a slide is plenty. A filter's region defaults to 10% beyond its element: a blur that must reach the edge sets `x="-20%" y="-20%" width="140%" height="140%"` on the filter.
- Grain and fine texture are judged at the size the image runs: inspect_canvas at scale 1 shows it, scale 0.3 averages it away. baseFrequency below about 0.3 reads as blotches, 0.6 to 1.0 as grain.

# Method
1. Decide the box in slide pixels and write the viewBox to match.
2. One idea per layer: a color field is one drawing, its grain another on top with blend multiply or overlay, a light band a third with blend screen. Separate layers can be tuned, reordered and switched off; one monolithic filter chain cannot.
3. Colors come from the user's brand or brief, or the look from `skill-graphic-design`, not from the recipe.
4. inspect_canvas after the first draw, then patchLayer `svg` until it reads; render_canvas at the end.

# Recipes
Each is a complete svg for a 1024 by 1024 box; change the viewBox and the numbers that carry the size to fit another box. Seeds are integers; a different seed is a different take of the same texture.

## 1. Film grain
A full-bleed layer over anything, blend overlay or soft-light, opacity 0.3 to 0.6. Monochrome noise around mid gray, so overlay leaves the color under it alone.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><filter id="g" x="0" y="0" width="100%" height="100%"><feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves="2" seed="4" stitchTiles="stitch"/><feColorMatrix type="matrix" values="0 0 0 0 0.5  0 0 0 0 0.5  0 0 0 0 0.5  0.9 0 0 0 0.05"/></filter><rect width="1024" height="1024" filter="url(#g)"/></svg>
```
Coarser grain: baseFrequency 0.5. Color grain: give the matrix the color's channels in place of 0.5.

## 2. Soft color field (a mesh of colors)
Several radial gradients, each a color, blurred together into one field with no banding. A ground for type, a card, a cover.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><defs><radialGradient id="a"><stop offset="0" stop-color="#2a4cff"/><stop offset="1" stop-color="#2a4cff" stop-opacity="0"/></radialGradient><radialGradient id="b"><stop offset="0" stop-color="#ff6a3d"/><stop offset="1" stop-color="#ff6a3d" stop-opacity="0"/></radialGradient><radialGradient id="c"><stop offset="0" stop-color="#8a2be2"/><stop offset="1" stop-color="#8a2be2" stop-opacity="0"/></radialGradient><filter id="s" x="-20%" y="-20%" width="140%" height="140%"><feGaussianBlur stdDeviation="90"/></filter></defs><rect width="1024" height="1024" fill="#0b1030"/><g filter="url(#s)"><circle cx="260" cy="300" r="520" fill="url(#a)"/><circle cx="820" cy="760" r="480" fill="url(#b)"/><circle cx="760" cy="200" r="380" fill="url(#c)"/></g></svg>
```
Fewer, closer colors read as premium; three far-apart hues read as a party. Grain from recipe 1 on top at opacity 0.35 hides any banding the export compresses in.

## 3. Light band or glow
A band of light across a dark ground, blend screen, or a glow behind a subject. The band is a thin rotated ellipse blurred wide.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><defs><filter id="b" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="60"/></filter></defs><g transform="rotate(-28 512 512)" filter="url(#b)"><ellipse cx="512" cy="512" rx="820" ry="70" fill="#dbe8ff" opacity="0.7"/><ellipse cx="512" cy="512" rx="600" ry="26" fill="#ffffff" opacity="0.9"/></g></svg>
```
A glow behind a subject: one circle with a radial gradient to transparent, blur 40, placed under the subject's layer at blend screen.

## 4. Textured field
A color with relief: a bump map from noise lit from the upper left, multiplied over the color, plus grain. Reads as rolled paint, linen or a surface with a grain, not as brushwork; a real painting is an image layer.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><defs><filter id="p" x="0" y="0" width="100%" height="100%" color-interpolation-filters="sRGB"><feTurbulence type="fractalNoise" baseFrequency="0.006 0.03" numOctaves="4" seed="9" result="bump"/><feDiffuseLighting in="bump" surfaceScale="10" diffuseConstant="1.2" lighting-color="#ffffff" result="lit"><feDistantLight azimuth="235" elevation="50"/></feDiffuseLighting><feColorMatrix in="lit" type="matrix" values="0.55 0 0 0 0.35  0 0.55 0 0 0.35  0 0 0.55 0 0.35  0 0 0 0 1" result="relief"/><feBlend in="relief" in2="SourceGraphic" mode="multiply" result="painted"/><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="1" seed="2" result="grain"/><feColorMatrix in="grain" type="matrix" values="0 0 0 0 0.5  0 0 0 0 0.5  0 0 0 0 0.5  0.35 0 0 0 0" result="g"/><feBlend in="g" in2="painted" mode="overlay"/></filter></defs><rect width="1024" height="1024" fill="#1a2fb8"/><rect width="1024" height="1024" fill="#1a2fb8" filter="url(#p)"/></svg>
```
The two baseFrequency numbers set the grain's direction: a smaller first number stretches the relief horizontally; equal numbers give an even stipple; swap them for vertical; wrap the filtered rect in `<g transform="rotate(-8 512 512)">` with a larger rect for a diagonal. surfaceScale raises the ridges; the matrix's 0.35 offsets keep the shadowed side from going black.

## 5. Patterns
A `<pattern>` tile repeats across the fill. One ink, so `currentColor` and the layer's color recolor it.
Stripes:
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><defs><pattern id="s" width="48" height="48" patternUnits="userSpaceOnUse" patternTransform="rotate(45)"><rect width="16" height="48" fill="currentColor"/></pattern></defs><rect width="1024" height="1024" fill="url(#s)"/></svg>
```
Dots: the same with `<circle cx="24" cy="24" r="5" fill="currentColor"/>` and no rotate. Grid: a path `M48 0H0V48` stroked 1.5 in currentColor, fill none. Checks: two 24 by 24 rects at (0,0) and (24,24). Halftone that fades: dots whose radius grows with x, written as a few hundred circles by the caller.

## 6. Vignette
Darkened edges over a picture, blend multiply, opacity to taste.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><defs><radialGradient id="v" cx="50%" cy="50%" r="72%"><stop offset="0.55" stop-color="#000" stop-opacity="0"/><stop offset="1" stop-color="#000" stop-opacity="0.75"/></radialGradient></defs><rect width="1024" height="1024" fill="url(#v)"/></svg>
```
A lift of the center instead: white stops, blend screen.

## 7. Organic or torn edge
A shape whose edge is displaced by noise: a blob, a torn paper edge, a painted swatch behind words.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024"><defs><filter id="t" x="-10%" y="-10%" width="120%" height="120%"><feTurbulence type="fractalNoise" baseFrequency="0.02" numOctaves="3" seed="7" result="n"/><feDisplacementMap in="SourceGraphic" in2="n" scale="48" xChannelSelector="R" yChannelSelector="G"/></filter></defs><rect x="120" y="360" width="784" height="300" fill="currentColor" filter="url(#t)"/></svg>
```
scale is the roughness in pixels; baseFrequency 0.005 gives long waves, 0.05 a fine fray.

## 8. A custom mark
The user's logo or a symbol as a vector: paste the path data from their SVG into a drawing with that file's viewBox, `fill="currentColor"` to paint it with the layer's color, or the file's own fills to keep its colors. The box takes the viewBox's proportions. A mark drawn this way scales to a favicon or a billboard without a pixel, and a timeline overlay or a watermark can reuse the same svg.
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 831 839"><path fill="currentColor" d="…the path data…"/></svg>
```

# Plan of calls
1. get_canvas for the size, the slide ids and the revision.
2. edit_canvas: addLayer the drawing with its box and blend, at the index that puts it under or over the right layers; several drawings in one call.
3. inspect_canvas at scale 1 for grain and edges, 0.3 for the whole.
4. edit_canvas patchLayer {svg} to adjust; opacity, blend and box as layer fields.
5. render_canvas when it reads.
