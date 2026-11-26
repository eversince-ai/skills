# Motion graphics: overlays

A short graphic is one idea, and the move is the idea: a name arrives, a number lands, a line finds
the thing that matters in the shot. Everything here serves that move being legible, on time and in
the right place, over footage that has to survive it.

## The kinds

What each needs before it is built:

- **Kinetic type**: the exact copy, broken by breath, 3 to 7 words a scene in English (4 to 12
  characters in Chinese), each scene one job (hook, build, punch, resolve), one or two emphasis words
  that take the accent or the beat.
- **Single stat**: the value, any prefix or suffix, what it means, whether it is a share of a whole
  (which earns a ring). 4 to 6 seconds.
- **Chart**: the data, the labels, the one sentence the chart proves. Axes hidden unless magnitude is
  the point, then muted. Bars grow in a stagger, lines draw on, a pie sweeps, a ring fills.
- **Logo reveal**: the logo from the library, a tagline, a url. 3 to 5 seconds. A vector logo draws
  on with Paths; a raster one reveals by mask or scale with a glow behind it; the palette is sampled
  from the logo.
- **Lower third**: the name and role exactly as they should read, the brand color, a frame of the
  shot it sits on.
- **Callout**: the thing in the footage, where it is at that moment, the words that name it.
- **Social overlay**: display name, handle, count, the action (follow, subscribe). A generic card in
  the user's colors; a real app's logo or interface is not drawn.
- **Quote card**: the quote word for word, who said it, the phrase that earns the highlight.
- **Flat map**: the places and what connects them. No geographic data in the runtime: shapes drawn in
  SVG, or a map picture from the library with marks measured onto it; a country's outline is never
  guessed. Subjects in one or two accents over neutral everything else; labels clear of every border
  and each other; no ambient glows or particles; marks animate only while the map holds still; real
  map imagery carries a small credit in a low corner. Shelf: Maps.
- **Post, news or headline card**: the text itself, from the user or from research. A post for a
  real account or a headline for a real outlet is never invented.

Every name and title goes on screen exactly as given.

**Overlays** (lower third, callout, social card, notification, logo bug) leave the frame alone.
**Cards** (quote, stat, chart, map, headline, kinetic title) own their ground.

**Dressing a talking head** is planned from the words: `get_transcript` says what the speech claims.
A person or company named takes a lower third where first said; a number said takes a stat card on
that line; the strongest line a quote card; a thing shown a callout; each timed to the frame of its
word, spread across the clip. Captions go on first and every
card stays clear of their band. Where a card needs the room, the speaker shares the frame with it
for that stretch: the speaker's clip split at its edges (`edit_timeline` splitClip) and that part
laid out (`apply_layout`), full frame again once it clears.

## Short-form rules

- The hook lands in the first 0.5s.
- One dominant motif: one way things move, one accent, one signature move.
- Past 2.5s, a pattern interrupt that changes exactly one thing (an element, a color, a direction,
  a scale). Several at once read as a new graphic.
- A key element stays readable for at least 0.3s: still, whole, at full opacity.
- Beats land 0.1s early: a move timed onto a sound reads late, so it starts three frames ahead at
  30fps (`beatFrame - 0.1 * fps`).
- Intensity matches the energy: a calm interview takes a clean wipe, a hype cut a slam.
- Every property tells the same story: rising and fading says arriving; rising, rotating, scaling
  and unblurring at once makes four claims. Entrances ease out, exits ease in, moves across the
  screen ease in-out, impacts (a stamp, a slam) ease in.
- Supporting parts are delayed, never the focal one: the name first, the role after. Parts that
  arrive one after another stagger; parts already there show up at once.
- A hold is still. The only life in a hold is the subject doing something: a count ticking, a
  button pressed.
- A number never overshoots: a counter that runs up to 47 before dropping to 42 has put a false
  figure on screen.

## Lower thirds

**Size.** A name is read in under a second from across a room: 5 to 6% of the frame's height
(about 56 to 64px at 1080 lines), the role at about half that (2.4 to 2.9%), never under 2.4%. It
sits on the 7% side margin about a tenth up; on 9:16 it rises above the platform's buttons (the
bottom 24% stays clear) at about the same size in pixels.

**Card or cardless.** Cardless type with a light lift (a 1 to 3px shadow and a soft 14px one, black
at 16 to 22%) is the default over live footage; a heavier shade behind reads as a filter over the
shot. A plate is for a shot too bright or busy for bare type, or for a broadcast or sport look.

**Tones** (the Lower thirds shelf has one of each: `get_templates` {shelf: "Lower thirds"}):

- **Plain**: name 600 over role 500 at 75% ink, no box (name-clean, name-two-up for a two-shot).
- **Broadcast**: a heavy name on a solid square plate, the role on a shorter strip in the accent
  directly under it, both opening from the left (name-solid-plate, news-headline-roll).
- **Soft and modern**: a frosted plate (the shot blurred about 26px, a 38% tint, a 2px rim at 16%
  white), or a white pill around a round face mark (name-glass-plate,
  name-portrait-tag).
- **Documentary**: a tracked capital label above a serif name, no box (name-serif-credit).
- **Business**: a logo tile beside the name (name-company-credit).
- **Energetic**: a block of the accent sweeping across each line and leaving the words behind
  (name-wipe-credit); condensed capitals with a number tab for sport (name-match-credit).
- **Tech**: the role typed in a mono on a dark chip after a prompt mark (name-terminal-credit).
- **Place**: a pin and the place typed in capitals, the date beneath in a mono (place-dateline).

Thin accent rules, upright bars beside a name and underlines read as generated; a plate edge or a
change of weight does the same work.

**Placement.** The lower band inside the safe area: at 16:9 about 6.5% of the width in and 10 to 11%
of the height up; at 9:16 above the bottom 24%, where the platform's interface sits. Below the chin
line on the open side of the frame, read off an `inspect_timeline` frame; a speaker framed left
puts it right and everything mirrors (right edge, wipe from the right, rules growing from the right).
Never across the face; above the captions when they are on.

**Timing.** First move at 0.1s, every part landed by about 0.95s, then still. Out in 0.3 to 0.4s
starting half a second before the clip ends, eased in, the parts that came in last leaving first,
timed against `durationInFrames` so the exit lands wherever the clip is trimmed. Placed for at least
4 seconds, about 5 as normal; a long name or title gets longer (read it aloud, then add a beat). Check it at the start and the end of its hold: text readable over one frame can vanish when the shot behind it brightens or gets busy, so give it a backdrop or a shadow when the footage changes under it.

**Mechanics.**
- A bar carrying text wipes with `clipPath: 'inset(0 X% 0 0)'`, X 100 to 0: the card uncovers from
  its left edge and the words stay still. `scaleX` from the left is for a bar carrying nothing (a
  rule, a tab, an underline); scaling a box with words squashes them.
- Text rises behind a mask: a wrapper with `overflow: 'hidden'`, the line inside from
  `translateY(110%)` to 0 (the broadcast look); a fade-up is the softer one.
- A clipped card's shadow is `filter: drop-shadow(…)` on a wrapper: `clipPath` cuts a `boxShadow`
  off, and a drop-shadow on the parent follows the wiped shape.

## Callouts

A callout works only if it is on the thing.

- **The target is located by picking grid cells, never by estimating coordinates.** By eye a mark
  misses by about 6.5% of the frame on average, 16 to 24% on weaker vision; picked off a grid in two
  passes, by 2 to 4%. Pass one: `inspect_timeline` at that moment, ruled every 0.1 of the frame;
  every cell the target touches is named. Pass two: a throwaway `compose_motion` on that moment for
  a few frames drawing only a fine grid over that region, a line every 0.02 of the frame, each
  labeled; the moment read again and the cells picked. Center and size come from those cells; the
  ring is drawn and read on a frame and corrected by the direction it is off; then the throwaway is
  removed (`edit_timeline` removeClip). A target that could be several things is made unique first.
- **A line has a start and an end anchor**: start on the target's ring, end where the label sits,
  the label's edge touching the line's end. Ring, line and label come from the same numbers in the
  frame's own pixels: one SVG the size of the frame, the label placed from the line's end point.
- **Ring, then line, then label.** The ring draws (0.6s, in-out); a looser second ring over-strokes
  it (0.5s, starting about 0.4s later); the line draws out toward the words (0.35s); the label
  arrives at its end (0.45s).
- **The label goes outward**, toward the nearer edge and off-center, measured with
  `Layout.measureText` and flipped only when it would cross the safe edge; checked against faces
  on a frame.
- **Nothing is pre-drawn**: a path stays hidden until its progress leaves 0 (a round-capped dash at
  full hide still leaves a dot).
- **No push under a fixed mark**: a shot that zooms or pans while the callout holds slides the target
  out of the ring; the shot holds, or the marks move with it.
- A darkening wash (a radial gradient lit at the target, falling to about 66% black at the edges)
  is for one moment, since it covers the footage.

Shelf: Callouts (callout-drawn-ring, callout-arrow-note, callout-pinned-label and callout-tick draw
their strokes on; callout-focus-box frames a region; callout-spotlight and callout-blur-box for the
wash and the frosted box; callout-click-marker, callout-step-marker and callout-tooltip for
interfaces).

## Social overlays

Generic, brandable versions of the cards people know: the shape is recognizable (a dark pill with an
avatar and a button, a rounded toast from the top-right), never a real platform's logo, icon or
interface. Name, handle, count and color are knobs. In a vertical frame everything is about 25%
larger.

- **Follow or subscribe card**: a dark pill (#1c1c1f), centered low; a 96px avatar inside a 3px gray
  (#3a3a3e) ring; name white 34px bold; handle and count 24px mid gray (#a4a4aa); a round 220×68
  button in the accent. Rises 300px in 0.5s and holds; at 1.0s the button is pressed (0.92 scale over
  0.15s) and released with a small spring, the one overshoot, because a button has one; the accent
  fades to dark gray (#343438) over 0.12s while the label changes to its done state over 0.08s. Out in
  0.25s, 0.7s before the end. Shelf: Social (social-follow-prompt, social-subscribe-press).
- **Notification toast**: top-right inside the safe area, scaled about 2.2× from a desktop
  notification's real size: 820px wide at 1080 lines, radius 32, a 76px icon, app name and time 24px
  at 50% and 35% white, title 34px 600, body 28px at 62% white clamped to two lines. In from the
  right edge over 0.5s at 0.2s, out the same way in 0.3s, 0.8s before the end; a subject framed right
  puts it top-left. Frosted glass is a backdrop blur (`skill-motion-recipes`, glass) and the toast
  then fades itself rather than the clip; a 94% dark fill with a 1px inner light edge is the cheaper
  stand-in. Shelf: Chat and notifications (notify-banner, notify-stack, notify-email, notify-payment).
- **Post card** (a post, a comment, a now-playing card): rises in, text reveals line by line, counts
  tick up and land on their value, one press (a like, a play) is the signature beat at about 1.5s.
  Shelf: Social (social-text-post, social-post-frame, social-viewer-comment),
  Chat and notifications (chat-text-exchange, chat-pop-in).
- **News card**: text at a readable size, never zoomed to fill the frame. Rises and fades in (0.8s
  on `Easing.out(Easing.poly(4))`, from 52px below); once settled, a marker grows left to right
  behind the key word (0.5 to 1.5s by length): `backgroundImage` a flat `linear-gradient` of the
  marker color, `backgroundSize` 0% to 100% wide by 64% tall, `backgroundPosition: '0 72%'`,
  `backgroundRepeat: 'no-repeat'`, `WebkitBoxDecorationBreak: 'clone'` so a wrapped phrase
  highlights as one. Vertical: an outlet kicker at the top, one sentence in a bold (700) serif at 70
  to 76px on 1.4 line spacing, about 900px across. Wide: logo top left (about 64px), date under it
  (about 30px, muted), headline in a serif at 96px 700 across about 1080px, line height 1.16, a 38px
  gray standfirst, the outlet bottom left (about 50px), the person as a cutout (`remove_background`, credits,
  offered; otherwise their photo inside a shape, never a bare rectangle) rising from the bottom right. Order: logo 0.15s, date 0.4, person 0.45 (for
  0.85s), headline 0.7, standfirst 1.5, outlet 1.9, highlight 2.2 for 0.8s; about 6.5s in all.
  Shelf: News and live (news-story-banner, news-full-story, news-explainer-opener).

## Cards

- **Quote card**: lines rise behind a mask one after another (0.12s apart, 0.7s each), the
  attribution follows, the highlight grows across the phrase last, after everything has settled. Type
  sized with `Layout.fitTextOnNLines`; the phrase found in the quote by its words, so both survive
  a rewrite. Shelf: Quotes and reviews (quote-pull, quote-editorial, quote-press, quote-split,
  review-card, review-portrait).
- **Stat card**: the count runs 1.2 to 1.6s on `Easing.out(Easing.cubic)`, then holds (a harder
  ease such as `poly(4)` lands the figure a third early and freezes the last digits). The ring fills
  on the same ramp, only when the number is a share of a whole. The label arrives after the number
  lands. Tabular figures; the number sized to its final value so it never grows while counting. Past
  2.5s one new thing arrives: the context. Shelf: Stats and progress (stat-big-count,
  stat-ring-percent, stat-card, stat-then-now, stat-row), Charts (chart-share-bar).

## Building it

- One palette at the top: every color in a few named constants or controls.
- A centered element moves inside its centering: `translate(-50%, -50%)` stays in the same transform
  string as the move, or the move replaces the centering and it jumps.
- One repair pass on what the frames show (text off the frame or colliding, an empty frame, wrong
  content, a move nobody could read), fixed in place once and read again; the clip's length is never
  changed to hide a defect; frames that show the composition wrong as a whole mean a redesign.
- Three moments prove it, drawn with `inspect_timeline`: the opening state (the first frame where
  it reads, 0.3 to 0.5s in, since frame 0 is empty by design), the signature move at its peak (the
  wipe mid-way, the click, the line reaching the label), the final hold (the last frame before the
  exit).

**Depth and glass.** A card tilting as it lands, flipping to show its back, frosted cards, a logo
extruded into depth, screens on a tilted plane: the stage and numbers are in
`skill-motion-recipes`. An overlay that frosts what is under it carries its own fade rather than
the clip's, or the glass shows the footage sharp while the clip fades.
