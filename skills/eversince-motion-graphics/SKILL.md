---
name: eversince-motion-graphics
description: "Graphics as code on the timeline, Remotion JSX or HyperFrames HTML: titles, lower thirds, charts, kinetic type, scenes."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-motion-graphics"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A motion graphic is code drawn on a graphics track over the cut: a title, a lower third, a
chart, a logo reveal, kinetic type, or a whole explainer built from scenes. `compose_motion` places
it from Remotion JSX, from a HyperFrames HTML document, or from a ready graphic on the shelf. It
draws over whatever sits below it, footage or nothing. Captions over speech are
`skill-animated-captions`; type, color and composition are `skill-graphic-design`; a still made of layers
is a canvas.

## The plan

A graphics-only piece with several sections, once the brief is settled. Ids in later steps come
from earlier responses. Over footage, the ground in step 3 is left out and the sections sit
transparent over the footage.

1. get_timeline {timeline_id, response_format: "concise"} for the frame rate, size and what sits where
2. detect_beats {ref: the music item} when the piece cuts to music
3. compose_motion {timeline_id, source_code: the ground (background, grain), placement: {from_seconds: 0, duration_seconds: the whole piece}}
4. inspect_timeline {timeline_id, seconds: [one settled frame of the ground]}
5. compose_motion {timeline_id, source_code: section 1, placement: {from_seconds, duration_seconds}}
6. inspect_timeline {timeline_id, seconds: [settled, the signature move, either side of a cut]}
7. compose_motion and inspect_timeline once per remaining section, each placed by its seconds
8. check_timeline {timeline_id}; its warning about a graphic over black is expected on a graphics-only piece
9. inspect_timeline {timeline_id, span: {start, end}} over the piece, up to 60 seconds a call, for the arc as one strip and the stranger's read
10. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over; render_timeline {timeline_id, idempotency_key} for a file used elsewhere

Two overlays on an existing cut: a lower third, and a stat card timed to a spoken figure.

1. get_timeline {timeline_id, response_format: "concise"} for the fps and where the talking head sits
2. get_transcript {timeline_id, granularity: "words"} for the frame of the spoken figure
3. get_templates {shelf: "Lower thirds"} and {shelf: "Stats and progress"} for the closest cards
4. compose_motion {timeline_id, template: the lower third's id, values: the card's controls for the name and the title, placement: {from_seconds: the clip's start plus 0.5, duration_seconds: 5}}
5. compose_motion {timeline_id, template: the stat card's id, values: the card's controls for the figure and its suffix, placement: {from_seconds: the word's frame over fps, minus 0.3, duration_seconds: 4}}
With no spoken figure, step 5 places the stat card on the line that makes the point, or the card is left out.

6. inspect_timeline {timeline_id, seconds: [a settled frame of each, the count landing]}
7. check_timeline {timeline_id}
8. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over

## The parts

- `skill-motion-planning`: a piece with scenes. Sections as timed scenes, the 22 shot plans by
  role, cuts between scenes inside one graphic, and choosing transitions inside a graphic and
  between clips.
- `skill-motion-overlays`: graphics over footage. Which kind a request is; lower thirds, callouts,
  social cards, quote and stat cards; the short-form rules.
- `skill-motion-recipes`: the index of named moves (a count, a typewriter, a slot roll, a camera
  leg, a morph, a click), each with its numbers and where it exists: a runtime helper, a shelf
  graphic, or a mechanic you write by hand.

A single title card from the shelf needs none of them. For a graphic over footage, open the
overlays part for its kind's numbers. For a piece with sections, open the planning part before the
first section.

## The shelf

`get_templates` with `query` and `kind: "graphic"`, or `shelf` by name, lists ready graphics on its
shelves; `get_templates` with nothing names the shelves. The workspace's own saved graphics come
first. Every card measures the frame it lands in, so it fits wide, vertical and square. A `ground`
control usually paints the whole frame over the footage; one labelled Card color colors only the
card. Reading one by id returns its controls and the `compose_motion` call that places it; an
`image` or `video` control takes a library item's ref in `values`; `code: true` adds its reviewed source to build from. Once
placed, a shelf graphic is the clip's own code.

Each shelf graphic carries its own type, position and entrance. A piece of several beats reads as
one designed piece when its graphics share one display face and one text face, one ink and one
accent, one zone of the frame for words and one entrance, set in the spec before the first graphic.
Shelf graphics then go in as parts of that system: their controls set to it, and a font, position
or entrance that differs changed in their code (`source_edits`). Four cards placed beat by beat in
their own styles read as four ads.

## The contract

The compiler enforces these; code outside them fails at compile or draws nothing.

- **One component named `DynamicAnimation`**: plain JSX, no imports, no exports, no TypeScript.
- **In scope, and nothing else**: React with useState, useMemo, useRef, useEffect · AbsoluteFill,
  Sequence · useCurrentFrame, useVideoConfig · spring, interpolate, interpolateColors, Easing · Img,
  Video, OffthreadVideo · Icons (the lucide set, as components) · Shapes (Circle, Ellipse, Rect,
  Triangle, Star, Pie, Heart, Polygon) · LightLeak · Layout (measureText, fitText, fitTextOnNLines,
  wrapText, fillTextBox) · Paths (evolvePath, interpolatePath, getLength, getPointAtLength and the SVG path
  tools) · Notation (Underline, Highlight, Circle, Box, Bracket, StrikeThrough, CrossedOff) ·
  useSound · Camera, Cursor, Streak, Grain · ThreeCanvas, THREE, useTexture (3D) · Geo (real maps:
  `Geo.view({ width, height, focus })` fits the world or named countries to the frame and gives
  `paths` (every country's SVG d), `path(name)`, `center(name)`, `box(name)` and `point(lon, lat)`; `Geo.city(name)`,
  `Geo.arc(view, from, to)` a flight curve) · durationInFrames
  (the clip's length) · `controls` (the graphic's knobs). A name the code declares shadows the scope's.
- **Blocked anywhere outside a comment**: fetch, XMLHttpRequest, WebSocket, Worker,
  any name ending in Loader, require, importScripts · document, window, localStorage, sessionStorage · eval, Function ·
  setTimeout, setInterval, Date, Math.random, performance.now, crypto, THREE.Clock, MathUtils.rand* ·
  process, globalThis, self, Reflect, Proxy, prototype access · requestAnimationFrame, CSS
  transition and animation keys, `@keyframes`, raw `<video>` `<audio>` `<style>` `<animate>`. The scan is textual: a headline containing "process", "document" or "window" fails
  the same way and is rephrased.
- **Every frame is computed from `useCurrentFrame()`**; frame 0 is the clip's first frame, not its
  place on the timeline. A frame can be drawn first, last or alone (the preview seeks; a cloud render
  starts each chunk cold), so state and effects hold only what is worked out once: a measured
  layout, a table built from the controls.
- **Time is seconds times `useVideoConfig().fps`**, never a bare frame count, or the piece plays at
  the wrong speed on another rate.
- **Every ramp says where it stops**: `interpolate` extends past its last frame unless
  `extrapolateRight: 'clamp'` is set. The input range climbs and both lists match in length, or the
  compile is refused. The last frame drawn is `durationInFrames - 1`; a ramp ending on
  `durationInFrames` never shows its end state.
- **Knobs are `CONTROLS`**: a top-level list of plain literals, read off the parsed source without
  running it. Types: `text` (up to 500 characters), `color` (#rgb or #rrggbb), `number` (min,
  max, step), `boolean`, `select` (options, the value among them), `image` ('' in the code, a
  library ref in `values`, drawn as `{controls.logo ? <Img src={controls.logo} /> :
  null}`), `video` (the same for footage, drawn as a muted `<OffthreadVideo>` in its own
  Sequence from the first frame, its sound left to the clip on the timeline). Up to twelve (a thirteenth reads undefined), labels up to 40 characters, read
  as `controls.name`. Each is an inspector field and a `values` entry for `compose_motion` or
  `edit_timeline` updateClip.

  ```jsx
  const CONTROLS = [
    { name: 'headline', label: 'Headline', type: 'text', value: 'Doors open' },
    { name: 'accent', label: 'Accent', type: 'color', value: '#ff5252' },
    { name: 'size', label: 'Size', type: 'number', value: 96, min: 24, max: 240, step: 2 },
  ];
  ```

- **Type is sized by measuring**: `Layout.fitTextOnNLines({ text, maxLines, maxBoxWidth,
  fontFamily, fontWeight, maxFontSize })` returns the biggest fitting size and its lines;
  `wrapText({ text, fontSize, maxWidth, fontFamily, balance })` the lines at a set size; both break
  Chinese and Japanese between characters. `fitText({ text, withinWidth, fontFamily })` is one
  line; `measureText` returns `{ width, height }`. Measure in the family, weight and
  letterSpacing the text is drawn in; widths are frame pixels. `Type.fit({ text, size, maxWidth,
  maxLines, steps, fontFamily, fontWeight })`, which the shelf graphics use, sets type in steps (the
  size, then about 0.72 and 0.52 of it, cut with an ellipsis past the last) with balanced lines.
- **The fonts are the catalogue's, spelled exactly**: Sans: Montserrat, Inter, Poppins, DM Sans, Manrope, Outfit, Plus Jakarta Sans, Rubik, Nunito, Quicksand, Raleway, Lato, Open Sans, Roboto, Archivo, Space Grotesk, Sora, Figtree, Lexend, Urbanist, Work Sans, IBM Plex Sans, Geist, Instrument Sans, Albert Sans, Red Hat Display, Hanken Grotesk, Schibsted Grotesk, Bricolage Grotesque, Inter Tight, Onest. Display: Oswald, Bebas Neue, Anton, Barlow Condensed, Big Shoulders, League Gothic, Antonio, Fjalla One, Teko, Saira Condensed, Unbounded, Syne, Archivo Black, Dela Gothic One, Chakra Petch, Orbitron, Russo One, Righteous, Bungee, Alfa Slab One. Serif: Playfair Display, DM Serif Display, Fraunces, Instrument Serif, Cormorant Garamond, Libre Baskerville, Lora, Merriweather, EB Garamond, Crimson Pro, Newsreader, Young Serif, Bodoni Moda, Abril Fatface, Gloock, Prata, Spectral. Mono: JetBrains Mono, Space Mono, IBM Plex Mono, Fira Code, Roboto Mono, DM Mono, Geist Mono. Handwritten: Caveat, Permanent Marker, Pacifico, Dancing Script, Kalam, Satisfy, Great Vibes, Rock Salt. Hebrew, Arabic, CJK: Heebo, Assistant, Noto Sans Hebrew, Cairo, Tajawal, Almarai, Noto Sans Arabic, Noto Sans SC, Noto Sans JP, Noto Sans KR. Any other family
  draws in a fallback with nothing reported.
- **Ready-made pieces**: `<Camera legs={[{ at, dur, focus, scale,
  ease, blur }]}>world</Camera>` is the one camera, each leg from wherever the camera is, `focus`
  the world point brought to the center in the frame's pixels as laid out, a value a leg leaves out
  holding, `blur` peaking at the leg's fastest moment; `Camera.at(t, legs, width, height)`
  returns the same numbers with `toScreen(x, y)`. `<Cursor moves={[{ at, to: [x, y], dur, ease }]}
  clicks={[t]} from exit size tone color ripple label />` is the pointer: in from an edge (bottom by
  default), 0.75s entrance, 0.6s between targets, a 1:2 press, no ring unless `ripple` (true or a color) asks for one, out by the
  nearest edge; `Cursor.press(t, clicks)` compresses what it clicks on the same curve;
  `Cursor.at(t, props, width, height)` is the tip. `<Streak progress from={[dx, dy]} copies
  spacing>` brings an element in fast with fading copies. `<Grain amount size animated seed />`
  lays film grain over what is under it (amount 0.12 by default, 24 pattern changes a second,
  `animated={false}` for a still texture); an adjustment clip's effects never reach a graphic, so
  this is the one way to grain one.
- **A line that draws itself is Paths**: `Paths.evolvePath(progress, d)` returns the
  `strokeDasharray` and `strokeDashoffset`; `Paths.interpolatePath(progress, from, to)` morphs
  one shape into another; `getLength` and `getPointAtLength` put something travelling along a path.
- **A word marked by hand is Notation**: `<Notation.Underline progress={p} color strokeWidth
  padding iterations roughness seed>word</Notation.Underline>`, the same for Highlight, Circle (with
  `box: 'around'`), Box, Bracket, StrikeThrough, CrossedOff; the mark wraps its children and draws
  on as `progress` runs 0 to 1, seeded so every render is the same.
- **A graphic that moves to sound reads `useSound()`**: the clip sounding under the graphic, per
  frame, as `bands` (32 values bass to treble, 0 to 1), `level` and `waveform` (64 points, -1 to
  1); `useSound({ bands: 48, points: 128 })` asks for more (bands up to 128, points up to 512);
  `{ follow: 'voice' }` follows the speech. Where nothing sounds every value is 0, so the resting
  shape still reads as finished; the song is on the timeline before the graphic is composed to it.
- **All of CSS draws** in the preview, the desktop app and the cloud: 3D (`perspective`,
  `rotateX`, `rotateY`, `transformStyle: 'preserve-3d'`), `mixBlendMode`, `backdropFilter`,
  `zIndex`, `writingMode`, SVG filters. A sibling carrying a transform or a position paints above
  one carrying neither whatever the order, so every layer of a stack gets `position: 'relative'`
  or an explicit `zIndex`.
- **Real 3D is three.js through React Three Fiber** inside `<ThreeCanvas>`, which fills the frame
  by default with a transparent background, so footage under it shows: `<ThreeCanvas camera={{
  position: [0, 0, 6], fov: 40 }}><ambientLight intensity={0.6} /><directionalLight position={[3, 4,
  5]} /><mesh rotation={[0, t, 0]}><boxGeometry args={[2, 2, 2]} /><meshStandardMaterial
  color="#ff5252" /></mesh></ThreeCanvas>`. Every three.js object is an element by its camelCase
  name; `THREE` is three.js for values (`THREE.Shape`, `THREE.Color`, `THREE.MathUtils`)
  without its loaders and renderers. Motion comes from `useCurrentFrame()` as everywhere else;
  `useFrame` and the scene's own clock are not in scope. A picture is `const map =
  useTexture(controls.photo)` called inside a component rendered within the canvas, then
  `<meshBasicMaterial map={map} />`; it is null until loaded and the render waits for it. Text in a
  3D graphic is HTML over the canvas; footage in perspective is CSS 3D on `Video`, not a texture.
  A 3D graphic draws in software in a cloud render, so a few lights and simple meshes keep it quick.
- **An overlay leaves the frame alone**: a background on the outer `AbsoluteFill` covers the
  footage for the clip's whole run, right for a title card, wrong for a lower third or a callout.
- **No props arrive at runtime**: every value is in the source; what changes later is a knob, or
  `compose_motion` again with `clip_id`.

**What compile reads back.** Two things refuse: an `interpolate` input range that does not climb,
and input and output lists of different lengths. Everything else compiles and returns as a note
on the response: a ramp with no clamp · an animation timed past the clip or ending on
`durationInFrames` · a full-frame fill over footage · text under 24px at 1080 lines (scaled with
the frame) · a font family the renderer lacks, or no `fontFamily` (the default serif) · bare frame
numbers with no `useVideoConfig` · a move starting on frame 0 · a spring with no config or damping
under 15 (it overshoots about 16% and swings back; `config: { damping: 200 }` settles in under a
second) · three or more `Easing.back` moves · a sine of the frame (an idle loop) · state or an
effect that follows the frame · a knob declared and never read. Weigh the notes against the frames.

## HTML (HyperFrames)

JSX gives knobs a person adjusts in the editor, saving and reuse, text checks and the runtime's
helpers. HTML suits a graphic already written for HyperFrames or GSAP, or one the user asks for in
that format.

`compose_motion` with `html` takes a graphic in HyperFrames' format instead of JSX, drawn and rendered
here like any other graphic, with nothing installed: HTML and CSS animated by a paused GSAP timeline registered in `window.__timelines` under the document's `data-composition-id`.
Timed elements carry `class="clip"`, `data-start`, `data-duration` and `data-track-index`; the
clip runs as long as the last of them unless `placement` sets a length. `data-width` and
`data-height` give the size it was written at; it is fitted to the frame. The background stays
transparent unless the document paints one, so footage under it shows.

It runs sealed: scripts from cdn.jsdelivr.net, unpkg.com or cdnjs.cloudflare.com (GSAP from jsdelivr),
styles and fonts from Google Fonts or jsdelivr (Fontsource). Pictures come from the library, written as `item:<ref>` in `src` or `url()`, or as data:
addresses; an outside picture is imported with import_media first, and any other address is refused.
No requests, frames, video or audio inside it; footage and sound go on the timeline. HyperFrames
shader transitions are refused (they do not render frame-exact); transitions are the timeline's or GSAP. A relative
address does not load. It has no knobs, no template or `save_as`, and its words are not read by text search, `change_language` or
the text notes `inspect_timeline` returns, so its frames are judged by eye. A revision is
`compose_motion` with `html` and `clip_id`. A HyperFrames project of several files comes in as one
document, its sub-compositions inlined and its pictures imported to the library and named
`item:<ref>`. It renders in the cloud and through the desktop app;
a render in a browser tab refuses it.

## The motion rules

- **Every move has a job**: it directs the eye, carries something across a change, shows what
  changed, or gives the brand its character. The strongest motion is the content's own verb: a
  pipeline flows, a build assembles, a countdown ticks in steps, a route draws in the direction of
  travel. Cut any move with no job.
- **Smooth, not bouncy**: elements arrive on a long decelerating curve and settle.
  `Easing.out(Easing.poly(4))` is the default entrance, `Easing.out(Easing.exp)` a fast arrival,
  `spring({ frame, fps, config: { damping: 200 } })` a settle that is itself the moment. Keep
  overshoot (`Easing.back`, a loosely damped spring) for one chosen playful beat. What enters eases
  out, what leaves eases in, what moves between two places eases in and out.
- **A title lands in place**: words sharpen from a 6px blur and full transparency in about 0.13s,
  0.07s apart, or the whole line from 8px over 0.15s, with no travel. Once landed it is dead still,
  and it leaves the same way in 0.13 to 0.25s. This replaces fading and sliding up every line.
- **Nothing arrives before its beat**: everything on screen in the first quarter and then frozen is
  a slideshow. Bring each piece in when the words reach it, spread across the back half of the scene.
- **Stillness over filler**: breathing, floating, drifting and pulsing loops look like the video is
  waiting. A slow push through the back half of a scene pulls the eye off the content. A hold
  carries only a barely visible jitter, or the subject acting: a count ticking, an icon's part
  turning, a cursor leading the eye.
- **Cut while moving, in one direction**: across a cut inside one graphic, the outgoing element is
  still accelerating when the cut lands, and the incoming one carries on along the same axis, the
  same way, at the same speed. One direction carries the piece; save the others for meaning
  (`skill-motion-planning`).
- **One primary transition**: one style carries most changes, with one or two accents for the
  opening, a change of topic and the climax. Nothing animates out before a scene change except in
  the final scene: the cut does the leaving.

Timing under all of it: a single entrance lands within about 0.8s (a longer build is a stagger,
not one slow element) · an exit takes about half its entrance · a whole stagger stays under half a
second however many items · the first move starts 0.1 to 0.3s into the clip · a secondary move
travels 50 to 70% as far as the hero move and starts 0.1 to 0.2s after it · similar elements share one easing and one duration · a pause of 0.3 to 0.75s comes
before a payoff.

## Before code

A single element (a lower third, a callout, a title card) needs only the look, taken from what it
sits on. A piece with sections settles five things first:

- **The concept**: the visual idea unique to this piece: a metaphor, a transformation, a build. A
  section plan is not a concept. Several ideas from different starting points (the subject's own
  world, the feeling, the audience, the category's cliché turned over) beat the first one; two with
  the same silhouette count as one.
- **The feeling**: relief, recognition, aspiration, curiosity. It sets pacing, motion character and
  color temperature.
- **The look, in one anchored sentence**: tie it to the brand or the subject and name a design
  language (editorial typography, keynote minimalism, condensed display, editorial serif). Without
  an anchor you get the template.
- **The arc**: what changes across the piece (energy escalating, complexity building to a reveal,
  tension to release). Every section at the same energy is flat.
- **The one spectacle**: the single exaggerated moment the piece earns, when it lands and how it
  settles. Everything around it stays restrained.

Read the answers together: a vertical ninety-second piece built on dense charts puts them on a
phone screen too small to read. A specific brief (a reference, a named style) is executed as given.

Write them down before the first compose as the spec, a comment block at the top of the source
(`get_timeline` returns a motion clip's `sourceCode` when `fields` names it). Turn every
adjective into a number: "big" is a share of the frame, "fast" is seconds.

- GOAL: the one thing the viewer knows or feels afterwards.
- OUTPUT: shape, length, where it plays.
- LOOK: the anchor and what it gives: two fonts, a ground, one accent. The user's own fonts and
  colors come first: what they name or point to, or their brand kit if they have one
  (`manage_brand_kit`). With nothing given, anchor it in the subject's own world (a bakery's kraft
  bags and early light, a studio's cables and tape), not a preset look.
- MOTION: the entrance ease and duration, the exit, the stagger step, the primary transition, the
  one spectacle and the second it lands.
- SHOTS: each with its seconds, what is on screen, its one move, every element with its verb
  (arrives, draws, counts, swaps, docks) and what carries its middle (`Scene 2 (1.0 to 3.0s): the
  three icons arrive as the line names each`).
- BANNED: the defaults below that this piece could fall into, by name.

For scenes and cuts, see `skill-motion-planning`.

**The type sets density**: kinetic typography (text is the visual) · data visualization (numbers get
their moments) · UI walkthrough (screens in a device frame with callouts) · explainer (icons and
diagrams building an argument) · social cut (bold, silent-first) · brand anthem (imagery and
statements, no product detail) · logo reveal (one moment, sound carries it).

## Pacing and copy

- **Up to 6s**: one beat, one message, one visual, at most two text elements. **15s**: hook (0 to
  3), body (3 to 12), close; three to five text elements. **30s**: opening, three or four beats at 4
  to 5s each, close; five to eight. **60s**: hook, context, value, close; eight to twelve. **Longer**:
  five or more sections on a planned energy arc.
- A held statement stays on screen about 0.5 seconds plus 0.3 seconds a word, the piece's last line
  at least 2 seconds. A longer hold is a dead stretch, not a safer read. With voiceover it stays at
  least half a second past the spoken phrase. Over twenty words, split it into several beats.
  Kinetic typography breaks this on purpose: words land and leave inside a second, and the sequence
  carries legibility. The longer the text, the simpler its motion: a sentence enters plainly; only a
  word or two can carry an elaborate move.
- Nothing flashes more than three times in a second: past that is the accessibility limit for
  seizures, and a strobe on a hit is still one flash.
- Text carries the piece with sound off. It follows an arc whose close resolves what the hook
  opened, two to five words a beat, density inverse to the visual's. Keep a real figure or a named
  feature as the source wrote it ("2.4M a second", not "fast"). On-screen text is motion copy (a
  hero word, a number, a short line), not a sentence lifted from the narration.

## What makes it read as made

- Fill the frame with content, not decoration: particles and accent lines around small centered
  text is a screensaver. A short colored rule above or below a title reads as generated.
- Under a big title, the lines that follow are about a sixth of its size, set close to it and to
  each other, the address or handle no larger than the line above it; lines that grow toward the
  title's size, spread apart, lose the design.
- Vary the structure: two sections that could swap their text and look identical are one too many.
- Hierarchy is contrast (size, weight, opacity, position): a display face and a text face that
  differ, plus a mono for figures and labels where the look calls for it.
- Color comes from the brand or the footage. Without either, type is white or near black over
  the picture; an accent appears only where it marks one thing (a price, the word that turns), taken
  from a color already in the shot. The saturated purple-blue gradient and an orange accent on
  everything read as generated.
- Motion is personality: heavy and damped reads premium, snappy reads modern. Pick one physics and
  hold it.
- Stagger in order of importance, not reading order. What existed before the shot appears together.
- Velocity contrast creates energy: slow, slow, fast is anticipation into release.
- The handoff is the motion: the next section arrives while the current one leaves. A fade to black
  is a deliberate beat. Where the piece repeats a structure, give each transition a fixed meaning:
  the primary for the next thing along, an accent for going deeper into this one.
- A persistent thread (one shape, line or accent that carries through and morphs between sections)
  keeps a multi-section piece from reading as several ads. So does a persistent frame, accumulation
  where nothing exits and all but the newest quiets, or a hero prop held while everything around it
  changes. The deliberate alternative is a hard anthology of short sections.
- Connected parts share one coordinate space: a label and its arrow, a callout and its target, in
  one SVG or with shared constants.
- Deliberate imperfection (grain, a light leak on a transition, a cursor that hesitates): one is a
  signal, two is texture, three is noise. Use none where precision is the point.

## Code patterns

- **The easing table**: `Easing.out(Easing.poly(4))` the default entrance · `Easing.out(Easing.exp)`
  a fast, confident arrival · `Easing.in(Easing.poly(5))` and `Easing.out(Easing.poly(5))` the two
  halves of a cut made while moving · `Easing.inOut(Easing.cubic)` a move between two positions ·
  `Easing.in(Easing.exp)` an exit that accelerates away · `Easing.inOut(Easing.sin)` calm and slow
  · `Easing.out(Easing.circle)` a camera or an orbit · linear only for a constant scroll or the
  value behind a counter. A piece speaks in about three easing characters: a calm, a standard, a
  punch. A bounce, when chosen, is a real spring, `spring({ frame, fps, config: { damping: 12,
  stiffness: 200 } })`, held as the piece's one physics; a spring times itself, so
  `durationInFrames` stays off it and stiffness sets its speed.
- **Enter, hold and exit in one interpolate** on one value: keyframes `[0, 0.5 * fps, hold, hold +
  0.4 * fps]` to `[0, 1, 1, 0]`, one easing per segment, clamped both ends.
- A scale animation takes `output: 'perceptual-scale'`: a linear scale reads as slowing the larger
  it gets.
- **Anticipation**: a few frames of the opposite move before the real one (a card dips before it
  rises, a counter settles back before it climbs). **Follow-through**: what a move drags (a shadow, a
  label) resolves one or two frames after it, and a surface landed on gives a few pixels and returns.
- **Stagger by frame offset**: `frame - i * 3` (letters 1 frame apart, words 2 to 4, list items 1
  to 3, at 30fps and scaled with the rate), the cascade inside half a second, the step tightening
  with many items. Opacity leads position by 2 to 3 frames, position leads scale.
- **Size off the canvas, and use it**: `fontSize: width * 0.08` display, supporting text from
  `height * 0.03`, nothing below `height * 0.026` (28px at 1080). Words are cut before type is
  shrunk. A graphic that owns the frame composes the whole frame: a small card centered in an empty
  frame is not a shot. The full scale is `skill-graphic-design`. Key
  content inside about 7% of the width from each side and 9% of the height from top and bottom; on
  9:16 nothing in the top 12% or bottom 24%, text
  30 to 50% larger; 1:1 centers. Recompose, never crop.
- **Animate with `transform`, lay out with flex**: animating top, left, width or height reflows text
  every frame.
- **Elements time themselves**: `AbsoluteFill`, `Img` and `Video` take `from`,
  `durationInFrames` and `trimBefore` in frames, and inside them `useCurrentFrame()` starts at 0.
  `Sequence` does the same for anything else; a Sequence holding an Img or a Video takes
  `premountFor` (a second's worth of frames). `Video` takes `trimBefore`, `trimAfter`,
  `volume`, `playbackRate`, `loop`, `muted`; a backdrop video is `muted`.
- **A video is mounted from the start, never behind a time condition**: `t < 3 ? <Video/> : <Video/>`
  or `if (up <= 0) return null` above it opens the file on the frame the branch flips, and the
  player stalls for the open and the seek. Each video gets its own `Sequence from
  durationInFrames premountFor`, all declared at once; the Sequence shows and hides it, and a
  change of shot is two Sequences meeting, not one element swapping its source. Past a few cuts
  into footage, the shots go on the timeline as video clips under the graphic, or are rendered to
  one file with the cuts in it that the graphic plays straight through.
- **Numbers**: `toLocaleString('en-US')`, or the separator depends on the rendering machine; figures
  that count or stack take `fontVariantNumeric: 'tabular-nums'`, and so does the
  `Layout.measureText` call sizing them.
- **A background is a decision**: atmosphere is a base color plus two or three blurred color blobs
  (`filter: 'blur(100px)'`, 15 to 30% opacity) and optional grain; editorial flatness is one solid
  chosen color. A single default gradient reads as a slide.
- **Text over imagery** gets a gradient shelf, a panel or a shadow.

## Audio

If the piece has music, the music comes before the design: the user's own track, then
`generate_audio` music made to fit the piece's length and hits exactly (a job, in credits) where
the user has credits, a stock track (`search_stock` kind music) only where the user picked it. Measure beats with `detect_beats` (cloud processing). A hit lands
about 0.1s (three frames at 30) ahead of the measured beat, where it reads as on it. On calm music
the grid is the tracker's guess, so changes land on phrases instead. The rest of cutting to music is
`skill-sound-design`. Sound effects make transitions physical; use them selectively. The piece
still works silent, because the feed plays it silent.

## The order across tools

1. **The shelf** (`get_templates`): place a saved or ready graphic that fits with `compose_motion`
   `template` and `values`. For a change the controls do not cover, start from its source
   (`code: true`).
2. **The settled frame first**: compose every ramp at its end value, draw it, and check overflow and
   hierarchy. Then write the ramps as the journey into it.
3. **`compose_motion`** compiles the source and inserts the clip at `placement` (`from` and
   `duration_frames`, or `from_seconds` and `duration_seconds`), on the top graphics track when
   that track is free at that time. Without placement, the clip is three seconds. A graphic timed to a
   spoken word takes its place from `get_transcript`: the word's from_frame over the timeline's fps
   is from_seconds. The response carries the compile notes and draws no frame.
4. **`inspect_timeline`** draws real frames of the composite with notes: words cut off by their
   box, off the frame, overlapping, too small, too faint for what is behind them, under a vertical
   platform's buttons, read from the graphic's own words once settled. Draw one frame past the first
   sixth of the clip and before the last, where it has settled; the signature move at its peak; and
   either side of any cut inside the graphic. Cloud processing. A drawn frame inside the clip's range
   with no graphic on it means the code threw at that frame (a runtime failure reports success and
   renders nothing): check the placement once, then rewrite the section.
5. **Revise** with `compose_motion` and `clip_id`: the code is rewritten in place and the clip
   keeps its place, length, lane, fades and whatever a user set its knobs to. For a small change use
   `source_edits`, find and replace on the clip's code; for a rewrite send the whole file. With the
   user watching, a small change goes straight back to them with no frame drawn. Turn a relative note
   ("finer", "bigger") into an absolute value; "longer" means a longer hold unless they say slower.
   If a scene keeps missing, strip it to its subject and its one move, confirm it on a frame, then
   build it back up one layer per compose. Move or resize the clip with `edit_timeline` moveClip and
   trimClip; change a knob with updateClip and `values`; `undo` walks back a compose that made
   things worse.
6. **Keep it** with `save_as`: code, settings and length under a name, overwriting an existing
   name; `manage_templates` renames or deletes. A graphic meant for another editor sits alone on its
   own timeline and renders with `render_timeline` format webm: transparency kept, nothing drawn
   where no clip is.

**Sections are separate composes placed by frame**, unless an element carries across the cut between
them (a headline word, a card that docks, a cursor). Then they are one graphic, since nothing crosses
between two clips. The ground under a whole piece is its own graphic on the lowest track, with its
`<Grain />` and any vignette; sections sit transparent above it. Stills and b-roll from the library
sit under the overlays the same way. Put an asset inside the clip when the graphic has to move with
it (a callout on a screenshot, a logo being revealed): an `image` or `video` control, set in
`values` to the library item's ref. Code draws interfaces, type, shapes and charts. A
character, an illustration or a painted scene in SVG tops out at clip art, so take it from the user's
own art, a stock illustration or icon (`search_stock` kind illustration or vector), or generate one
(`generate_image`, credits; one made on a flat contrasting ground cuts out cleanly with
`remove_background`). Place it as an `Img` with the moves built in code above it, and check its
edge over a light and a dark ground.

Build a multi-section piece audio first, then the first section alone, composed and inspected, since
it sets the direction; then the remaining sections one compose at a time, each inspected; then read
the whole piece: does the arc build, does one primary transition carry the changes, does text hold
long enough, does anything sag?

**A finished picture animated in place** (a thumbnail, a slide, a screenshot, a diagram, where every
pixel stays as drawn) needs no code and no generation. The picture is the lowest clip. Each part that
moves is the same item again on its own track above, with `transform.crop` cut to that part's box,
so it sits exactly where it is in the picture. `inspect_color` points and lines give the boxes and
the ground to the pixel, which the eye cannot. Opacity, scale and position keyframes move a part. A
shape in the picture's ground color hides what has not arrived yet, and a dark shape at partial
opacity dims the rest. Scale grows a cropped part from the center of the frame, not its own: a part
centered at (cx, cy) in frame fractions stays put at scale s with position `{x: (cx - 0.5)(1 - s), y: (cy -
0.5)(1 - s)}` keyed beside it, and travels from elsewhere by adding the offset. A `frame` slot
resizes its media, so it does not work here. Every part back at scale 1, position 0 and full opacity
is the picture again, which is where a loop ends.

## The check

Check real frames at the entrance, mid-hold and exit of every section, about five a call at 1080p (more exceeds the
request size limit):
legible at delivery size over the actual background; nothing mid-animation on a clip's last frame;
nothing drifted that should connect; one primary transition held, with accents only where they mean
something; the arc felt; every compile note weighed; the look chosen rather than generated
(`skill-graphic-design`'s check). Rank each problem P0 (unreadable, broken, an empty frame), P1 (a default
from the list below, a number off the spec) or P2 (polish). Fix P0 and P1 before handover.

When a piece is close: off pacing is the energy arc (no escalation, no holds), not individual
timings · "more energetic" means more contrast between slow and fast, not everything faster ·
"looks generic" is, in order, brand colors or the purple default, structural variety, the persistent
thread, full-frame content or centered text on a gradient · "shorter" cuts the weakest section whole
rather than compressing everything · "a slideshow" is content dumped early and held: move each piece
onto its beat and hand scenes over while moving · "cheap" or "AI" is bounce, idle wobble, web-sized
type and the default fonts, in that order.

`skill-finish` has a last pass over the whole piece.

## Handover

- The piece at its destination's shape and length; a section cut to music lands on beats from `detect_beats`.
- Over the user's footage: no graphic over a face or in the caption band.
- In the app from its first media, where it plays; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere, a transparent WebM when the graphic goes into another editor.
- Also possible: the graphic saved as a template for reuse (`save_as`), versions at other shapes, sound effects on its transitions, a music bed cut to it.

## Defaults that read as generated

What a graphic falls into when nothing is decided. Each needs a reason from the brief, the brand or
the subject; a reference or a brand from the user outranks this list.

- Small things floating in the middle of a plain ground, dark or white, the frame mostly empty.
- Everything fading and sliding up; the same move beat after beat. The primary transition carries
  the changes between scenes; the opening and the close each arrive their own way.
- Glow, particles, purple gradients, glass cards.
- Corner brackets, HUD readouts, status pills and small labels that report nothing real.
- Cream or paper tones with a serif display, an italic accent word and small numbered labels
  (01 / 03, NEW FOR FALL in spaced capitals).
- Three cards in a row, or side-by-side panels standing in for a scene.
- Everything centered; a logo docked in a corner.
- A title that keeps drifting or breathing after it lands.
- A ring on every click; bounce on interface elements; things popping in from scale zero.
- A prompt box being typed into as the opening shot.

## The stranger's read

Once, on a finished piece, using frames already drawn: describe each beat as someone who has not
seen the brief would, naming every object on screen. Fix any object that needs explaining first:
remove it or make it read. The words alone and the pictures alone should tell the same story. Skip
this for a single card, or a small note while the user is watching.
