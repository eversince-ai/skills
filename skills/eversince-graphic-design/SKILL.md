---
name: eversince-graphic-design
description: "A post, poster or cover in editable layers, and the type, color and composition for anything seen: a still, a slide, a frame."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-graphic-design"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A frame gets about two seconds of attention, a still in a feed less, shown at a size nobody picked,
through compression that eats fine detail. So set type larger and heavier than on a web page, make
one element win at a squint, use color strong enough to survive the codec, and decide the look
before writing the first style. This holds for a frame of video and a still on a canvas alike; a rule
for video only says so. `skill-design-looks` has ten named looks to anchor a brief to. A brief, a reference or a brand the user gives outranks every default here.

## The plan: a post, a poster or a cover

1. Settle what it has to say: one message a stranger gets in two seconds, where it runs, and its size (`get_canvas` with `destinations` lists the named sizes)
2. A template that fits: `get_templates` {kind: "canvas", query}, then `manage_canvases` {create: {from_canvas: its id, name}} and replace its words and pictures; otherwise `manage_canvases` {create: {destination, name}} or {width, height}
3. The look: the user's own fonts and colors (what they name, or their brand kit through `manage_brand_kit`), or a palette and pairing below, or a look from `skill-design-looks`
4. `edit_canvas` {canvas_id, operations: [setCanvas {patch: {background}}; addLayer image (the user's photo by ref, a stock picture through `search_stock` and `import_media`, or a generated one in credits); addLayer shape, icon or chart; addLayer text for the headline and the rest]}: every part its own layer, so each stays editable (every field, with its unit, is in `skill-canvas-operations`)
5. `inspect_canvas` {canvas_id, scale: the size it is seen at}: the check below
6. `publish_preview` {canvas_id}: the link to hand over; `render_canvas` {canvas_id, format} for a file

A thumbnail is `skill-making-thumbnails`, a carousel or a deck `skill-making-carousels`, an ad with copy `skill-image-ads`.

## The brand is fixed, the application is not

A brand kit's or a brief's values are exact: the hex codes, the families, how the weights relate,
every don't. What changes for the screen is how you apply them: type size, spacing, how strongly
decoration shows, how heavy a border is. A web card's 1px edge and 3px shadow disappear on video. A
brand built on a light ground stays light.

With no brand, fix three colors before drawing anything: a ground that holds across every section, a
foreground, and one accent. The accent is the only hue that carries the work; no element gets a
color of its own. The subject decides light or dark: cooking, health and children's content read
best on a light ground; software, film and money on a dark one. Assign the roles from a row below
(first the ground, last the accent, the three between a second surface, a muted tone and the text,
picked by contrast against the ground), or use a single hue at three lightnesses with every gray
leaned toward it:

- Office paper (explainers, how-tos, onboarding): #FAFAF6 #D6D3CB #4A4843 #1F1E1C #2F6FE4
- Clay (documentary, founder stories, customer cases): #F1E8DA #D9B48F #A65F3E #5B6B4E #2A2622
- Moss (outdoor, sustainability, food): #F4F1E6 #C9D2B4 #6E8B5A #34462F #D08C45
- Blush (beauty, fashion, interiors): #FBF1EE #F2C6C0 #D98E8A #7FA8A1 #3A2E33
- Flare (launches, announcements, social cutdowns): #FFF4E0 #F4552E #FFC53D #1FB8A6 #1A1333
- Arcade (games, music, nightlife): #120A24 #3B1C6E #FF3D7F #FFE14D #45D49A
- Midnight (software, finance, premium products): #0A1422 #16233A #2C4A6B #8FA7C2 #E7ECF2
- Garnet (events, awards, hospitality): #2A0E17 #7A1730 #C8A15A #F5EBDD #1E3346
- Graphite (typographic work, dramatic reveals): #1C1B1A #545250 #A3A09B #F2F0EB #C24B3A

## Type

The font catalog is one list for the canvas, captions and motion graphics (`get_templates` kind
font lists every family with its weights and scripts). A family or weight it does not hold is refused
with the ones available, so choose from the catalog, not from memory.

**Faces that read as nobody chose**, because every model picks them first: the sans Poppins, Outfit,
Sora, Nunito, Lato, Open Sans, Roboto and Inter; the serifs EB Garamond, Prata, Bodoni Moda,
Cormorant Garamond and Playfair Display; Syne, the surest sign of a default; and six defaults in
waiting, Space Grotesk, DM Serif Display, Archivo Black, Fraunces, Instrument Serif and Bricolage
Grotesque. Any of them fits when the content asks for it and you can name the reason. A face used in
the last work for the same user counts as a default too.

**More of the catalog, by voice.** Serif: Crimson Pro, Gloock, Libre Baskerville, Newsreader,
Spectral, Young Serif. Sans: Albert Sans, Archivo, Figtree, Geist, Hanken Grotesk, IBM Plex Sans,
Instrument Sans, Inter Tight, Onest, Red Hat Display, Schibsted Grotesk, Work Sans. Condensed and
heavy display: Anton, Antonio, Barlow Condensed, Bebas Neue, Big Shoulders, Dela Gothic One, Fjalla
One, League Gothic, Oswald, Saira Condensed, Teko, Unbounded. Mono: DM Mono, Fira Code, Geist Mono,
IBM Plex Mono, JetBrains Mono, Roboto Mono, Space Mono.

**Picking by voice.** The work speaks in a register (official, intimate, exacting, cheeky). Pick the
kind of face printed on the object where that voice lives in the world (a railway timetable, a
wedding invitation, an engineering spec sheet, a gig flyer, a pharmacy label). Category rules do not
hold: editorial work sets well in a grotesque, a technical one in a serif, a children's one in
something sharp.

**Pairing.** No near-twins: two geometric sans, two humanist sans or two old-style serifs look
faintly wrong and give no hierarchy. Use one family at two weights, or faces that differ in more than
one way: serif and sans, mono and proportional, condensed and wide. One face performs, the other
supports; two characterful faces share a frame only when they share a temperament. Give each kind of
content its own voice: claims in one face, figures in a mono, names and sources in the text face. A
mono held to numbers and labels adds a voice, not a third character. A good pair carries a contrast
already in the content (hand against machine, loud against quiet). Contrast inside one family works
too: proportional and mono cuts of one design (IBM Plex Sans with IBM Plex Mono, Geist with Geist
Mono), an upright with its italic, one face at display and at text size.

**Weight, size, tracking.**
- Push weight contrast hard: 400 against 700 disappears once things move; video wants about 300
  against 900. A family drawn at one weight (Anton, Bebas Neue, Dela Gothic One, Archivo Black and the
  other single-weight display faces) takes its contrast from the face beside it.
- Size for the screen it plays on. Full screen (YouTube, an embed): headlines from 60px, body from
  28px, data labels from 24px (5.6%, 2.6% and 2.2% of a 1080 frame). Small in a feed (X, LinkedIn,
  Instagram): headlines from 90px, body from 32px, labels from 24px (8.3%, 3.0% and 2.2%). Nothing on
  video goes under 24px on a frame 1080 high, or 43px on one 1920 high: the floor scales with the
  height, and `check_timeline` reports against it. Web instinct says 14px; on a screen that is chrome.
- Judge a still at the size it is seen. A thumbnail is a few hundred pixels wide among others, so its
  few words run at feed sizes or larger. A slide shown in a room holds nothing under about 40px at
  1080, with its headline at 72 to 96px and its body near 48px.
- A line that carries the message stays at or above 1.4% of the width (27px at 1920); smaller type is
  chrome (a page number, a catalog mark).
- Fit to measure: cap a headline block at 78% of the width and let its word count pick the tier:
  three words or fewer get the largest display size, four to six the next, seven or more the next
  down. A hero line spans 60 to 80% of the frame; step a long line down rather than run it edge to edge.
- Track display tighter than on the web, -0.03em to -0.05em, since compression blurs the gaps between
  letters; open uppercase labels up instead, +0.08em to +0.18em.
- On a dark ground pale type looks bolder and more crowded than on paper. Step body down a weight (300
  rather than 400), open line height by 0.05 to 0.1, and give display +0.01em of tracking so the glow
  around each letter does not close the gaps.
- Set every number that ticks, stacks or lines up (a counter, a clock, a column of stats) in tabular
  figures; proportional digits differ in width and a column goes ragged.

## The scale

| Element | On video | Of a 1080 frame | Web habit |
|---|---|---|---|
| Headline | 64 to 120px | 5.9 to 11.1% | 32 to 48px |
| Body | 28 to 42px | 2.6 to 3.9% | 14 to 16px |
| Label | 24px, the floor | 2.2% | 12px |
| Padding | 60 to 140px | 5.6 to 13% | 16 to 32px |
| Border | 2 to 4px | | 1px |
| Decoration opacity | 12 to 25% | | 3 to 8% |

The codec erases decoration under 10% opacity; a hairline in a look is a 2px rule at 1080.

## Depth

Each depth has a job. Back: a soft radial light, a word set huge and cropped by the frame at 12% or
more, a panel of color, grain, a grid, or a plain field when the idea calls for one; bare #000 reads
as something that failed to load. Middle: the message, the figure, the card, the code, the picture.
Front: rules, small labels, bars of data, crop marks, metadata in a mono, the small things that make a
frame look made by someone. A produced marketing frame suits six to ten distinct roles; a lower third
or a logo sting needs a few. Decoration carries hierarchy, motion or the idea, never new copy or a
claim nobody made. It does not breathe, drift or pulse: it arrives, then holds while the content
moves.

## Color that survives the codec

- Give every scene one color that draws the eye; quiet color is fine, lifeless color is not.
- Let the accent show: 15 to 25% opacity as atmosphere, full strength where it is the focus; a glow
  at 5% is gone after compression.
- On a light canvas, firm up the structure rather than switch to dark: solid borders of 2px or more,
  rules and dividers, the accent at full strength, and some texture (fine grain, a grid, a pattern)
  so the ground does not read blank.
- A full-frame linear gradient on a dark ground bands in H.264, and again when a platform recompresses
  a still. A radial, a flat fill, or a flat fill with one local glow holds.
- Lean every neutral toward the palette. An untinted gray looks undesigned; pure #000 and #fff are
  defaults; a near-black and a warm or cool off-white carry the palette.
- Text reads with the decoration removed: contrast comes from the type against its ground. The
  accessibility standard (WCAG) asks for at least 4.5:1 for body text and 3:1 for large display
  type. The text's color is on its layer (`get_canvas`); over a photo, `inspect_color` `points` on
  the picture's item where the words sit give the ground, and on video the same on a timeline frame.

## Composition

- Give a frame at least two points of interest; one block of text alone in space is a web page.
- The squint test: one element outweighs its nearest rival by three to six times, a clear gap rather
  than a slope. Two competing headlines means neither leads.
- Pin content to an edge, top left or bottom right. Floating in the middle is a web reflex; centered
  is for a solemn close, a quote or a wordmark.
- Zones: figures left and story right; a thin band of metadata above a full-width field; two flat
  color fields meeting at a clean line, split 38/62, 40/60 or 50/50.
- Empty space has a target: a declarative frame (cover, statement, quote, close) is 45 to 60% empty.
  A look allows one busy frame, its figures or its list, busy by repeating one quiet unit rather than
  adding kinds of thing. Do not fill a sparse frame to make it seem finished.
- Rules, dividers and panel edges draw the structure and are easy to animate.
- Every line connects two things. A beam, rail, connector or drawn underline starts at one real
  element, ends at another, and does one job (revealing, routing, confirming, stressing). If deleting
  a line loses nothing, it was ornament. A lit edge on a card or a status dot reads more deliberate
  than an arc floating free.
- In work with several sections, no three in a row share one anchor.
- The subject's own words, figures and files beat a stock icon. A prop that would fit another
  company's video unchanged is not from this one.

**Captions and platforms.** With captions on, keep readable small text out of the caption line's
span: at 16:9 and 1:1 the bottom sixth or so; at 9:16 roughly 64 to 76% of the height, where the line
sits just above the platform's own band. Imagery, cards and a full-bleed subject may run under it.
Compose on the frame's true center, not a lifted one; with no captions there is no band. On a
captioned video a lower third sits above the caption line, never in its span. At 9:16 the platform's
interface is a separate keep-out whatever the captions do: the top 12%, the bottom 24%, and on TikTok and Reels the right-hand column of buttons, about 10% of the width. Place the
hero high in the middle, clear of all three.

**Other ratios.** A row of three stacks on 9:16 and goes 2+1 or 2x2 on 1:1. A side panel becomes a
top or bottom band; a four-column list drops a column; decoration thins on tighter frames. Key display
size off the shorter side so portrait never blows a headline out. Every message line stays above the
floor.

## Pictures are never flat

A picture dropped in as it came reads as a placeholder. On a canvas, crop to the subject, then commit
to an edge, a frame, a hard color region or a full bleed; a small rectangle adrift in space is the
failure. On video, use a settle (in at 1.06, easing to 1 over about a second, then held), a phone or
laptop body drawn around a screenshot, one element lifted out of a screenshot and moved at a different
speed from what is behind it, or a window (the picture masked to a region and slid vertically inside).
A slow push that runs the length of the beat is drift, not a treatment.

## Words on screen

- Screen copy is not the script: a hero word, a figure, a single stressed word. A voiceover sentence
  on screen prints the spoken words twice, since captions already carry them.
- Hold text about 0.5 seconds plus 0.3 seconds a word, the last line at least 2 seconds. When it does not fit, use fewer words, larger.
- Take figures from the brief, the transcript or the source; with no figure, a card carries a word.
- On video the subject shows by 0.5 seconds: start the first move 0.1 to 0.3 seconds in.
- A still says one thing: a headline of a few words and at most one supporting line.

## Defaults that read as generated

What a model produces when nobody decides: a dark background with glowing cyan, violet fading into
blue, neon accents; text filled with a gradient; a colored bar down the left of every card; pure #000
and #fff; a grid of cards all the same size; everything centered at equal weight; the overused faces
above. Each is fine when the content asks for it (a centered close that should feel solemn, cards that
depict a real product, a common face that fits); the test is whether you can name the reason.

## The check, on real frames

The frames `inspect_timeline` draws of a graphic or any other moment, or the slides
`inspect_canvas` draws, are what the export shows. Run this list against them:

- Squint: one element outweighs its neighbor three to six times, and the eye has a second place to go.
- Space: declarative frames 45 to 60% empty; only the look's one busy frame is busy.
- Scarcity: every rare gesture used once, one accent moment, one signature device, one hard shadow.
- Palette: every color in the declared palette or the brand kit; one ground across sections; no pure
  #000 or #fff unless chosen; no full-frame linear gradient on dark.
- Type: catalog families at weights they ship, one expressive face, fit within 78% of the width,
  message lines at 1.4% of the width or more, display tracked tight, tabular figures on every number
  that counts.
- Composition: anchored and zoned, no three sections sharing an anchor, every line with two real ends
  and a job, no small readable text in the caption span when captions ride.
- Words: screen copy readable in two seconds, no voiceover sentence, no invented figure.
- Pictures: none flat. Defaults: each one present has a reason. Brand: families, weights, corners,
  spacing, depth and the kit's don'ts match. The look: its reference can be named and the frame shows
  none of its failure.

## Handover

- A still or a slide: drawn by `inspect_canvas` at its export size with no word cut off, overlapping or off the slide; the canvas is in the app from its first layer, and `render_canvas` makes a file for use elsewhere.
- A frame of video or a graphic: checked on the timeline's own frames (`inspect_timeline`), and the timeline in the app.
- Work done for another skill's job: that skill's handover as well.
