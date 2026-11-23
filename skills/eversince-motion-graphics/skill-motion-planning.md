# Motion graphics: planning

A shot is written as timed scenes before code, each window saying what is on screen, what moves
and where it sits. A shot of several scenes borrows one of the 22 shot plans below; a single beat
(one lower third, one stat) is two to four recipes from `skill-motion-recipes`.

# Scenes first

```
Scene 1 (0.0 to 1.0s): only what the words say now enters
Scene 2 (1.0 to 3.0s): the next piece arrives as the words reach it (a line, a card, a number)
Scene 3 (3.0 to 6.0s): everything has resolved; the read holds, still
```

- **One window per cue**, spoken or on screen: a line with two beats gets two windows, a list of
  five features five or six.
- **Every arrival has a time**, spread across the shot and its back half. The shot ends on the held
  read; on a short shot the last reveal and the hold are one window.
- **One signature move per shot**, on the key reveal; two is two shots. Only the last shot of a
  piece exits; every earlier one leaves by the seam into the next.
- **Each scene line names its job**: the layout (centred for a hero or climax, thirds, a split for
  a comparison, 60/40 or 70/30 for editorial, a triptych, a full-width strip), the device (before
  and after, a numbered run, a counterexample, a callback) and the beat it lands as (recognition,
  the aha, relief). On a composed frame the primary visual takes at least 40% of the frame over at
  least three depth layers, and one element dominates by two or more of size (3:1), weight (900
  against 300), contrast, position (the upper third) or motion.
- **Times the build cannot misread**: two things "at 4s" overlap for frames, so one is gone by 4.2s
  and the next starts at 4.2s. A count starts in the shape it lands in ("counts up to $4.2M", never
  "$0 to $4.2M"). Layers are listed back to front, the order the siblings are written.
- **The clip is as long as its content**; a longer length leaves seconds the build has to invent.
- **Left out**: interface furniture (navigation, footers, scroll bars, the browser's frame), a
  placeholder shape where a real asset belongs, drifting bokeh and the violet-to-blue wash, unless
  the shot rebuilds an interface on purpose.
- **Captions share the frame.** Small readable text (a label, a stat's caption, a URL) keeps out of
  the span the caption line covers; imagery, cards and full-bleed subjects may run under it. The
  composition sits on the frame's true centre; with no captions there is no band. At 9:16 the
  platform's interface holds the bottom quarter and the top eighth (`skill-animated-captions`).

In code each window's start is one constant, seconds times fps, and every move reads its start from
one of them, so retiming a scene changes one number.

A piece of several shots writes what they share once: the palette's roles, the motion grammar (one
easing family, the reveal model, what may move during a hold), which shots are the held breathers,
what never appears. Each shot's plan carries only what differs. Emphasis inside the palette is
inversion, scale, weight or density, not a third color. A motif planted early (a dot, a mark, one
color) returns later, fuller, as the payoff; the plan names both appearances as the same element.

**A storyboard or deck is a set of states to move between, not pages to flip.** An element shared
by neighbouring frames (matched by name, then by size and place) animates between its states;
whatever appears or leaves does so on its own frame's beat. Notes under the frames are motion
direction, not on-screen copy. Successive states of one app screen are interactions (the cursor
acts, the screen answers), not tweens. Stills cut one after another are the fallback for frames
that share nothing.

## Fill, bend or build

The role menu names a shot plan for the beat, then one of three:

- **Fill it.** The product's words, numbers and assets take the shot plan's places; the timings
  stay.
- **Bend it.** The shape is right, the content, count or surface is not: one line on what stays
  and what changes, then the scenes. The signature move stays (a shot plan that cannot keep it was
  the wrong pick) and the reveals stay spread out.
- **Build it.** No shot plan suits: recipes, every arrival timed to the words. A shot plan forced
  onto content that does not fit is worse than none.

```
number-first, bent: the count-up ring stays; one figure replaces the three, and the chart becomes
a pile of discarded takes that grows.
Scene 1 (0.0 to 1.2s): dark field; ring and number in the middle, about 50% of the frame; the
  ring fills while the number climbs to 20, both on one ease.
Scene 2 (1.2 to 3.4s): the voice says the figure and the camera travels through the ring's
  opening to the pile of takes at lower left, which gains a take each beat beside a ticking
  counter. 60/40, three depth layers.
Scene 3 (3.4 to 5.0s): the main stat card settles in the middle over a soft bloom and holds.
```

## The role menu

The story decides which beats exist; the menu names a shape for a beat already there. Where two
suit, the one whose moves sit closer to the scenes; where none does, build.

- **Opening**: word-beats (one word changing in place; a statement climbing beat by beat; three
  beats alone on one field) · keystroke-line (a line the viewer would type, typed live, folded
  away for the brand) · roll-and-crash ("for X, for Y" rolling until the hero barges in) ·
  pinned-line (a lead line that never moves over a carousel of who it is for) · mark-to-button
  (one widget alone on a flat field changes shape and does its job) · ask-and-answer (close on the
  input, type the prompt, cut on submit) · number-first (one figure bursting to full size while
  icons scatter to their places) · ring-around-core (tools pop into a circle, the camera closes on
  the middle) · station-pan (dated milestones along a line, panned up to today) · wide-reveal (an
  unexplained close detail pulls back to show the tool) · pointer-walkthrough (named cursors of
  several people editing one canvas as a headline forms).
- **Pain**: word-beats (3 to 5 short lines of frustration, each alone; or questions passing through
  the centre until one climax word) · number-first (a figure getting worse, then a push into the
  chart) · crowded-frame (tools stack up, the viewer is in the middle, tasks close in) ·
  station-pan (steps that do not connect, ending in a knot).
- **First look**: word-beats (build-up beats landing on the name; a headline holding while one
  word changes; a sentence assembled a word at a time; the wordmark changing shape) · logo-forming
  (a sting with no words, or the mark beside a line that sets it up) · quiet-title (three dark
  cards: logo, name with version, tagline) · pointer-walkthrough (a cursor showing the surface for
  the first time) · ask-and-answer (a product that is mostly its input box) · held-screen (one
  surface runs the main loop start to finish) · number-first (a grid of cards scrolls a headline
  metric into the middle) · clip-to-stat (footage of the product moves aside for a key figure) ·
  station-pan (an idea stated, one pan into the working product).
- **Feature in use**: tile-cascade (a grid of feature tiles assembles; a table fills in) ·
  pointer-walkthrough (a whole task ending on the main button; steps whose final approval is the
  peak) · ask-and-answer (a prompt and what comes back) · work-underway (the agent at work, a
  checklist ticking itself off) · tweak-and-watch (a control adjusted and its target changing in
  the same frame) · held-screen (screens stepping through the feature) · log-to-result (a record of
  work scrolls by, one click opens what it produced) · number-first (a headline cutting between
  charts that draw themselves) · two-up (two capabilities of equal weight side by side) ·
  clip-to-stat (a clip gives way to the number it moves) · camera-route (a sweep across the surface
  ending on the button).
- **Proof**: quiet-title (a crowded opening cleared to a clean lockup and "in use at N+ studios") ·
  ring-around-core (partner logos circling the mark; about 20 icons around a headline) ·
  tile-cascade (a wall of logos pulling back to a huge ecosystem) · number-first (a single gauge
  counting inside a run of type).
- **Value**: word-beats (a rapid run of 8 to 12 phrases; 2 to 4 statements each held 1.5s or more,
  each arriving its own way) · quiet-title (two calm lines: the pause in the piece) · pinned-line
  (one surface stays while its whole look changes each beat) · tile-cascade (a list of gains that
  builds) · camera-route (something small done here changes something there, the camera travelling
  between) · wide-reveal (small actions close up, one pull-back shows how much got done) ·
  pointer-walkthrough (a demo, type, another demo).
- **The ask**: word-beats (a final line landing beat by beat on the logo or address) · logo-forming
  (the logo builds and the camera pushes through it to the address; a button outlines itself before
  the wordmark) · mark-to-button (the mark shrinks into the button, a cursor presses it) ·
  quiet-title (statement, ask, logo on hard cuts) · ring-around-core (a click pulls the circle
  inward, the demo bursts out) · ask-and-answer (the install command typing into a terminal pill).
- **Sign-off**: word-beats (a volley of verbs landing on the one word the brand owns; an address
  held long) · keystroke-line (the mark stays while the ask types under it) · logo-forming
  (everything clears and the lockup draws itself) · roll-and-crash (options roll, the mark barges
  in) · pinned-line (the wordmark fixed as quotes rotate beside it, settling into the lockup).

## Reading a shot plan

Each gives its roles and usual length, its scenes with timings, its signature move, the recipes
behind its moves by name (`skill-motion-recipes`), and what breaks it. Times are seconds in the
clip's own time; `placement` takes them as duration_seconds. Where a variant turns in depth, the
flat version does the job for anything that must be read; the depth recipes are in the recipes
part.

Shared by every shot with a product surface, a camera or a cursor:

- **A product surface is built or placed, never implied or invented**: rounded rects for panels,
  muted bars for lines of text, `Icons` for glyphs, real words only where the eye reads; or a real
  screenshot from the library as `Img` in a `Sequence` with `premountFor`. Moving content is
  clipped at its card (overflow hidden on the card, the moving layer inside); the background belongs
  to the scene, never to the layer that moves. A raster inside a zoomed world needs a source at
  least its rendered size times the largest scale it reaches. A change is shown by the strongest
  honest route: the real interface, rebuilt or placed; an analog whose behaviour is the claim
  (caching as two identical request rows, the second finishing at once; batching as ticks drawing
  together into groups; a speed-up as a counter and bar on an in-ease); a terminal the command
  types into; last, a checklist of six rows at most. An interface implying a screen the product
  does not have is invented.
- **The camera is one `<Camera>`**, the whole world inside it, every move a leg: one state, one
  transform about the frame's centre. Hard out-eases on dives and landings, in-out on
  repositioning; no spring or back-ease on a camera.
- **A cursor is `<Cursor>`, in the interface's own coordinates**, placed from the same constants as
  what it clicks and painted last. It arrives decelerating, lands a few px off its target's centre,
  and the target compresses with it on the press (`Cursor.press`).
- **Depth** (a tilted card, a camera flying in perspective, cards opening like a book) stands on the
  stage in the recipes part, each depth variant paired with its flat form. A 3D object (a device, a product) can be
  built in three.js inside `<ThreeCanvas>` (`skill-motion-graphics`); a lifelike modelled hand is beyond its primitives.

## The shot plans

### word-beats: the motion is in the wording
Opening, pain, first look, value, the ask, sign-off. 3 to 13s: a rapid value run of 8 to 12 beats
under 0.5s each lasts 3.5 to 4s; a held-line reveal from 3s; a closing address card up to 12.6s.
Flat, heavy type in the middle of a solid field, the camera fixed. Three mechanisms: **A, held
line, one slot**: the sentence stays and a single slot changes on a hard cut with no roll and no
blur, or the last word is deleted and typed again. **B, line after line**: every beat cuts to a
fresh line, can swap a light field for a dark one with the type reversing, and enters its own way (a
big blurred slam settling to size, a sideways snap, a rise with a slight turn, a masked slide up, a
soft fly-in coming into focus). **C, handoff**: phrases arrive at the centre in turn, the one on its
way out shrinking and parting toward both sides; the peak word gets one treatment of its own (a
gradient settling to flat, a small -8° tilt), or the question is answered by the product surface
sliding in as an element.
- Scene 1 (0.0 to about 1.0s): the opening beat arrives one way: typed with a caret, a flash on a
  hard cut, a blur-in word by word, or a word far too big shrinking to fit; an accent picks out the
  key word (an underline drawn in, a strike, a box).
- Scenes 2 to N: each beat takes the last one's place and nothing is left over. A rapid run allows
  about 0.4s a beat; a handoff of statements 1.5s or more each; a volley of verbs every 0.2s. Beats
  sit 1.2 to 1.8s apart for line after line: under 0.8s frantic, over 2.5s loses the pulse.
- Scene N (last beat to end): it arrives and stays, never scaling away. A payoff pops on (the
  logo, a mark), an address card stays longest, the brand's word holds about 0.5s.
Shapes within it: **three alone** (three beats each by itself on one field, one may be a logo, a
button or a chart; the third stays) · **sentence build** (color takes the frame first, the sentence
assembles a word at a time, every key word with one effect of its own) · **name becomes mark** ("now
meet", or the old name, becomes the wordmark on the spot; screenshots fly outward from behind it at
their own speeds) · **held statements** (2 to 4 statements, 1.5s or more each, each built
differently) · **joke run** (3 to 5 beats each with its own joke, then the logo gathers out of a
glow) · **dive out** (the peak beat exits with the camera diving into a huge letter).
**Signature**: the change in place, a word swapping inside a line that holds. **Recipes**:
typed-states, beat-lines, pop-arrival, ghost-trail, Notation marks, color-through-type,
split-shudder, same-spot-swap, count-and-grow with value-bars. **Breaks it**: the same entrance on
every beat; a beat still visible under the next; a roll or blur on an A swap; the final beat scaling
away. Shelf: Titles (title-typed-hook, title-setup-payoff, title-bold-statement), Text effects.

### keystroke-line: a person at the keys
Sign-off, opening. 3.6 to 7s.
- Scene 1 (0.0 to about 2.0s): on a solid field a caret blinks where the line will begin, then the
  line types a character at a time with the caret following. Opening: the frame holds nothing
  else, or the typing happens in an input pill that slides left as it fills. Sign-off: the mark is
  in place from the start and never moves, with one flourish (an icon stroke drawing in, fine rings
  spreading); the typed line sits under it.
- Scene 2 (about 2.0 to 4.5s): the line is edited where it stands. Opening: the final word deleted
  and another typed, or the last word flips to the accent. Sign-off: the line goes by a hard
  replace or a travelling mask, never a backspace, and the mark stays.
- Scene 3: opening, the caret disappears and the line shrinks to a point (0.3 to 0.7s), then the
  brand pops on, the mark first, the wordmark sliding out from behind it under a mask; or a control
  pops on, a cursor finds it, and a 150ms change of state fades it to the accent. Sign-off: the ask
  types into the line under the mark, an accent ring settles around the mark, the frame holds.
**Signature**: typing and correcting the way a person does; the correction is what convinces.
Typing is the string sliced by frame; a typo or backspace is one more state in a list of (time,
string) pairs; the caret is a square wave, solid while typing and blinking only at rests.
**Recipes**: typed-states, colored-caret, follow-the-caret, line-draw-on, same-spot-swap,
pop-arrival, pointer-click, soft-glow. **Breaks it**: typing at a machine-even rate with no edit;
the brand arriving before the line has cleared; the sign-off mark moving. Shelf: Code
(terminal-command-run, code-one-line), Screen demos (screen-typed-search).

### quiet-title: a single quiet move, then calm
Value, proof, the ask, first look. 3 to 5s; a run of cards 2 to 3s each, 5.5 to 9.5s in all. The
calm is what it delivers.
- Scene 1 (0.0 to about 0.3s): the field at rest, empty for value; for proof, a crowded collage of
  app cards under a line that sets it up.
- Scene 2 (about 0.3 to 1.5s): the arrival. Value: the first line fades in at the centre, growing
  95% to 100% on a smooth out-ease. Proof: a big accent pill crosses the frame on a diagonal, lower
  left to upper right and out through the corner, its clip clearing the collage as it passes and
  uncovering the lockup while the icon's strokes draw in.
- Scene 3 (1.5s to end): the hold. Value: the first line lifts and fades and the second comes up
  from below into the centre as the first leaves, one move. Proof: icon, wordmark and "in use at N+
  studios" (the N counting up) settle and stay.
- Card run (the closing cards of an ask, the opening cards of a first look): 2 to 3 cards, each
  arriving, making one move and holding. Cards change on instant hard cuts at full opacity, or by a
  blur that clears as the next card comes sharp. The final card is the logo and stays to the last
  frame.
**Signature**: the single move and the stillness after it. The title's enter, hold and exit are one
interpolate with an easing per segment (exit about 75% of the entry). **Recipes**: same-spot-swap,
typed-states, line-draw-on, count-and-grow, pop-arrival, rack-blur, soft-glow; the diagonal wipe is
a `clipPath` polygon. **Breaks it**: a second development phase; anything manufactured to fill the
hold; cards flipping at sub-second tempo (that is word-beats). Shelf: Titles (title-opening-title,
title-episode-card, title-section-divider).

### roll-and-crash: contact, never a crossfade
Opening, sign-off. 5 to 7s.
- Scene 1 (0.0 to about 1.4s): an opening phrase types cleanly with no mistakes.
- Scene 2 (about 1.4 to 3.0s): one accent word in the line rolls upward through 2 to 3 alternatives,
  a new word each tick; past 3 it is padding.
- Scene 3 (about 3.0 to 4.2s): the hero arrives from beyond the frame at speed and pushes the whole
  line out of its way; the words are knocked aside by the hit and never faded. The hero lands with
  weight on a long `Easing.out(Easing.cubic)` settle.
- Scene 4 (about 4.2s to end): only the hero, in the middle, still.
**Signature**: the push. How far the line moves follows the hero's own progress from the frame they
touch, and lasts 40 to 50% of the hero's travel; blur on the impact peaks at top speed and is gone
by the landing. **Recipes**: typed-states, slot-roll, knock-aside, ghost-trail. **Breaks it**: a
crossfade where there should be contact; a light, quick landing; a slot rolling through more than
three.

### pinned-line: one thing fixed while the rest rotates
Opening, value, sign-off. 6.6 to 11s; the rotation itself 3 to 5s. The camera never moves.
- Scene 1 (0.0 to about 2.0s): the anchor (a wordmark, an input box, an opening line) arrives once,
  by a fade, a scale or a word at a time, or is there from the first frame, and does not move
  again. An interface anchor holds its exact text and a blinking caret.
- Scene 2 (about 2.0s to 70%): the rotation. **A, the space next to it**: a chip lands on top of the
  previous one on a hard cut, resizing to each phrase and extending away from the anchor; or a
  highlight moves along the line, one word lit; or a quick vertical carousel at about 0.5s a phrase.
  Either an even pace (0.5 to 1s a state) or a burst speeding up from 1s a beat to 0.15 to 0.3s a
  change: 12 to 16 states say "everyone", 3 to 8 say "a named few". **B, the entire look**: every
  1.3s the background, type, radii, icons and logos all fade (0.3s) into another product's styling
  with the anchor's text unchanged.
- Scene 3 (70 to 85%): the accent beat. The whole line lights at once; the burst stops on its
  strongest phrase; (B) a faint dot grid fades in as the interface drains to a still; (opening)
  the anchor clears for 1 to 3 statement beats.
- Scene 4 (to end): one last element joins the anchor, which still has not moved, and the lockup
  stays for 20 to 30% of the length.
**Signature**: the anchor that stays put. A burst is a list of states whose holds shorten by a
constant ratio. A change of look is N full layers stacked in identical geometry and crossfaded on
opacity alone, the anchor drawn once above them. **Recipes**: typed-states, slot-roll,
length-timed-cards, word-drop, pop-arrival, colored-caret, skin-swap. **Breaks it**: the anchor
breathing, drifting or re-laying out; the rotating region touching it; a push-in for energy; a
collision (that is roll-and-crash).

### logo-forming: the mark comes to exist, then locks up
First look, the ask, sign-off. 4.4 to 11s. Three stages: empty the frame, make the mark, settle the
centred lockup and hold. How the mark appears: **stroke by stroke** (feature pills leave by all four
edges, each moving out from where it sits; the mark draws a stroke at a time, the wordmark uncovers
beside it) · **grown from nothing** (a serif tagline fades on left to right, holds, goes; the
complete mark grows in the exact middle with a slight turn, eases left while the wordmark uncovers)
· **pieces converging** (a glowing dot draws a circle, half-circles overlap, strokes swing in and
become the icon; the wordmark slides out from behind) · **phrase into mark** (a phrase in the middle
changes where it stands, then swaps on the spot into a line icon whose strokes lock together as the
mark; the lockup holds for the final 40 to 50%) · **drop and slide** (the icon falls in from the
top, letters slide in in turn, the closing punctuation lands) · **outlined button** (a pill rises and
its outline traces itself with a spark leading it; a slanted band covers the frame and swaps the
ground color, then narrows to an accent slash; the wordmark types beside it) · **line then mark**
(a supporting line types first, the mark pops in beside it, the line departs as the layout
re-centres, the camera closes in) · **ring pulse** (rings spread from a mark that does not move,
the ground fades light to dark, dots on the rings circle the centre and slow to a stop) · **already
there** (the lockup from frame one; shapes drift away, an underline runs, the tagline wipes on) ·
**through the mark** (the camera rushes through the gaps in the mark with sideways blur, letters of
the ask flying past, landing on an address wiped on from the left).
**Signature**: the mark coming to exist, then the lockup. Strokes are `Paths.evolvePath` with any
fill fading in only after its stroke; a morph between outlines is `Paths.interpolatePath`; a
wordmark "from behind" is a clip reveal under a mark painted later in the markup, its disc filled
with the background so the letters really come out from behind it. The wordmark begins no sooner
than 0.2s after the final stroke. **Recipes**: line-draw-on, pop-arrival, word-drop, typed-states,
slot-roll, same-spot-swap, spoke-spread, ghost-trail, camera-phases, aim-zoom, count-and-grow,
confetti-pop. **Breaks it**: cutting from the phrase to the finished logo; the wordmark fighting the
drawing; a stroke that overshoots (a pen does not bounce). Shelf: Logo reveals.

### mark-to-button: from the mark to the button
The ask, opening. 4 to 7.5s. The camera stays still.
- Scene 1 (0.0 to about 1.4s): the mark in the exact middle; a title beneath it does not move.
- Scene 2 (about 1.4 to 2.4s): the mark shrinks and fades at the very moment a smaller, brighter
  button grows where it was, both positioned absolutely around one centre, so the eye sees one thing
  changing shape.
- Scene 3 (about 2.4 to 3.4s): a cursor pops into view with no fade, travels in from outside the
  frame slowing as it comes, and stops a few px short of the button's exact centre.
- Scene 4 (about 3.4s to end): cursor and button squeeze together (to 0.9 and back) and let go with
  a ripple or a glow. Hold on the pressed state.
- As an opening: a single pill or chip on a flat field changes where it stands, unfolding into a
  menu or springing open into a prompt card; it does its job (placeholder text types, a control
  switches from muted to vivid); then it disappears and a closing title types onto the empty frame.
**Signature**: the mark shrinking into the button on one shared centre. **Recipes**:
same-spot-swap, heavy-press, pointer-click, soft-glow, edge-grow, shape-morph, typed-states,
colored-caret, pop-arrival. **Breaks it**: a button in normal flow that pushes the mark aside; a
cursor at even speed straight onto the exact centre; several steps (that is pointer-walkthrough).
Shelf: Calls to action (end-card-button), Social (social-subscribe-press).

### tile-cascade: the sheer amount
Feature in use, proof, value. 3 to 10.5s.
- Scene 1 (0 to about 1.0s): the area set up empty, then items land 0.04 to 0.08s apart, each
  fading in and sliding or growing a short distance directly into place; nothing scatters or
  bounces. A headline can come first, a supporting figure counting up in the accent.
- Scene 2 (1.0s to X): the remaining items land, the layout completes and stays still; a single
  sheen passing over the tiles is all the hold needs.
- Scene 3 (X to end): hold; a final line; the area clearing for a payoff (a price snapping in via a
  brief in-between state, an address uncovering from the left); or a push into one key item that
  opens onto a far larger field of words receding to show more before clearing.
Variants: **tile grid** (icon and label tiles, staggered two columns or 3×3) · **list of gains** at
about 1 item a second, built ADD (a marker pops, a tick draws, the text wipes on, every line stays
lit), STEP (the stack moves up one place per beat, about 0.1s eased, the newest line bright and the
others dimmer with distance) or SCROLL (a quick scroll through the focus slowing to a stop) · **a grid
filling in** as a table (placeholder pills fill left to right, staggered top row down, each turning
into its value, status pills changing after) or pinned to places (data cards arriving in turn, each
joined by a drawn line to its point, a ring spreading once as its card arrives) · **wall of logos**
(all at once in a shuffled order, `(i * 37) % n`, or column by column, then one zoom-out) ·
**frosted cards** (close on 2 to 3 icons, a zoom-out as cards grow from behind them and the icons
shrink about 50% to become headers, the middle card settling about 5% bigger and in front).
**Signature**: a staggered fill into place that reads as one arrival, 0.35s first to last for six
tiles. **Recipes**: pop-arrival, spoke-spread, slot-roll, line-draw-on, value-bars, typed-states,
count-and-grow, soft-glow, rig-camera, face-ring, ghost-trail. **Breaks it**: a stagger longer than
0.5s (a queue); 8 or more items flying out of one centre. Shelf: Lists and timelines, Galleries
(gallery-tile-build, gallery-picture-wall).

### ring-around-core: everything around one centre
Opening, proof, the ask. 5 to 8s.
- Scene 1 (0 to about 1.5s): the main nodes (circles holding an icon and a label) pop in one after
  another into a broad circle around a centre that is blank or holds the mark.
- Scene 2 (about 0.7 to 2.5s, overlapping): smaller chips fill the spaces between; thin lines or a
  circular track draw outward from the centre.
- Scene 3 (2.5s to end), one ending, then hold. **Close in**: the camera moves to the centre, near
  nodes staying sharp as far ones slide off and blur. **Circle round**: the centre mark lands, a
  circle draws, badges pop onto it and travel clockwise to their places, upright, easing to a stop
  as a zoom-out finishes. **Pull-in** (the ask): icons sit motionless around an empty button; a click
  draws the whole circle inward to the cursor as the demo bursts out. **Icon field**: about 20 icons
  pop in across the frame around a serif headline on two lines, settle and hold. **Gathering**
  (proof): a logo in the centre collects endorsements one by one, faces fall into the circle with a
  line drawing to each while partner logos scroll along a strip at the bottom.
**Signature**: the circle assembling, then one resolve onto its centre. Nodes settle smoothly; a
playful brand may overshoot 1.15 at most. A 2D orbit is position from one eased angle,
`cx + R * Math.cos(a0 + SWEEP * p)` with the y term scaled for an ellipse, which keeps badges
upright; it revolves into place and stops. **Recipes**: pop-arrival, face-ring, line-draw-on,
rack-blur, camera-phases, aim-zoom, spoke-spread, pointer-click, letter-flap (flat), typed-states.
**Breaks it**: a centre a few pixels off the hub; all nodes landing together. Shelf: Diagrams
(diagram-hub, diagram-rings), Galleries (gallery-picture-ring).

### two-up: two things weighed at once
Feature in use. 4 to 6s. Two soft glows, accent A at about 30% across and B at 70%. The camera does
not move.
- Scene 1 (0 to about 0.8s): a centred title with one accent word comes down into position, the
  vertical stroke of a T the cards complete.
- Scene 2 (about 0.4 to 1.9s): the first card in from the left and the second from the right 0.2s
  later, each growing 0.85 to 1 while the title is still finishing, so the three read as one
  entrance; shadows fall away from the centre.
- Scene 3 (1.9s to end): a pill badge lands where each card faces the other, left first, right 0.3s
  after, about 15% over its card, the only overshoot. Hold.
**Signature**: two cards arriving as mirror images. **Recipes**: facing-pair (flat), pop-arrival,
soft-glow. **Breaks it**: a third item (that is tile-cascade); steps in a sequence; both cards
landing on the same frame. Shelf: Comparisons (compare-split, compare-versus, compare-plans).

### crowded-frame: surrounded on every side
Pain. 6 to 10s.
- Scene 1 (0 to about 1.6s): three familiar app surfaces come together, the middle at full size and
  the two beside it at about 0.86.
- Scene 2 (about 1.6 to 3.0s): app icons land scattered around them.
- Scene 3 (about 3.0 to 4.6s): the middle surface changes shape, its contents fading as its frame
  reforms (scaleX and scaleY, never width and height), and underneath is the viewer's avatar.
- Scene 4 (4.6s to end): bubbles of tasks move in from every side, staggered around the circle,
  while the avatar stays put. Hold on the crowding.
- Ending on a question: notes, windows and alerts pile up while the camera slowly pulls back until
  nothing else fits; one fast push-in knocks the pile to the edges in a single frame, a serif
  question assembles a word at a time in the space that opened, and a cursor drifts in to rest under
  it.
**Signature**: the middle turning out to be the viewer, then everything closing in. **Recipes**:
pop-arrival, shape-morph, spoke-spread, knock-aside, camera-phases, typed-states. **Breaks it**:
pushing in while things close in ("surrounded" reads as "zoomed into"); both endings in one shot.

### number-first: the numbers carry the argument
Opening, pain, first look, feature in use, proof. 4 to 12s (a single gauge about 2.5s inside a run
of type).
- Scene 1 (0 to X): a single instrument in the middle. The number counts as it grows to full size
  from 40 to 60% of it, and its ring or bar fills on the same easing over the same time: one beat.
  The label is in before the count tops out; a suffix (%, ×, +) slides on once the number has
  landed.
- Scene 2 (X to Y): the camera travels through the opening in the ring, or scrolls, to the next
  instrument: a trend line drawing across grid lines with the area under it filled, a second figure
  counting.
- Scene N: the main metric settles in the exact middle with a glow behind it (opacity 0.45 at
  most), and holds.
Variants: **icon burst** (3 to 5 icons pop in bunched at the centre, then the number bursts to full
size as they fly out to their places) · **getting worse** (ring, push through, trend, then a pan
across a grid of avatars where some dim while others get ticks) · **scroll to the figure** (a grid
of cards scrolling its key metric into the middle beneath a tagline building a word at a time,
finishing on a soft push-in that goes a little too far and settles back) · **chart cutaways**
(headlines cutting between charts that draw themselves, the line climbing past its band with a dot
and a pill label at the high point) · **single gauge** (one gauge counting inside a run of type,
arriving out of blur and leaving past the lens).
Data shots beyond the plan: stats that belong together keep one layout and one look, only the value
changing; every number gets a visual beside it that gives it weight (a fill, a ring, a shape sized
by the value), never a figure alone in empty space; a ranked race (bars overtaking) names which
pairs cross and on which beat; the user's own product photo or footage can carry the data, its
geometry the instrument, the palette sampled from the picture, its parts measured on a frame
(`skill-motion-overlays`).
**Signature**: the count and its graphic landing as one beat, then the move through to the main
figure. The count runs 1.2 to 2.5s on `Easing.out(Easing.cubic)`; a harder ease freezes the last
digits. **Recipes**: count-and-grow, value-bars, line-draw-on, chart-read-head, soft-glow,
rack-blur, camera-phases, aim-zoom, ghost-trail. **Breaks it**: a bouncing number; a count under
0.8s (a flash); a count that misses its target (round it, clamp the ramp); digits jittering without
tabular numerals. Shelf: Stats and progress (stat-big-count, stat-ring-percent, stat-then-now),
Charts (chart-build, chart-marked-growth, chart-share-bar).

### clip-to-stat: show, yield, pivot, stamp
First look, feature in use. 6 to 8s. A real clip from the library, sound off.
- Scene 1 (0 to about 1.6s): the video settles in the middle on a smooth grow.
- Scene 2 (about 1.6 to 3.2s): it slides to one side and gets smaller, making room for the key
  figure, which comes in as layered type: attention passes from one to the other in a single move.
- Scene 3 (about 3.2 to 5.0s): both go; a bold line types into the empty middle, its accent words
  saying what the video showed.
- Scene 4 (5.0s to end): a gradient pill closes around the last line (scaleX 0 to 1), its glow
  settling a beat later. Hold.
**Signature**: attention handed from clip to figure around one shared point. The clip is
`<OffthreadVideo muted>` on the item's url in a rounded wrapper with overflow hidden. **Recipes**:
stacked-type (flat echo layers), same-spot-swap, typed-states, soft-glow. **Breaks it**: a cut where
the video should slide away; the video and the figure moving as two events.

### station-pan: one camera, stations laid out ahead of it
Opening, pain, first look. 7 to 10s, about 1.0s held at each station, every station laid out in one
world larger than the frame.
- Scene 1 (0 to about 1.0s): the first station in the middle; its reveal finishes and the pan
  starts.
- Scenes 2 to N-1 (about 1.0s each): a pan eased at both ends brings the next station to the
  middle; its label uncovers as the camera gets there.
- Scene N: the final pan arrives; the last callout uncovers and stays, the camera at rest.
Variants: **timeline** (markers along a thin line across the lower third, the camera only ever
panning left, callouts popping out of their triangle with a year rising, ending on today) ·
**tangle** (stations spread about, the camera panning diagonally along a rough accent line that
draws on toward each next one, ending on a scribbled knot) · **idea to demo** (two shots linked by
one sideways pan with parallax, a phrase whose accent word unscrambles, then someone typing in the
live product).
**Signature**: a single camera moving across stations placed in advance; nothing at a station moves
until the camera reaches it. The rough lines are `Notation` or `Paths.evolvePath`. **Recipes**:
rig-camera, camera-phases, aim-zoom, pop-arrival, line-draw-on, typed-states, letter-flap (flat).
**Breaks it**: a push-in; each leg on a different ease. Shelf: Lists and timelines (timeline-dated,
list-steps-row, timeline-roadmap), Maps (map-stops-journey).

### wide-reveal: the detail was a small part of the whole
Opening, value. 6.8 to 11s. Never zoom in, zoom out exactly once, the camera still after.
- Scene 1 (0 to about 2.5s): a detail filling the frame, no interface edges in sight, doing
  something. A: the graphic reshapes while the pull is already under way. B: rows pop on and a
  highlight jumps from row to row with no glide, while the camera stays or drifts down the list.
- Scene 2 (2.5s to the reveal): A: the pull settles on an in-between composition that still fills
  the frame. B: the close view moves on: a new row, a hover.
- Scene 3, the reveal: A: the end of that same pull (4.3 to 4.5s of travel inside 6.8s), slowing
  exponentially, arrives at a design tool's workspace. B: a sharp move of 0.5 to 1s, slowing hard,
  arrives at a workspace with several panes. The pull finishes before the shot ends.
- Scene 4 (lock to end): only elements move now: a cursor clicks, a playhead scrubs while the canvas
  plays in time, a pane changes through a brief flash to white.
**Signature**: one move outward that changes the meaning of everything shown so far. The whole
workspace is laid out in its final positions in one world, opened at scale S0 (usually 4 to 12×)
centred on the detail, eased down to 1 on one curve, `Easing.out(Easing.exp)` or
`Easing.out(Easing.poly(5))`; anything visible at S0 is text or vector. **Recipes**: rig-camera,
aim-zoom, rack-blur, pop-arrival, pointer-click, edge-grow. **Breaks it**: animating letters the
pull is already moving; a cut where the pull should be. Against tile-cascade: items assembling then
a pull-back placing them in context is tile-cascade; a world complete from the first frame whose
single outward move is the story is this one.

### camera-route: the camera tells the story
Value, feature in use. 5.6 to 11s. **A, cause and effect**: dive to a panel, something is
triggered, sweep over to what it changed. **B, no cursor**: the camera flies over content that runs
by itself.
- Scene 0 (optional, 0 to about 1.8s): a still opening, ended by a hard cut or the headline
  shrinking off as the first dive begins.
- Scene 1 (about 0.5 to 2.0s), dive: quick and close onto the element that matters.
- Scene 2 (about 1.5 to 6.0s), the turn: the camera waits while the content does something. A: a
  cursor presses send and the element empties. B: a dropdown opens on its own, highlights a row,
  closes on the choice.
- Scene 3 (about 4.0 to 8.0s), the journey. A: a sweep to the second region as the result builds
  there (bars dropping while a dotted line draws, a response growing row by row). B: a pull-back, or
  a blurred whip that becomes a flat pan.
- Scene 4 (the final 1 to 2s), arrival. A: the camera settles on the new state with the cursor
  drifting toward the payoff. B: an abrupt push onto the main button, scaled huge, which may cut off
  before it lands.
**Signature**: every leg has a reason and a different verb; four matching pushes is a slideshow.
**Recipes**: rig-camera, camera-phases, aim-zoom, ghost-trail, rack-blur, three-speed-slide,
pointer-click, value-bars with line-draw-on at the same moment, edge-grow with knock-aside,
detect-box. **Breaks it**: a leg with no reason; a camera following the cursor around (that is
pointer-walkthrough).

### pointer-walkthrough: the pointer does the work
Feature in use, first look, opening, value. 4 to 13s.
- Scene 1: the surface appears; the cursor comes in and works the first target, and the interface
  responds on the same beat.
- Scene 2, with one camera approach throughout. **Follow**: push in and pan, or whip, to each
  target. **Fixed frame**: each click sets off a change in an element (a dialog springing out of the
  middle, a side panel sliding in, a table filling a row at a time, a graph of nodes spreading).
  **Single sweep**: one continuous zoom curve while the cursor drags a see-through copy of a chip and
  drops it in place, with a selection box and handles.
- Scene 3: the cursor settles on the main action and a hover highlight pops behind it, or the
  result springs on; hold.
- Shapes: **sign it off** (a list of steps moves from waiting to in progress to done in hard state
  changes, and a button receives the press as the payoff, turning to the success color as a tick
  lands) · **inspect** (the first click brings up a toolbar with labels, the view widens from a
  close crop to the full page, the cursor sweeps across with a small inspector panel trailing it,
  outlining every element it crosses) · **wander** (the cursor drifts over one page and its controls
  react with small hover changes; may stop mid-drift) · **demo, type, demo** (two fixed-frame demo
  halves of 4 to 5s either side of a full-frame run of type built as word-beats).
The camera approach comes from the material: follow for a task that moves across a surface, a fixed
frame for a crowded dashboard, the single sweep for one document travelling through the product. In
a fixed frame any zoom is the interface scaling itself, never the camera.
**Signature**: every state change follows a cursor action. **Recipes**: pointer-click, pointer-drag,
team-pointers, heavy-press, rig-camera, aim-zoom, same-spot-swap, word-drop, detect-box,
length-timed-cards. **Breaks it**: the interface changing with nobody acting on it; a change of
camera approach partway. Shelf: Interface (ui-agent-steps, ui-drag-drop), Screen demos (screen-zoom-detail).

### ask-and-answer: the question typed, the product replying
Opening, first look, feature in use, the ask. 5.2 to 12s. **A**: the question is the whole shot,
stopping at submit. **B**: the question and the complete answer. **C**: a result that appears at
once.
- Scene 0 (optional, 2s at most): a headline typing, a title card, a logo settling into a corner.
- Scene 1 (about 1 to 3s): the input becomes active: a pill widens, a prompt card springs on, a
  single push-in frames the input box, a ⌘K dialog springs up while the page behind blurs. Before or
  while typing, optionally: a file dragged into a tray under the input, a model chosen from a
  dropdown and ticked.
- Scene 2 (about 2 to 6s): the prompt types quickly after a caret, the box growing downward as lines
  wrap; a typed name can turn into an inline brand pill. A can stop here, mid-word or on submit.
- Scene 3 (about 4 to 7s): submit (the button dips on the press and may change from waveform to
  arrow to stop); what was there disappears; the product shows it is working: phrases fading into
  each other under a shimmer, a spinner, a checklist turning into green ticks, a heading changing
  from "Reading" to "Read". A can stop on this working state.
- Scene 4: B builds a block at a time, each pushing the rest down (paragraphs streaming, rows of
  actions with icons, change cards, chart lines growing from one point) while the view follows the
  latest line; C shows a finished result (suggestion chips popping as the query types, code
  flipping to its preview).
- Scene 5: hold the result; or the click that confirms it; or (opening) a second prompt begins and
  the shot stops mid-action. The install card for an ask: the headline steps back (50%, grey, moved
  up), a $ chip stretches into a wide terminal pill, the command types, tool icons pop on one at a
  time, only the caret moves.
**Signature**: the keyboard acts and the product responds. At most two real camera moves, none
while it is working or streaming: the content scrolls and the camera stays. **Recipes**:
typed-states, colored-caret, length-timed-cards, edge-grow, pop-arrival, color-through-type (the
working shimmer), living-icon, line-draw-on, camera-phases. **Breaks it**: the whole answer in one
go; a cursor clicking from state to state (that is pointer-walkthrough); typing that nothing
responds to (that is keystroke-line). Shelf: Chat and notifications (chat-assistant-reply), Code
(code-copy-line).

### work-underway: the work shown while it happens
Feature in use. 4.2 to 11.6s. A warm flat background, white cards, one accent for work in progress
and one color for finished.
- Scene 1 (0 to about 1.5s), one trigger: a question pill unfolds into 3 to 4 numbered options, a
  cursor hovers and clicks, the menu shrinks away (the only cursor in the shot); or a dialog's button
  is clicked and the dialog fades; or a scan is under way from the start; or a conversation begins
  with the user's message.
- Scene 2 (about 1 to 4s), the work, 1 to 3 signs one after another: a spinner beside a label typing
  quickly ("Draft" to "Drafting outline…") under a shimmer; pairs of status lines changing at an
  even pace; a scan label typing, then moving into the top left; in a conversation, a tool line with
  three pulsing dots that stop on the exact frame the result arrives.
- Scene 3 (about 2 to 4s), the result: a card pops on and unfolds downward; rows follow (rising as
  they fade on), each a badge, a label and a detail line; badges turn over in turn from an outlined
  number to a filled tick with a small scale bounce, the label crossed out and dimmed, unfinished
  rows showing part-drawn outlines. Stops partway down the list. In a conversation, a white card
  opening downward from the tool line with 2 to 4 pale monospace lines, or a dark message card
  growing out of a status row until it fills the frame.
- Scene 4 (the final 1 to 2.5s): hold, or scroll the card quickly; in a conversation, one push-in
  onto the confirming line as a reaction pill springs on.
**Signature**: rows landing as the output, then changing state. The spinners and dots are the
content: driven by the frame, finite, gone on the frame the state resolves. At most one real camera
move. **Recipes**: edge-grow, pop-arrival, pointer-click, living-icon, typed-states,
color-through-type, line-draw-on, Notation.StrikeThrough, count-and-grow. **Breaks it**: a typed
prompt (that is ask-and-answer); rows that arrive and do nothing (that is tile-cascade); a spinner
still turning once its result is in. Shelf: Lists and timelines (list-checklist), Interface.

### tweak-and-watch: adjust here, see it there
Feature in use. 5.3 to 12s. **A**: a control changes the target. **B**: hovering over the target
changes the panel. **C**: the panel's value changes unit.
- Scene 0 (optional, 0 to 2.0s): a dark card with one white line saying what it can do, then a cut.
- Scene 1 (about 1 to 3s): target and panel both in view; selection marks show they are linked (a
  selection box, dashed guides, size chips popping on in turn); the cursor moves to the first
  control.
- Scenes 2 to N (about 2s each): one unbroken gesture and the change it causes as it happens. A:
  drag a value (0° to -10°, 0 to 38px) while the target turns or moves, or type over a value while
  the preview resizes with each key. B: swatches under the cursor update a hex value. C: a unit is
  chosen from a dropdown and the value jumps (0.5rem to 8px to 6.495%).
- Scene N: the final edit holds, or a zoom-out shows the product using the new value; never a
  tooltip with the dropdown not yet opened.
**Signature**: the gesture and its result on screen together, on every beat. Both come from one
source: the displayed value is `Math.round(v)` of the same interpolate that moves the target. The
camera keeps both in shot: fixed, one slow move, or a quick push-in that returns to exactly the same
framing. A curve bent by a handle is `Paths.interpolatePath`; a knob along a path,
`Paths.getPointAtLength`. **Recipes**: live-control, pointer-click, colored-caret, count-and-grow,
typed-states, pop-arrival, aim-zoom. **Breaks it**: control and target timed separately; framing
that cuts off the part that responds. Shelf: Interface (ui-context-menu, ui-checkout,
ui-code-entry).

### held-screen: the surface held, its screens moving
Feature in use, first look. 5 to 11.3s; each screen stays 1.0 to 1.5s.
- Scene 1 (0 to about 1.5s): the surface slides on and settles, or fades up from a title card, with
  an accent shape coming into focus behind it.
- Scene 2 (1.5s to X): used through its own screen: a tap presses a button (95% to 100%), the old
  content is pushed off as the new slides up, a headline beside it changes (the old line rising
  away, the new rising into place).
- Scene 3+: 2 to 4 beats of screens, holding on the final one.
Variants: **plain tour** (no camera move, no cursor) · **window on color** (a title card of about 2s
fading into a window over a bright gradient, a push-in on its sidebar, a zoom back out while the
content scrolls) · **start to finish** (setup, action and success with no cursor: a code's digits
popping in left to right before a green tick, a confirm press with a spinner, a tiny push-in at the
confirm and nowhere else) · **rotating screens** (a widget trying on brand styles about 0.8s each,
then phone screens sliding left about 1s each, each half introduced by a single centred word).
**Signature**: a surface that stays in place and is used through its own screen; screens clip to the
screen's rectangle. **Recipes**: tilted-page (its flat form is a translateY), typed-states,
same-spot-swap, press-back, pop-arrival, camera-phases, aim-zoom, spoken-word-glow. **Breaks it**:
screens shown one after another with no task linking them; a device drifting for no reason. Shelf:
Screen demos (device-phone, device-laptop, device-browser-stage,
device-floating-screen).

### log-to-result: the work as evidence, then the deliverable
Feature in use. 5 to 11.8s. A flat pale background; the surface shares the scene's background.
- Scene 1 (0 to about 40 to 60%): a single opener (a title types out, or shrinks 50% and tucks into
  the top left), then the journey down through the work, by panning the camera or scrolling the
  content: the prompt, tool pills, ticked items, a summary being typed.
- Scene 2 (about 1 to 2s), one interaction: a file chip pops on and a cursor clicks it; a sentence
  gets a sweep of highlight and a small label pill; a collapsed row arrives at the middle and opens.
  A cursor, if any, appears here for the first time.
- Scene 3 (the rest), one reveal: a fast zoom-out that slows shows the panel was a workspace's
  sidebar all along; or a document window grows and a slow push settles on highlighted cells; or a
  panel opens in place and a follow-up question types. The frame then stays still. Optionally one
  click inside the revealed document changes the screen instantly.
**Signature**: the journey through the work is the evidence, and the turning point pays it off. At
most two real camera moves, one before the turning point and one after; pan or scroll, not both;
scrolls in steps share one easing family. **Recipes**: rig-camera, tilted-page (flat), ghost-trail,
word-drop, typed-states, pointer-click, Notation.Highlight, aim-zoom, edge-grow. **Breaks it**: a
second interaction ahead of the reveal; a device frame (that is held-screen).

# Cutting inside a graphic

Several scenes in one graphic play as one unbroken move when every cut keeps the axis, the
direction and the speed, and falls while both sides are still travelling. The same choices apply
inside one `compose_motion` piece, where the handoff is written by hand, and between clips on the
timeline, where `transitionIn` and `transitionOut` offer a fixed set of styles (the mapping is at
the end).

## The continuity rule

1. **Axis.** Horizontal stays horizontal, vertical stays vertical, depth stays depth.
2. **Direction.** Never reversed. In depth, direction is whether scale is rising or falling: rising
   pushes toward the lens, falling pulls back. The usual slip is a shrinking exit followed by an
   entry that scales up from small.
3. **Speed.** The incoming side starts at the speed the outgoing side finished on. Paired eases:
   exit `Easing.in(Easing.poly(5))`, entry `Easing.out(Easing.poly(5))`, equal distance, equal
   duration; together they are one in-and-out ease split at its midpoint.
4. **Phase.** Both sides are moving when the cut lands.

**One main direction** (leftward unless the brief says otherwise) carries every routine cut. The
others are held back and carry a message: **up** is a lift, a conclusion or reveal above what led
to it · **into depth** (push-in) is further into the same idea · **out of depth** (pull-back) is an
arrival, something larger taking the frame · **blow-out** is quitting one world for another, a
surface flying past the lens. Two cuts in a row never go opposite ways; a change of direction takes
a visible reason (a click, a rebound, a hit) or a new chapter.

**The cut plan comes before code**, a line each: the second it falls, the exit direction, the entry
direction, the technique. `2.4s  A→B  x−  x−  cut while moving`; `4.8s  B→C  z−  z−  pull-back
cut`. Exit and entry agree on axis and sign, or the plan changes, not the eases.

## Bridges, causes, pauses

- **Bridges.** The best cut passes a real object over the join at the same place and speed: a
  cursor partway along its path, a panel that shrinks and settles into the next layout, a mark
  travelling into its slot, the words of a word relay. A cursor bridge speeds up in its final 0.3s
  before the cut toward where the next scene will click first, and the next scene picks it up at
  that point and speed. A cursor with nothing to bridge exits by the closest edge on an in-ease over
  0.5 to 0.7s, never a fade in place. With nothing natural to bridge, each scene's main element does
  it, short travel and a fade that finishes first. A crossfade bridges nothing.
- **Cause and effect.** Every move sets off the next (click, squash, release, flight, impact,
  recoil, reveal), the effect beginning on the frame of its cause. Large elements rebound slowly,
  small ones flick back. Only a force justifies a change of direction.
- **A pause before the payoff**: 0.3 to 0.75s of stillness between the big action and what it
  produces.

**Timing at a cut**: exits take 0.2 to 0.4s; entries take at least as long and at most about 127% of
the exit (0.33s out, 0.42s in); equal for an exact speed match. Elements of the same kind share an
ease and a duration. No bounce or elastic at a cut; an overshoot from `Easing.out(Easing.back(1.4))`
to `back(1.7)` belongs only on an entry meant to be playful.

## In this runtime

- **The background is painted** on the outer `AbsoluteFill`. A cut made while moving has frames
  where the two sides' opacities add up to less than 1, and anything beneath the graphic flashes
  through. A graphic with cuts inside it is full frame with its own background, and the compile
  note about painting the frame is expected.
- **One scene per frame.** Every scene is gated on the cut frame (`frame < CUT * fps` for the
  outgoing, everything after for the incoming), so no frame draws both. Two opacities overlapping
  make a dissolve, not a cut.
- **A scene arrives complete.** Its layout is built on its first frame and the cut brings it in
  whole. Reveals inside it begin no sooner than 0.4s after the cut and end no later than 0.45s
  before the next.
- **Nothing is visible before it starts.** A word timed to appear later holds opacity 0 until its
  start frame (`frame >= start * fps ? … : 0`).
- **Blur the container, not what is inside it.** One blur on the scene or the word group; a
  `Video` blurred through a wrapper.
- **Pixel values here assume 1920 wide**, scaled by `width / 1920`. **Cut times are seconds**,
  `CUT = 2.4` used as `CUT * fps`.

## Checking a cut

`inspect_timeline` at the clip's `from` plus the cut frame, minus 2, at 0 and plus 2. The numbers
in the code first, then: exit and entry agree on axis and sign · the exit's ramp finishes on the cut
frame, not earlier, and the entry's begins on the cut with an offset and an out-ease · end minus
start has the same sign on both sides · speed at the cut is k × distance ÷ duration, where k is 5
for `poly(5)`, 4 for `poly(4)`, 3 for `cubic`, 2 for `quad` and about 6.9 for `exp`, and the two
sides stay within a factor of 3 · no frame shows both sides above about 0.04 opacity · scale moves
the same way on both sides, counting the incoming scene's own entrances in the 0.5s after the cut ·
an object passed across a match cut stays within 12px of its centre (at 1920 wide) and within 5% of
its size from the frame before to the frame after · a change to a scene's first or last second,
including a re-time to new voiceover, reopens its cut · cuts sit on the real word timings; a line
is never sped up to fit a slot.

## The techniques

Five cuts and two moves within a scene, all on one idea: cut when the motion is fastest, and keep
direction and speed equal either side. Depth cuts size their blur to the subject: type peaks at
10px (at 20px the letters smear); a surface filling the frame peaks at 18 to 20px (less on
something that large reads as a render glitch); on the cut frame both sides carry identical blur.

| Technique | Where | Axis | For |
|---|---|---|---|
| Cut while moving | scene to scene | x / y | the everyday cut, in the main direction |
| Push-in cut | swapping text in place | depth, scaling up | further into the same idea |
| Pull-back cut | arrival, payoff | depth, scaling down | something larger takes the frame |
| Word relay | line to line | x, word by word | handing over between two large lines |
| Defocus cut | new state, same surface | x / y / depth | the one cut meant to be noticed |
| Stepped arrival | an entrance, no cut | y, rising | title cards, openers, list intros |
| Three-phase slide | a group moves, no cut | x / y | shifting a finished group to make space |

A sentence still under way (one idea over scenes that look different, a series of cards) takes cut
while moving or a word relay; a shift to a new state or section takes a push-in cut; an arrival or
a payoff takes the pull-back.

**Cut while moving, the default.** The outgoing main element speeds up in one direction, the cut
falls mid-move, the incoming carries on the same way and slows to a stop. About 0.6s end to end.
Short travel, 12% of the width (150 to 300px at 1920), never fully out of frame: exit
`0 → −width*0.12`, entry `+width*0.12 → 0`. Paired eases, exit `Easing.in(Easing.poly(5))` and
entry `Easing.out(Easing.poly(5))`, equal distance, equal duration, 0.33s by default (0.2 to 0.4;
the entry may run to 0.42). The fade finishes first: the exit's opacity drops on
`Easing.in(Easing.cubic)` and reaches zero about 0.02s ahead of the cut, while the element is
plainly still speeding up; with several elements the last reaches zero exactly on the cut. The
entry begins at 0.35 opacity, already partway along, and reaches 1 on the same out-ease; from 0 it
pops. Blur optional, 8px (6 to 10) on both sides at the cut. Combined with depth, both sides scale
down: exit `1 → 0.92`, entry `1.08 → 1` over 0.42s. A full-frame push slide gives up both the
short travel and the mid-move cut.

**Push-in cut, further into the same idea.** Everything scales up: the outgoing text rushes toward
the lens, the swap happens in one frame while blur is at its height, and the incoming is still
scaling up as it settles. The two texts never share the screen. Headlines and short phrases only;
body copy at 0.75 scale cannot be read. About 0.7s.

| Phase | Scale | Blur | Opacity | Ease | Time |
|---|---|---|---|---|---|
| Exit | 1 → 1.2 | 0 → 10px | 1 → 0.15, linear, its own ramp | `Easing.in(Easing.poly(4))` | 0.2s (0.15 to 0.3) |
| Cut | in at 0.75 | 10px | out 0, in 0.15 | | one frame |
| Entry | 0.75 → 1 | 10 → 0px | 0.15 → 1 | `Easing.out(Easing.exp)` on all | 0.5s (0.4 to 0.6) |

The exit's opacity takes a linear ramp of its own: on the in-ease it would sit near 1 for most of
the move. Adjustable: exit scale ±0.1, entry scale ±0.1, opacity at the cut 0.1 to 0.2.

**Pull-back cut, an arrival.** The push-in run backwards, everything scaling down: the outgoing
falls away from the lens and the incoming comes in too large, as if from just behind the camera,
then shrinks into position. For a payoff line, a big answer or a final held state, never a routine
cut. About 0.7s, split 30% exit and 70% entry.

| Phase | Scale | Blur | Opacity | Ease | Time |
|---|---|---|---|---|---|
| Exit | 1 → 0.8 | 0 → 10px | 1 → 0.15, linear, its own ramp | `Easing.in(Easing.poly(4))` | about 0.2s |
| Cut | in at 1.25 | 10px | out 0, in 0.15 | | one frame |
| Entry | 1.25 → 1 | 10 → 0px | 0.15 → 1 | `Easing.out(Easing.exp)` | about 0.5s |

The incoming scene is already laid out inside the shrinking wrapper; none of its elements scale up
from small in the first 0.5s, and its own staged entrances wait until the shrink has settled.

**Word relay, one word at a time.** Cut while moving applied word by word between two large lines:
each outgoing word tears away on its own curve, each incoming word arrives already in flight, and
the eye follows the wave through the cut. Inside a single graphic, outside the transition budget.

| Parameter | Value |
|---|---|
| Travel | ±12% of the width |
| Exit | 0.34s, `Easing.in(Easing.poly(5))` |
| Exit fade | 0.18s, `Easing.in(Easing.quad)`, beginning with the move |
| Exit stagger | +0.022s per word in reading order; the last word's fade reaches zero on the cut |
| Entry | 0.3s, `Easing.out(Easing.poly(5))` |
| Entry opacity | starts at 0.35 |
| Entry gaps | 0.05s, each 0.84 times the one before, so the words come faster and finish together |

A run of relays keeps one direction; a pull-back cut can only be where the run ends. A short first
line can leave as one piece: its fade about 0.3s, reaching zero 0.02s ahead of the cut. Shelf:
Transitions (transition-word-carry).

**Defocus cut, a cut meant to be noticed.** A brief surge of blur covers a one-frame swap, the way a
handheld camera racks focus, when the same surface changes state in the same visual world: on a
story beat, no more than about once every 8 seconds, never during a caption or a hold. The outgoing
keeps full opacity right up to the cut; blur covers the swap, not a fade. Lens softness rather than
momentum: `Easing.in(Easing.cubic)` out over 0.3s, `Easing.out(Easing.cubic)` in over 0.35s. The
swap happens once blur has peaked: 6px minimum, usually 8 to 12, 16 to 18 at most. A small scale to
1.06 and back reads as a lens; the continuity rule still applies, so the pan runs the same way either
side (x `0 → −80px`, then `+80px → 0`), or is dropped for a depth version.

**Stepped arrival, an entrance, not a cut.** Words or elements snap up from below, each setting off
before the one ahead has landed: a wave that gathers speed and settles into a finished layout. Its
rules differ from the relay's: opacity switches on, straight from 0 to 1 on each word's start frame
(cuts fade, entrances do not) · each piece begins within 2 frames of the one before landing, the gaps
tighten along the wave, the final piece snaps home · `Easing.out(Easing.poly(5))`
(`Easing.out(Easing.exp)` for a sharper snap), never an in-out on an entrance, one direction per
wave.

| Piece | Rise | Duration | Next piece starts |
|---|---|---|---|
| Key word, heavy | 60 to 80px | 0.16 to 0.2s | 0 to 2 frames after this one lands |
| Ordinary word | 40 to 50px | 0.13 to 0.16s | 1 frame before this one lands |
| Short word, punctuation | 30 to 48px | 0.1 to 0.13s | 1 to 2 frames before this one lands |

A frame here is 1/60 of a second. Breaking the last word into pieces draws out the climax, and those
pieces travel further. The line is fitted with `Layout.fitText` over the whole sentence, capped, so
the wave never runs off the frame. Shelf: Text effects (text-rush-in, text-letter-settle).

**Three-phase slide, moving a group without a jolt.** Slow, then fast, then slow, for shifting a
finished group (a row of words, a stack of cards) to reveal something or free space. No single ease
manages it (an in-out slams into its end); one interpolate with three segments does.

| Phase | Ease | Distance | Time |
|---|---|---|---|
| Ramp in | `Easing.in(Easing.poly(4))` | about 10% | about 20% |
| Burst | `Easing.linear` | about 65% | about 18% |
| Tail | `Easing.out(Easing.poly(5))` | about 25% | about 62% |

A worked case: 270px in 0.57s (`−30, −210, −270` at 0.12s, 0.22s, 0.57s). The tail lasts at least
three times the ramp in; a stop that still jolts gets more time on the tail, not more distance. New
content comes in during the linear burst, where the speed hides its arrival.

## The budget

One main cut (cut while moving, in the main direction) handles most scene changes, with one or two
accents beside it; two or three kinds of cut cover a whole piece. A hand-built morph and word relays
within a block of lines sit outside the count. Nothing animates out ahead of a scene change: the
exit half of the cut is the handoff, and only the final scene leaves on its own.

What goes wrong, and the fix: each scene's entrance designed separately (plan the cuts first) · a
crossfade between scenes (cut while moving) · the exit finishing before the scene changes (both
sides travelling at the cut) · an entry setting off from rest (start partway along on an out-ease) ·
a shrinking exit into an entry that scales up (scale changing the same way) · the incoming scene
popping its own elements in during a depth cut (arrive laid out, stage entrances after it settles) ·
both texts on screen in a push-in (a one-frame swap once blur peaks) · 20px blur on type (10 on type,
18 to 20 on surfaces) · a soft entry ease after a sharp exit (paired) · an in-out ease on either
side of a cut (an in half and an out half) · a fade on a stepped arrival (opacity switches on) ·
every word rising the same distance at the same speed (key words further, punctuation snaps) · a
reaction trailing its cause (start it on the cause's frame) · a transparent background behind a
mid-move cut (paint it).

# Choosing a transition

A transition says what the next scene is to the last: a dissolve keeps a thought going, a push
moves to the next point, a slow blur lets the viewer settle. It is picked for what the content is
doing, not for variety, inside a graphic and between clips alike.

- **The budget**: one main transition on 60 to 70% of scene changes, one or two accents.
- **No exit before a transition**: the outgoing scene is fully present as it begins. A scene fading
  away before the next one's entrance is a hard cut with an empty gap. The final scene alone
  leaves on its own, with a fade to black.
- **A plain cut works when something carries across it**: a shape matched either side, a beat, an
  element that stays put, a line of voice running through.

| Energy | Main | Accent | Length | Ease |
|---|---|---|---|---|
| Calm: slow brand films, hospitality, care | blur dissolve, focus pull | light leak, iris | 0.5 to 0.8s | `Easing.inOut(Easing.sin)`, `quad` |
| Medium: product tours, business, how-tos | push, panel sweep | squash wipe, upward push | 0.3 to 0.5s | `cubic`, `poly(4)` |
| High: trailers, sport, music, reveals | push-in, overbright flash | panel sweep, fall-away | 0.15 to 0.3s | `poly(5)`, `exp` |

| Mood | Transitions |
|---|---|
| Warm, welcoming | light leak, blur dissolve, focus pull, film burn |
| Cool, precise | squash wipe, zoom out, blinds, shutter, tile dissolve |
| Editorial | push, upward push, angled wipe, shutter |
| Technical | tile dissolve, panel sweep, blinds, color fringe |
| Tense | glitch, color fringe, ripple |
| Playful | iris, clock wipe, growing disc |
| Dramatic | push-in, zoom out, fall-away, overbright flash, dip to black |
| Premium | focus pull, blur dissolve, dip to black, held back |
| Nostalgic | film burn, light leak, clock wipe |

| Where it falls | What to use |
|---|---|
| Opening | the most characterful one that suits the mood, 0.4 to 0.6s; it teaches the piece its style |
| Between connected points | the main transition, held steady, 0.3s |
| New topic | anything but the main one: panel sweep, shutter, squash wipe |
| Climax or big reveal | the strongest accent, the quickest or the most dramatic |
| Winding down | gentle again, a blur dissolve or a dissolve, 0.5 to 0.7s |
| Ending | the slowest and plainest, a dissolve or dip to black, 0.6 to 1.0s |

A blur dissolve sets its blur by energy: calm 20 to 30px across 0.8 to 1.2s holding at the peak 0.3
to 0.5s; medium 8 to 15px across 0.4 to 0.6s holding 0.1 to 0.2s; high 3 to 6px across 0.2 to 0.3s,
no hold. Dissolves built in code, fastest first: 0.15s `inOut(exp)` a flicker of a change, 0.2s
`inOut(poly(5))` crisp, 0.4s `inOut(cubic)` the even default, 0.5s `in(poly(4))` into
`out(poly(4))` for weight, 0.6s `inOut(sin)` soft, 0.7s `inOut(quad)` slow and rich.

**The code-built families**, in short (inside a graphic a push is cut while moving, and every other
family draws one scene per frame unless drawing both is the point): **dissolve** crosses two
opacities over 0.5s · **dip to black** fades out 0.2s, the next fades in 0.2s beginning 0.05s after
the first has gone · **iris** animates `clipPath: circle(0% → 75%)` on the incoming scene over
0.5s · **clock wipe** is a nine-point `polygon` sweeping the four quadrants at 0.1s apiece ·
**panel sweep** slides panels the size of the frame, in the piece's own colors, across it 0.06s
apart and swaps the scenes while they are hidden (whole-frame panels, never narrow bars) ·
**overbright flash** takes `filter: brightness()` up to 3 while a white layer peaks on the swap,
climbing over 0.035 to 0.055s and recovering over 0.24 to 0.38s, no more than once a second, white
or warm and never saturated red · **light leak** lays `LightLeak` over the cut · **glitch** is
tinted copies at 35% opacity jumping ±30 to 60px for 0.2s · **fall-away** drops the outgoing scene
about 1.1 frame heights while turning it 4° over 0.5s on `Easing.in(Easing.poly(4))`, drawn on top
of the incoming · **blinds** use 4 horizontal or 6 vertical strips when calm, 6 to 8 or 8 at medium
energy, 12 to 16 or 16 at high. Repeating geometry (a grid of tiles, even rows of dots) reads cheap
however carefully built. Shelf: Transitions (`get_templates` {shelf: "Transitions"}), each a short full-frame graphic
spanning a cut.

## Between clips: transitionIn and transitionOut

A clip transition is drawn at the cut with the neighbouring clip on the same lane, its window centred
on the cut: `{ style, durationInFrames, direction? }`, frames at the timeline's rate (seconds × the
fps `get_timeline` reports). The arriving clip's `transitionIn` wins over the leaving clip's
`transitionOut`. Every style eases in and out on a fixed curve, so the choice is the style and the
length, never the motion; a timeline transition cannot cut mid-move. `direction` (wipe, push,
slide) is the way the picture travels; `left`, the default, is the main direction.

| Family | Style | Note |
|---|---|---|
| Crossfade, dissolve | `dissolve` | the arriving clip's opacity |
| Blur dissolve, focus pull | `blur` | softness peaks mid-window at a fixed amount; only the length is chosen |
| Dip to black | `fade` | the outro and the deliberate full stop |
| Overbright flash, dip to white | `dipwhite` | an impact beat; it rises and recovers over equal halves, so a flash that rises fast is built in code |
| Push, upward push | `push` | both clips move a full frame; `direction` set to the main direction |
| Cover | `slide` | the arriving clip slides over the leaving one |
| Edge wipe, angled wipe | `wipe` | a straight edge only |
| Iris | `iris` | opens from the centre |
| Clock wipe | `clock` | sweeps from twelve |
| Push-in | `zoom` | grows in from 0.85: into depth only |
| Whip pan | `whip` | a push that smears; `direction` set |
| Spin | `spin` | both clips turn, blurred at the peak |

What the timeline cannot draw: a pull-back cut, a speed-matched cut with short travel, a word relay,
a defocus cut (those scenes are one graphic instead); panel sweeps, blinds and
shutter (a short full-frame `compose_motion` on a track above, spanning the cut, covering and
uncovering while the clips cut underneath, which is what the Transitions shelf holds). Light leak and
glitch on a cut are an adjustment clip with `lightleak`, or `chromatic` and `shake`
(`skill-video-effects`). Energy sets
the length, one main transition sits on most cuts, and the hard cut is a transition too: footage cut
on matching action, a beat or a sentence running through needs nothing drawn at the cut, and a
dissolve on every clip is the timeline's own slideshow.

## Not in this runtime

Inside a graphic: per-pixel shader transitions (warped or noise-driven dissolves, heat haze, vortices, lens bends,
turbulent burns, radial color splits) blend two scenes pixel by pixel, which the runtime does not
do. Nearest stand-ins: a whip pan is cut while moving with 8 to 10px blur and up to 8° of `skewX`
leaning into the travel; a zoom blur is a push-in cut at the full-frame 18 to 20px blur; a warped
dissolve is a calm blur dissolve; a shaped iris is a `clipPath` circle; a digital glitch or color
split is tinted offset copies; a flash through white is `dipwhite` or an overbright flash; a light
leak is `LightLeak`. Heat haze and turbulent burns have no stand-in. A burning page edge needs a
canvas for the charred rim and an inverted SVG clip. In depth, a card flip or a hinged door is built
on the stage in `skill-motion-recipes`; between two clips the timeline has no depth transition. A
star-shaped iris, tilt-shift and lens flare cannot be drawn in CSS.
