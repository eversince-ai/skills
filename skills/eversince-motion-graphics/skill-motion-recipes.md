# Motion graphics: recipes

Atomic moves under the names a plan cites ("the count lands by count-and-grow, the bars by
value-bars"). Two to four make a scene; six is noise. Shelf: compiled code that does it
(`get_templates` with the id and `code: true`). Helper: in scope, one line. By hand: the mechanic
given.

Every timed value goes through one clamped helper:

```jsx
const ramp = (t0, t1, from, to, easing = Easing.out(Easing.poly(4))) =>
  interpolate(frame, [t0 * fps, t1 * fps], [from, to], { extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing });
```

Eases by feel: **settle** `Easing.out(Easing.poly(4))`, the default · **slam**
`Easing.out(Easing.poly(5))` · **snap** `Easing.out(Easing.exp)` · **gentle**
`Easing.out(Easing.cubic)` · **orbit** `Easing.inOut(Easing.circle)`, camera legs and moves round a
center · **overshoot** `Easing.out(Easing.back(s))`, only where a recipe says so. A piece uses
about three. A stepped ease is `Math.floor(p * n) / n`. Pixel figures are at 1920×1080 and scale by
`width / 1920`. Every position is one the piece placed, or `Layout.measureText` for text.

## Text and typography

- **typed-states**: typing built from whole states at time thresholds (keystrokes, a typo,
  backspaces, a paste, a thinking pause), an array of `{ t, text }` built from a script, each frame
  showing the last state whose `t` has passed, nothing animating between them. Plain typing with no
  edits is the smooth slice, `text.slice(0, Math.floor(ramp(t0, t1, 0, text.length,
  Easing.linear)))`. Keystrokes 0.06 to 0.20s apart, word-break pauses 0.3 to 0.6s, a thinking hold
  0.8 to 2.0s; 3 to 5 characters a second is dramatic, 8 to 12 natural, 15 to 20 a tech demo;
  backspace 20 a second; the last state lands 1s or more before the clip ends. Type 48 to 96px
  monospace, the box fixed at the widest state (`Layout.measureText`), `whiteSpace: 'pre'`. The
  caret: a block about 0.3× the font size wide, solid while keys land, then a slow blink (about one per 1.1s, eased) once typing stops
  (a 0.15 to 0.3s grace), one visible at a time. Shelf: title-typed-hook, screen-typed-search,
  terminal-command-run, code-one-line, cta-sign-up, name-terminal-credit.
- **colored-caret**: the caret wears the color of the segment being typed (accent on the brand, dim
  on a placeholder, success on the completion), three or four colors at most, the brand segment up
  to 1.1 to 1.25× taller and typed a little slower; on a light segment the caret turns dark with the
  text. By hand on typed-states.
- **Word rotation**: type the word, hold 1.5s, backspace at 20 a second, wait 0.3s, type the next;
  the caret solid while keys land and blinking in every hold. By hand on typed-states.
- **length-timed-cards**: a run of cards or phrases where each lasts `BASE + chars × CHAR_SEC +
  hold`, summed once into `{ start, end }` windows and found each frame by the last-passed search,
  hard-cut. Base 0.6 to 1.5s · 0.03 to 0.06s a character, the same for every item · mid hold 0.5 to
  1.0s · final hold 1.0 to 2.0s · 3 to 6 items · the body's height reserved at the tallest item.
  **Accelerating cadence** (a roll call): hold i is `max(FLOOR, FIRST × DECAY^i)`, first hold 0.8
  to 1.2s, decay 0.75 to 0.88, floor 0.15 to 0.3s; under 0.5s the per-character term is dropped;
  the last brakes at 4× the floor or more; 12 to 18 items. By hand.
- **word-drop**: words arriving from below, each starting before the last settles; visibility binary
  (`frame >= start * fps ? 1 : 0`), `translateY` to 0 on slam (snap for more), one direction. An
  anchor word rises 60 to 80px over 0.16 to 0.20s with 0 to 2 frames of gap after it lands; a normal
  word 40 to 50px over 0.13 to 0.16s overlapping by 1 frame; punctuation 30 to 48px over 0.10 to
  0.13s overlapping 1 to 2. The final word split into fragments that travel further. Shelf:
  text-letter-settle, text-rush-in (with Streak), title-stacked-caps.
- **slot-roll**: a slot-machine step: a window one item tall (`overflow: 'hidden'`, about
  `fontSize × 1.25`, every item the same height) over a column moved by the sum of per-step
  overshoots, `y = -[0, 1, 2].reduce((s, i) => s + ramp(0.8 + i * 0.4, 1.3 + i * 0.4, 0, 1,
  Easing.out(Easing.back(1.8))), 0) * ITEM_H`. 1 to 4 steps · 0.3 to 0.7s each · spaced 0.3 to 0.5s
  so they overlap · overshoot 1.4 gentle, 2.0 firm, 2.5+ a casino landing · tabular figures · hold
  1s after the last step. Shelf: title-word-swap, text-split-flap.
- **Decode scramble** (letter-flap, flat): a word resolving out of flickering glyphs; each character
  has its own progress on a stagger, below the reveal threshold showing a glyph hashed from its
  index and `Math.floor(frame / swap)`, past it the real letter. Size 6 to 10% of the shorter side
  (15% alone on screen) · 0.4 to 1.0s a character · stagger 0.03 to 0.08s · reveal at 0.5 to 0.7 of
  its progress · a new glyph every 3 to 6 frames · monospace in fixed `1ch` cells. By hand; the
  hash is `const hash = (n) => { const x = Math.sin(n * 127.1 + 311.7) * 43758.5453; return x -
  Math.floor(x); }`.
- **Echo layers** (stacked-type, flat): the same heavy word stacked 4 to 6 times at growing offsets
  behind a solid front copy, each layer `position: 'absolute', inset: 0` with its color as rgba and
  `translate(i * 2 * grow, i * 2 * grow)`, the front copy `position: 'relative'`. Offsets 1 to 3px
  a layer · nearest alpha 0.6 to 0.85 stepping down 0.08 to 0.15, floored at 0.1 to 0.2 · 60px
  minimum, 200 to 340px for a hero, black weight · layers fade back to front 0.04 to 0.10s apart
  while the offsets grow from 0 over 0.4 to 0.8s · no text-shadow · under per-letter motion, 2 to 3
  layers. A color shift instead of fading: each back layer's hue steps 4 to 12° and its lightness
  down. Shelf: depth-deep-type (in depth).
- **beat-lines**: short phrases slamming in one per beat, each with a different entrance, locking
  into a finale. One beat array every element reads its time from; beats 1.2 to 1.8s apart and even
  · each hit 0.35 to 0.6s · exits 0.25s or less · 150px+ in a heavy display face · one accent hue on
  the verbs · at least three different eases across the piece (slam the default, snap for side
  snaps, `Easing.out(Easing.circle)` a heavy rise) · the finale dressed (an underline sweep, a
  Notation mark), then still. Shelf: title-bold-statement, title-setup-payoff, title-window-word.
- **color-through-type**: color moving through letterforms that never move: a gradient wider than
  the text (`backgroundSize` 200 to 400%), clipped into the glyphs (`WebkitBackgroundClip` and
  `backgroundClip` both `'text'`, `color: 'transparent'`), its `backgroundPosition` sliding
  linearly 100% to 0% over 1.2 to 3s. Settle by crossfading to a solid twin in the same grid cell
  over 0.1 to 0.35s. Word by word: a gradient copy per word rising 0.15 to 0.3s and falling 0.25 to
  0.45s, the next rising 0.25 to 0.5s later so the envelopes cross. Hue sweep: `filter:
  hue-rotate()` through 40 to 180° over 0.8 to 1.6s, then the twin. Sheen: one narrow band (25% of
  the span at most), one pass. Working status: whole passes of 1.2 to 1.8s repeating while live, a
  whole number of passes ending on the frame the status resolves. Shelf: text-fill-up,
  title-spoken-hook.
- **split-shudder**: interference as punctuation: tinted copies shudder behind the element, then it
  snaps sharp and holds. Offsets hash `Math.floor(frame / stepFrames)` with the copy's index (a
  step 1/30 to 1/12s), held a step then jumping · an envelope 1 → 0 on an in-ease, exactly 0 outside
  its span · copies 4 to 14px apart on a headline, the vertical offset 35% of the horizontal ·
  entrance 0.25 to 0.6s, burst 0.12 to 0.2s · copy opacity 0.5 to 0.9 on dark, 0.35 to 0.6 on
  light, or blended (screen on dark, multiply on light) · the base painted last with `position:
  'relative'`. Stretch entrance: `scaleX` 1.3 to 1.8 → 1 on slam while the envelope runs. Slices,
  the restrained form: 4 to 10 copies clipped to bands (`clipPath: inset(top% 0 bottom% 0)`) offset
  12 to 60px by each band's hash, converging over 0.2 to 0.4s in 3 to 6 steps. By hand.

## Data and stats

- **count-and-grow**: a number counting up while its scale grows to full size; count and scale share
  one start, duration and ease. From 40 to 60% of the landed size · 1.2 to 2.5s on gentle (settle
  and snap land the figure early and freeze its last digits) · suffix 0.3 to 0.6s at the count's
  end, overshoot 1.4 to 2.0 allowed on it (punctuation, not data) · label in before half the count
  over 0.4 to 0.7s · tabular figures in a box measured from the landed value · `Math.round` and
  `toLocaleString('en-US')` · a figure with a unit and a decimal counts from a floor that already
  has the landed shape ($1.0M → $4.2M), never from $0 · no overshoot on the number · stats counting
  together share ease, duration and start. Shelf: stat-big-count, stat-then-now, social-milestone,
  review-score, chart-race.
- **value-bars**: the graphic that gives a number weight: growth bars, a fill bar or ring, a row of
  stars filling partway. 4 to 6 bars, the last the accent · fills 0.8 to 1.2s sharing start and ease
  with the count beside them · stagger 0.06 to 0.1s · one accent hue, the rest muted, flat fills ·
  grown by scale from the baseline (`scaleY` from `'bottom'`, `scaleX` from `'left'`) · a ring's
  dash is its real circumference (`2 * Math.PI * r`), rotated −90° to fill from twelve, hidden at
  zero · stars are a gold row over a gray row, the gold clipped by `clipPath: inset(0 (1 − score /
  outOf) × 100% 0 0)` over about 1s. Shelf: stat-ring-percent, chart-bars-across, chart-standout-bar,
  chart-share-bar, progress-goal, review-card.
- **chart-read-head**: a tracking line and marker riding a drawn series while a readout steps
  through the data. One progress value, `at = ramp(1.2, 3.4, 0, SERIES.length - 1,
  Easing.inOut(Easing.quad))`, drives line, marker, readout and any cursor, all mapped through the
  same `X` and `Y` over one literal `SERIES` that drew the polyline. 10 to 40 points · 1.5 to 3s
  on the gentle in-out, linear for a playhead · readout 16 to 48px up-right of the marker, flipped
  near the right edge · marker radius 6 to 12 on a 4 to 8px line · hold 0.8s or more on the landing ·
  a peak stop: the marker pops a size larger and a pill lands with its label. Shelf: chart-build,
  chart-marked-growth.

## Camera and viewport

- **aim-zoom**: zooming into an element off center is one `<Camera>` leg with `focus` at the
  element's center as laid out. 1.5× gains a little, 3× takes over, 5× is extreme; capped so the
  target fills 88% of the frame at most, `Math.min(want, 0.88 * width / w, 0.88 * height / h)` · 1.0
  to 2.0s on `Easing.inOut(Easing.poly(4))`, starting 0.5 to 1.5s after the layout lands, holding
  1s or more after · pictures need source resolution at least rendered size × zoom · `overflow:
  'hidden'` and the background on the root. Helper: Camera. Shelf: screen-zoom-detail.
- **rig-camera**: the camera as one wrapper around everything, which `<Camera>` is. Scale 1.02 to
  1.05 barely there · 1.05 to 1.15 emphasis, the product default over 2 to 3s · 1.15 to 1.30 a region
  · 1.5 to 2.5 the element fills the screen · 3+ covers it. For a 4 to 12× pull-out the workspace is
  built at 1× and opened scaled in on one detail. Depth planes: the far plane at about a fifth of
  the camera's rate, content at full, a foreground element at about six times; depth sold by
  overlap, one big soft foreground shape at most; parallax rides a move with a destination. Helper:
  Camera.
- **camera-phases**: two or three zoom phases each a leg: phase 1 at 0.88 to 0.96 · phase 2 at 0.98
  to 1.02, at 0.3 to 1.0s over 1.0 to 1.8s · phase 3 at 1.04 to 1.15, at 2.0 to 4.0s over 1.0 to
  2.0s. Focus-in (back, neutral, slight push) is the product reveal; dramatic reveal is push,
  neutral, pull; bookend (neutral, strong push, neutral) lands a CTA. Gentle, settle or
  `Easing.inOut(Easing.cubic)`; the hero revealed after the pull-back lands; between phases the
  camera holds. Helper: Camera.
- **Camera legs** (depth-flight, flat): a journey across one oversized world leg by leg, each leg a
  verb, a `blur` on a whip or a dive. Dive or landing 0.6 to 1.0s on slam · repositioning 1.2 to
  2.0s on `Easing.inOut(Easing.cubic)` · pan 0.8 to 1.5s · whip 0.5 to 0.8s · holds 0.8s or more
  between legs, a final dwell 1s or more · anything read is read at a landing · a cruise past
  standing cards is one long leg, 2 to 4s near-constant · a piece ending mid-dive gives the last leg
  a window past the clip's last frame, and the compile note about a ramp past the clip is then the
  point. Helper: Camera.
- **follow-the-caret**: a line growing past the frame kept in view: the world holds until the typing
  edge reaches 0.5 to 0.75 of the width, then pans to keep it there, `worldX = Math.min(0, TARGET_X
  − (PAD + typedWidth))`, continuous at the handoff. The caret an inline sibling of the text. By hand
  on typed-states.
- **rack-blur**: blurring and dimming all but the focal element: 3 to 6px a depth step (a defocused
  plane 8 soft, 16 default, 24 heavy), dimmed to 0.55 (0.4 strong, 0.7 subtle), over 0.5 to 1.2s on
  `Easing.inOut(Easing.cubic)`; the focal layer sharp and painted after the rest; small or grouped
  layers blurred, never a full-frame background; refocused to zero before a handoff. By hand. Shelf:
  lens-into-focus, lens-focus-band.

## Layout and networks

- **spoke-spread**: elements starting at the center and radiating to their places, each centered with
  `translate(-50%, -50%)` and moved by its target offset × progress. 3 to 8 items · 1.0 to 1.8s on
  settle · stagger 0.04 to 0.08s · with 6+ items starting 0.3 of the way out. Driven by a count: no
  stagger, the count's own duration and ease. Its reverse is the orbit collapse. By hand. Shelf:
  diagram-hub, diagram-rings.
- **Orbit collapse** (ring-flip-in, flat): a ring of items condensing into its center on the frame
  of its cause, one 1 → 0 value multiplying both radii and each item's scale, 0.5 to 0.9s on
  `Easing.in(Easing.poly(4))`; the ring an ellipse (y at 0.4 to 0.7 of x, 4 to 12 items) that never
  touches the center label: `RADIUS_X × min|cos θ| ≥ label half-width + item half-width + air`. By
  hand.
- **Scatter and assemble** (tumble-assemble, flat): fragments flying in from a scatter and locking
  into a layout, each starting at its index on the golden angle, `a = i × Math.PI × (3 −
  Math.sqrt(5))`, at `(cos a, sin a) × RADIUS`. 4 to 14 fragments · radius 250 to 700px · 0.7 to
  1.4s on settle · stagger 0.03 to 0.09s with `n × stagger < duration` · a swap runs both sets off
  one shared progress · a scatter-out only as the piece's last beat. By hand. Shelf:
  gallery-photo-pile, gallery-tile-build.
- **face-ring**: faces on an elliptical ring around a hub with lines drawing out to each. Ring 20 to
  30% of the width by 18 to 25% of the height · 8 to 12 avatars at 80 to 120px · hub in over 0.4 to
  0.6s, avatars from 0.6 of the way through it, 0.4 to 0.7s each 0.06 to 0.10s apart · lines from 0.1
  to 0.2s before the last avatar settles, 0.4 to 0.7s each · one center constant feeds hub, ring and
  line ends; the hub paints after the lines · still for 1s or more. By hand. Shelf:
  gallery-picture-ring, diagram-hub.
- **detect-box**: a detection box following a moving subject: four L corners (48px arms, 6px thick,
  8px out), a mono confidence label, detection yellow #f9cb1c. Each frame computes the subject first
  and derives the box · drift 40 to 200px over 0.5 to 3 cycles · box 200 to 500px jittering 5 to 10%
  at a non-whole 1.5 to 3× the drift rate · confidence 95 to 99 flickering at 3 to 6× · lock-on from
  0.5 to 0.9 scale over 0.3 to 0.8s on overshoot 1.2 to 2.5 · several boxes offset by π / N. By hand.
- **edge-grow**: a container pinned at one edge growing along one axis with what follows riding its
  edge (a dropdown, a panel growing a stack). Built expanded, the collapsed state faked: a
  fixed-height mask (`overflow: 'hidden'`) holds a sheet sliding from `−BODY_H` to 0 and everything
  below rides the same value. Grow 0.35 to 0.6s on settle, rows fading in from a quarter of the way
  (0.2 to 0.3s each, 0.04 to 0.08s apart), collapse 0.2 to 0.35s on `Easing.in(Easing.poly(4))`. A
  side pane grows along x; its neighbor is overlapped or pushed, never both. **Typed-wrap growth**:
  a composer growing from its top edge as its text wraps, one short step per authored `\n` (0.12 to
  0.2s on settle, one line height), the card drawn at full height and clipped down with `clipPath:
  inset(top 0 0 0 round r)`, the lines riding up on the same value. By hand. Shelf: ui-context-menu,
  chat-assistant-reply.
- **Split cards** (facing-pair, flat): two cards entering from their own sides, 200 to 500px (0.3 to
  0.6 of a width) over 0.6 to 1.2s on settle, the right 0 to 0.3s behind; cards 480 to 820px at 1920
  with 40 to 120px between, the pair inside 95% of the stage; two lines of body copy at most;
  symmetric weight; copy arriving in the entry's tail; shadows falling outward. Shelf:
  compare-split, compare-versus, compare-plans, compare-do-dont.
- **Page scroll** (tilted-page, flat): a page taller than its card scrolling to one section, the
  distance measured from real section heights, 0.8 to 1.8s on one ease for every scroll in the
  scene, steps never overlapping, a spotlight dim fading in over 0.4 to 0.8s once the scroll lands.

## Morphs and handoffs

- **pop-arrival**: the canonical arrival: `scale` 0 → 1 with a 0 to 32px rise, settle or snap, 0.4
  to 0.7s, the hero visible by 0.5s. A group of 3 to 9 staggers `Math.min(0.06, 0.5 / count)`;
  over 9, a wipe. Order by index, from the center out by `|i − (n − 1) / 2| × step`, or from the
  edges in. For exact physics a spring with mass 1, `stiffness: (2π / response)²`, `damping: 2ζ ×
  2π / response`: response 0.25 to 0.35s a tight snap, 0.35 to 0.5 a standard entrance, 0.5 to 0.7 a
  weighted hero; ζ 1 is the house settle, 0.8 to 0.85 overshoots 0.6 to 1.5%, 0.6 to 0.7 overshoots 5
  to 10% (playful only). Opacity never rides an overshooting curve. By hand; nearly every shelf card
  arrives this way.
- **same-spot-swap**: one state swapping for another in the same place: the outgoing shrinks and
  fades fast on an in-ease (0.3 to 0.5s), the incoming pops from 0.6 to 0.8 scale just before the
  exit ends (0.45 to 0.7s, overlap 0.1 to 0.2s), painted on top; overshoot on the incoming only (1.4
  light, 1.8 firm, 2.2 cartoon; a serious piece uses settle); inner detail 0.2 to 0.4s after the
  settle; both states mounted at opacity 0 in one fixed wrapper; hold 1s or more. By hand. Shelf:
  ui-checkout, ui-upload.
- **shape-morph**: a container changing shape between two shots and carrying the eye across.
  Apparent size is uniform scale, never width or height; radius, surface and shadow ride the same
  value; old content fades over the first 30 to 50%, new over the last 30 to 50%. Hold shot one 0.6
  to 1.5s · morph 0.6 to 1.2s on `Easing.inOut(Easing.cubic)` · scale is width two ÷ width one, the
  shots aspect-matched · the radius lands at `apparent / END_SCALE` · a fill changing color is
  `interpolateColors` on the same value. By hand.
- **skin-swap**: everything re-skins around one thing that never moves: each skin one complete
  pre-styled layer, a simultaneous opacity crossfade between neighbors, the anchor drawn once on
  top. 3 to 5 skins · held 0.8 to 1.5s · morph 0.25 to 0.4s · skin k at `START + k × (HOLD +
  MORPH)` · the last held 2 to 3× · shared geometry aligned to the pixel. By hand.
- **knock-aside**: an entrance that knocks the previous element out: one value on overshoot 1.2 to
  2.0 over 0.6 to 1.4s drives both; the intruder travels its full path tilting 5 to 15° back to flat,
  opaque by a third to an eighth of the way; the victim finishes by 0.4 to 0.5 of it on the same
  axis the other way; the intruder paints above; hold 1 to 1.5s. By hand: `p = ramp(1.0, 1.9, 0, 1,
  Easing.out(Easing.back(1.5)))`, `victim = Math.min(1, p / 0.45)`.
- **three-speed-slide**: sliding a composed group aside slow, fast, slow, in one interpolate with
  three segments, `[0, 0.12, 0.22, 0.57]s` to `[0, −30, −210, −270]` with easings
  `[Easing.in(Easing.poly(4)), Easing.linear, Easing.out(Easing.poly(5))]`; the tail 3× the ramp's
  time or more; new content revealed during the burst. A whip on the camera is this curve with blur
  riding it: climbing as the move speeds up, held through the linear stretch, gone as it slows. By
  hand. Shelf: transition-speed-smear.
- **ghost-trail**: a fast entrance with velocity blur peaking at peak speed and gone at the settle,
  the blur sharing the position's window and ease exactly. Snap or slam · 0.25 to 0.6s · runway 40
  to 120% of the element's size · entrances and mid-shot moves only. A lateral streak is an echo
  trail, which `<Streak progress from copies spacing>` draws; a push-through from 1.3 to 2.5 scale
  takes `filter: blur()` 8 to 30px (default 18; on a camera wrapper about 18 to 20, a whip 24).
  Helper: Streak. Shelf: text-rush-in.
- **directional-blur**: a smear along the travel, sharp across it, where `filter: blur()` softens
  every direction. A zero-size `<svg>` holds `<filter id x="-50%" y="-20%" width="200%"
  height="140%"><feGaussianBlur stdDeviation={`${smear} 0`} /></filter>`, its id unique to the
  graphic, and the moving wrapper takes `filter: 'url(#id)'`; `0 ${smear}` for a vertical move.
  `smear` is the frame-to-frame travel of the same interpolate times 0.5 to 0.8, capped at 12 on
  type and 24 on a surface filling the frame, with `filter: 'none'` below 0.2 so the settled frame
  is sharp. By hand.

## Cursors and interaction

- **pointer-click**: a cursor enters, lands on its target and clicks, and the click causes the next
  thing on the same frame. `<Cursor moves clicks exit />` does all of it: size about 7% of the
  frame's width (5% inside a mock) · in from off frame on one vector, from below by default,
  decelerating over 0.75s · moves between targets 0.6s on a calm in-out · the tip is the click point
  · the tap is 1:2, down to 0.84 in 0.1s, back in 0.22s, the target compressing in parallel with
  `Cursor.press(t, clicks)` · a ring 0.02s after the click, 0.7s · exit off the nearest edge on an
  in-ease over 0.6s, never a fade in place, or carrying the cut (`skill-motion-planning`). Between
  clicks the cursor drifts aside over 0.5 to 0.9s on gentle. **heavy-press**: cursor and button
  compress by one amount (0.05 subtle, 0.10 standard, 0.15 heavy) over 0.1 to 0.25s, releasing
  together over 0.4 to 0.7s on overshoot 1.6, 2.0 or 2.4. Helper: Cursor. Shelf: ui-agent-steps,
  callout-click-marker.
- **press-back**: a tactile press on something already on screen: a near-linear dip then a spring
  back, one interpolate with two segments. Press to 0.92 (0.88 dramatic, 0.96 subtle) over 0.10 to
  0.30s on `Easing.in(Easing.quad)`; release 0.40 to 0.90s on overshoot 1.4 soft, 2.0 firm, 2.8
  cartoon; the shadow sinks with it. A burst behind, painted first, from the release: a blurred
  radial of the button's hue to 3× subtle, 6 default, 8 max, peak 0.4 to 1.0, 0.4 to 0.7s each way. A
  glow on the room peaks 0.1 subtle, 0.25 default, 0.45 at most. The button 3 to 5% of the frame or
  more; on a confirm it turns to the success color at the release and a check pops on overshoot 1.4
  to 2.0 over 0.3 to 0.6s. By hand. Shelf: ui-checkout, social-subscribe-press.
- **pointer-drag**: grab, carry, drop. Tip and payload read the same eased value (`Cursor.at(t,
  props, width, height)` is the tip). On the press a ghost (0.5 to 0.75 opacity) lifts to 1.03 to
  1.08 over 0.12 to 0.2s; travel starts after the grab plus both press halves plus the lift, 0.6 to
  1.2s on `Easing.inOut(Easing.cubic)`; at the end of travel the ghost goes and the placed item snaps
  in over 0.2 to 0.3s with its chrome. Resize is uniform scale about the anchor corner, the cursor
  sent to `anchor + scale × (corner₀ − anchor)` on the same window. Helper: Cursor; the payload by
  hand.
- **team-pointers**: two to four labeled cursors (`<Cursor label color>`) working one canvas, a
  literal table of legs per actor (0.6 to 1.2s, `Easing.inOut(Easing.cubic)`) with 0.5 to 1.5s rests
  of total stillness; actions on alternating beats 1s or more apart; each actor in its zone (two
  within 80px reads as a glitch); actors may be mid-canvas on the first frame; the camera never
  follows. Helper: Cursor.
- **live-control**: an edited control and the thing it controls answering in the same frame: one
  eased value drives readout and target, same start, duration and ease. Scrub 0.8 to 1.6s on a gentle
  in-out · 0.3 to 0.8s between edits · 2 to 4 edits · endpoints big enough to show · tabular figures
  and a fixed width on the readout · a camera punches in and returns around the beats, never
  through them. By hand.

## SVG and icons

- **line-draw-on**: a stroke tracing itself: `Paths.evolvePath(progress, d)` returns the dash
  values for a `<path>`. A segment 0.3 to 0.8s · a short one 60 to 80% of that, or scaled by
  `Paths.getLength(d)` · each stroke starts 70 to 80% into the last · the wordmark after the last
  stroke + 0.2s, over 0.3 to 0.8s · gentle or linear, never overshoot · `fill: 'none'` until a
  draw-then-fill raises `fillOpacity` after the stroke · a path hidden at 0 (a round cap draws a
  dot) · a circle starts at three o'clock, rotated −90° for twelve · a connector's ends sit on real
  elements · `Paths.getPointAtLength` rides a dot on the stroke's head. Helper: Paths. Shelf:
  callout-arrow-note, callout-drawn-ring, diagram-flow, diagram-tree, map-route-line, map-world-flight,
  timeline-dated.
- **living-icon**: an icon's own parts moving (a clock's hands, a recording dot, data down a line).
  `Icons` draws a lucide icon whole; a moving part is drawn as SVG on the same 24-unit grid (stroke
  2, round caps) with the part separate, rotated with the SVG attribute `transform="rotate(deg 12
  12)"` (CSS `transformOrigin` on a thin line swings off-center). Rotation linear: a minute hand
  0.5 to 2 turns over the clip, a second hand 4 to 10. Pulse: dot 0.05 to 0.20, ring 0.04 to 0.12
  lagging π/2, 2 to 4 cycles over 3 to 5s. Dash flow: `strokeDashoffset` linear through a whole
  multiple of dash + gap. By hand. Shelf: live-badge, notify-banner.

## Emphasis and markers

- **hand-marks**: marking a word by hand is `Notation`, each mark wrapping its word and drawing on
  as `progress` runs 0 to 1, seeded. Highlight 0.5s gentle (several lines 0.3s apart) · Circle
  0.6s gentle · Underline 0.8s `Easing.inOut(Easing.quad)` · CrossedOff and StrikeThrough 0.3s a
  stroke, the second 0.15s later · Box and Bracket as highlight or circle. Props: `color`,
  `strokeWidth` (not on Highlight), `padding` as {left, right, top, bottom} ({top} on Underline), `iterations`, `roughness`, `seed`, `box: 'around'` on Circle. Across
  captions the mode changes every 2 to 3 groups for high energy, 3 to 4 medium, 4 to 5 low. Clean
  geometry instead of a rough line, by hand: a highlight bar painted before the word, 6px past each
  side, opacity 0.35, radius 3px, `scaleX` 0 → 1 from the left over 0.5s, −2° `skewX`; an
  elliptical border 130% × 160% of the word, 3px, rotated −3°, scaled in over 0.6s; a burst of 12
  lines at 30° steps, 3px, lengths 40 to 80px, each `scaleY` from its base over 0.4s, 0.03s apart; a
  scribble drawn by `Paths.evolvePath` with control points alternating y 0 and 24; a sketch-out of
  two lines at ±12°, `scaleX` from the left, 0.3s each, 0.15s apart. Helper: Notation. Shelf:
  callout-drawn-ring, title-spoken-hook.
- **spoken-word-glow**: words lighting as they are spoken on an attack, sustain, release envelope
  over each word's window, timings from the transcript in seconds from the clip's first frame:
  `env = t < start ? 0 : t < end ? Math.min((t - start) / ATTACK, 1) : t < end + RELEASE ? 1 - ((t
  - end) / RELEASE) * (1 - REST) : REST`, per word `textShadow` `0 0 (BLUR × e)px` and `scale(1 +
  BOOST × e)`. Default: attack 0.1 to 0.25s, release 0.2 to 0.5s, rest 0.15 to 0.4, blur 15 to 25px,
  size lift 0.03 to 0.10. Karaoke (what reads in video, 5 to 10 word phrases): rest 0.05 to 0.2, blur
  30 to 45px, lift 0.15 to 0.25. Color is `interpolateColors(e, [0, 1], [rest, active])`; the glow is
  `textShadow`, never `boxShadow`; windows climb and never overlap; the brand word's is 1.5 to 2×
  a normal word's; 1s of hold after the last. By hand. Shelf: title-spoken-hook, audio-spoken-quote.
- **Audio-reactive**: `useSound()` supplies timing and intensity; the vocabulary comes from the
  brand. Bass drives big moves (scale, glow, position), treble detail, overall level the globals.
  `const { bands } = useSound({ bands: 8 }); const bass = (bands[0] + bands[1]) / 2;`. Type and
  marks move at most 5% in scale and 30% in glow; a background can take 10 to 30%. Bands: 4 for a
  glow, 8 for a bar chart, 16 a detailed meter, 32 a dense radial. Bars, spectra and waves for a
  visualizer (an audiogram over a podcast, `{ follow: 'voice' }`), never as decoration; no strobing
  or rainbow cycling. Helper: useSound. Shelf: Audio (audio-podcast-card,
  audio-speaking-ring, audio-spectrum-edge, audio-beat-word, audio-hit-flash).

## Particles

- **confetti-pop**: confetti or dots as one event, each particle a pure function of time since
  launch from hashed values, `x = vx·t`, `y = vy·t + ½·G·t²`, spin × t. 10 to 18 for a pop, 24 to
  40 for a dissolve, never over 40 · G 900 to 1600 px/s² confetti, 0 to 200 dots · speeds 250 to
  700 px/s · cone 0.35 to 0.8 rad · flight 0.7 to 1.4s peaking 35 to 45% in · chips 5 to 14px, dots 4
  to 8px · spin 180 to 720°/s · fade the last 0.2 to 0.35 · stragglers (`i % 10 < 3`) launched 0.12s
  later, slower · fired by a cause, dead before the next beat, invisible before and after. By hand.
  Shelf: social-celebrate, social-milestone.

## The only sanctioned aliveness

A hold belongs to the next reveal, a camera leg, a count ticking or an icon's part turning;
breathing, floating, drifting and pulsing loops are not aliveness. Two moves remain.

- **settle-jitter**: for a frame truly settled that still reads dead, `Math.sin(2π × (t − settle) /
  period) × amp` added to the settled transform, starting at the settle so nothing jumps. Scale
  0.008 to 0.015 or 2 to 3px of y · period 1.5 to 3s (2.5 to 4s on a long hold) · on N elements each
  amplitude ≤ default / √N with staggered periods · a hold over 6s halves the amplitude and ramps to
  zero over its last 20%. By hand; the compile note about a sine of the frame is expected here.
- **soft-glow**: one bloom or one sheen. Bloom: a soft radial painted before the hero, 200 to 450px
  past its edges, rising over 0.6 to 1.4s gentle from 0.8 to 1.0 scale, landing on the hero's settle
  frame, peak 0.15 subtle, 0.30 standard, never above 0.45, darker and more saturated than the hero;
  never a boxShadow glow on the hero as well. Sheen: one band 15 to 35% of the surface wide (8 to
  15% on a wordmark), linear, 0.8 to 1.6s, off one edge to off the other, clipped by the surface,
  peak 0.10 to 0.25, about 105° on a logo, one pass. By hand. Shelf: logo-light-pass,
  texture-light-leak, lens-soft-glow.

## Handmade steps

A paper cut-out, stop-motion or hand-drawn look reads a stepped frame everywhere: `const H =
Math.max(1, Math.round((2 * fps) / 30)); const f = Math.floor(frame / H) * H;`, each pose held two
frames at 30, the same time at any rate, each element offsetting its timing by its index hash.
Stepped on the frame index, never on seconds. It textures motion that already has a job. By hand.
Shelf: quote-note, transition-paper-pile.

## Depth

The stage every 3D move stands on: one lens, `perspective: 700 to 1400` (800 to 1200 when the
camera moves) on a wrapper that never animates · `transformStyle: 'preserve-3d'` on that stage and
every wrapper between it and anything carrying `translateZ` or `rotateX/Y` (one missing and the
scene collapses to a 2D squash with no error) · a `filter`, an `opacity` under 1, an `overflow`
other than visible, a `clipPath` or a `mask` on the stage or a wrapper flattens it, so blur goes
on a leaf, the fade on the children, the background on the outer `AbsoluteFill`, clipping on a
child that carries no depth · an element's `translateZ` plus the camera's z stays under about 0.6 ×
the perspective · past about 18° of `rotateY` (55° of `rotateX`) type is texture, so anything read
lands near flat and holds 1s or more · paint order is depth inside `preserve-3d`, so a label is
pushed forward with `translateZ`, not `zIndex` · `backfaceVisibility: 'hidden'` on anything that
turns past 90° · one shadow direction for the whole frame, falling away from the side a card turns
toward. Depth is a spectacle beat, one per piece; most work stays flat. Shelf: Depth and 3D
(depth-flip-card, depth-deep-type, depth-double-doors, depth-turntable), device-floating-screen.

- **letter-flap**: the flipping-tile decode: each character hinges from 90° (`transformOrigin:
  'bottom'`, `rotateX` 90° → 0; top for a falling flap) while cycling glyphs, clicking onto its
  letter at 0.5 to 0.7 of its progress; flip 0.4 to 1.0s a character, stagger 0.03 to 0.08s, a new
  glyph every 3 to 6 frames, opacity `min(1, p × 2)`, monospace in `1ch` cells. By hand on the
  decode scramble. Shelf: depth-deep-type.
- **depth-layers**: echo layers at real `translateZ` steps (8 to 24px a copy, 4 to 6 copies) with
  the stack at `rotateY` 8 to 16° so the extrusion shows; entrance turns from 0° over 0.6 to 0.9s or
  grows the z steps from 0 over 0.4 to 0.8s, never both. By hand.
- **facing-pair**: two cards tilted 10 to 18° toward each other on a stage at `perspective` 1000 to
  2400, the entrance starting 4 to 12° past the tilt and settling onto it, each sliding in from its
  own side as the flat split cards do; badges on the stage, not inside a card; three cards put the
  middle flat at 0°. By hand. Shelf: depth-double-doors.
- **tumble-assemble**: the scatter and assemble with depth: `z = Z_NEAR − (i / (n − 1)) × (Z_NEAR −
  Z_FAR)` (Z_NEAR +150 to +450, Z_FAR −150 to −500), `rotateX = sin(a) × TUMBLE`, `rotateY = cos(a)
  × TUMBLE` (40 to 110°), the cloud turning 15 to 60° during the assembly and stopping with it, the
  resolve flat. By hand.
- **ring-flip-in**: each item parked at its place on the ellipse first, hidden, and flipped there:
  `rotateX` ±60 to 120°, `rotateY` ±45 to 120°, z −200 to −400, scale 0.2 to 0.6, resolving over 0.4
  to 0.8s on overshoot 1.2 to 2.0, staggered 0.06 to 0.12s; the center element pushed forward with
  `translateZ(180 to 240)`, landing after two to four items have. By hand.
- **tilted-page**: a page taller than its card, `rotateY` ±4 to 12° and `rotateX` 0 to 6° at
  `perspective` 800 to 2000, set once and held; the card carries `overflow: 'hidden'` and
  `preserve-3d`; the scroll measured from real section heights over 0.8 to 1.8s; a spotlight fading
  in over 0.4 to 0.8s after the scroll lands, above the content; the page real markup, not a
  screenshot. By hand. Shelf: device-floating-screen.
- **depth-flight**: the camera that dives into an angled surface and tilts to flatten. One lens on a
  static stage; the world in one `preserve-3d` div at its final size; a pose `{ x, y, z, rx, ry }`
  written as `translate3d(x, y, z) rotateX(rx) rotateY(ry)`, translate first. Fly in raises z, pan
  right sends x negative, tilt down sends rx positive. Legs: a dive 0.6 to 1.0s on slam · a
  flattening pull-back 1.2 to 2.0s on `Easing.inOut(Easing.cubic)` · a lateral pan 0.8 to 1.5s · a
  landing 0.6 to 1.0s on slam · holds 0.8s or more between legs. rx 30 to 55° for an angled surface,
  |rx| ≤ 65°, |ry| ≤ 30°; wide z −100 to −400, dive and landing z 300 to 700 at perspective 1000. Poses
  are authored and proved on a frame; blur belongs to leaf cards, 3 to 6px a depth step. By hand.

## Glass

- **frosted-panel**: `backdropFilter: 'blur(20 to 40px)'` with a fill the blur shows through (rgba
  white 0.10 to 0.20 on dark, 0.55 to 0.75 on light), a 1 to 2px lighter inner edge, a radius,
  `saturate(130 to 150%)` where the ground has color. It blurs everything painted behind it,
  footage included, and stops at the nearest ancestor with its own `opacity` under 1, a filter, a
  mask or a clip: so the glass panel fades itself, never a wrapper, and a glass graphic over footage
  takes no clip fade. A panel or two, never a full-frame sheet held for seconds. Over footage it is
  checked on a busy frame and a flat one. Shelf: name-glass-plate, callout-blur-box, cta-app-badges,
  audio-hit-flash, Lens effects (lens-focus-band, lens-into-focus, lens-overexpose).

## Blends

`mixBlendMode` composites a layer with what is under it inside the same stacking context:
**screen** on a dark ground (light adds: glitch copies, light leaks and glows read as light),
**multiply** on a light ground (ink darkens: a stamp, a highlighter, a printed overlap), **overlay**
and **soft-light** push contrast and wash color, one or none. The base copy stays legible on every
frame. A blended layer forces its own stacking context and flattens 3D around it, so blends and
depth stay in separate parts of the frame. Shelf: texture-light-leak, lens-soft-glow,
diagram-overlap.

## Not in this runtime

Real 3D geometry (a mesh, a light, a material) is three.js inside `<ThreeCanvas>`, as `skill-motion-graphics` shows; CSS
`perspective` only turns flat planes in space. A modeled hand still tops out at what three.js primitives draw. Per-pixel transitions
between two scenes, a star iris, tilt-shift and lens flare are not CSS anywhere.
