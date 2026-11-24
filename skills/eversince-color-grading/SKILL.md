---
name: eversince-color-grading
description: "Corrects shots to one reference, then one look over the whole video. For a cut whose pictures do not match, or a grade."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-color-grading"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Correct first, then style. Shots that were never shot together don't cut as one world until their
brightness, contrast and color agree. The look (warmth, fade, mono) goes on after that, once, across
all of them. Do it the other way round and the look hides the mismatch while you work, then the
mismatch shows up again in the finished cut. A sequence graded shot by shot to taste drifts; one
corrected to a reference and styled as a whole holds together. A brief, reference or brand from the
user outranks every default here.

## The plan

1. Read every clip's grade and draw frames either side of each join where the source or the light changes
2. Pick the reference shot
3. Correct each shot toward it: exposure, then contrast, then small temperature and tint, saturation last
4. One look over the whole piece on an adjustment clip, only if the user wants a look
5. Check every shot again with the look on, then hand over

## Looks people ask for

- **Film emulation** (Portra, Kodak Gold, Fuji): the most requested look right now. The filters (`get_templates` kind filter) include these, with their own grain.
- **"Cinematic"** usually means gentle contrast, slightly lifted blacks and restrained color. Heavy teal and orange now dates a video. The modern version keeps skin natural and cools only the background: put the look on the frame and pull it back on the person with a person mask's grade, or put a cooler grade on the inverted mask (`skill-video-effects`).
- **Muted or desaturated** is common and easy to overdo until it looks flat.
- **Native social posts** often look better corrected and lightly graded, or not graded at all; a heavy look can make a creator's post read as an ad. Whether to add a look is the user's call.
- A creative look or LUT usually works at about 50 to 70 percent strength, not full.

## What the grade is

Per clip, on `updateClip`: a preset and its intensity (an everyday filter, or a film emulation with
its own color table and grain; `get_templates` kind filter lists them with what each suits),
brightness, contrast, saturation, temperature, tint, exposure, highlights and shadows, the three
wheels (lift, gamma, gain), and a LUT applied last. The patch replaces the whole grade, so to change
one value, read the clip's current grade (`get_timeline` detailed) and send it back whole with your
change in it. Fields from other systems (curves, whites, blacks, vibrance) are refused with the list
of valid ones. What you see rendered is what exports: the grade is one pass on the clip's own pixels,
inside the same composition the export uses.

How the numbers behave in the renderer:

- Exposure and brightness add into one term: exposure 20 plus brightness 20 is brightness 40. Move
  one and leave the other.
- Temperature is asymmetric: warm lays a sepia cast, cool rotates the hue toward blue. Warming 30 then
  cooling 30 does not get you back to the start, and a heavy warm value drags color toward brown. Use
  small values and judge them on the frame.
- Tint rotates every hue in the frame, skin, sky and product together. It removes a cast; it can't
  shift one color.
- Highlights and shadows are luminance-weighted: shadows 30 opens up the dark end and leaves the mids
  and highlights alone.
- Contrast at -100 is flat gray, at 100 a poster. Corrections belong well inside a third of the range;
  past that, the number is why the shot looks odd.

**To grade many clips at once, use an adjustment clip**: a clip of type adjustment on a visual track
above the pictures, spanning the stretch. Its grade and effects apply to every picture clip below,
after their own. Its opacity is its strength and can be keyframed, so a look that arrives at a turn
or fades out is the layer's opacity moving; an effect's number parameters animate on their own
(`fx<n>.<param>`). A clip's own grade can't change over time. Corrections go on the clips, the look
goes on the layer, and a shot added later picks it up automatically.

**To grade part of the frame, use a mask's grade**: a mask on the clip carries its own grade, and it is
the only way to limit exposure, saturation or a LUT to a region. Use a drawn ellipse or rectangle for
a corner or a vignette, and a person mask for the subject or, inverted, the room
(`skill-video-effects`).

Stills work the same way. A photograph on a canvas carries the same grade on its picture layer
(presets and grain included), an adjustment layer over several layers is the canvas version of the
adjustment clip, and a set of photographs is matched to a reference photo and given one look exactly
like a cut (`edit_canvas`; finishing a single photograph is `skill-photo-editing`). Canvas layer
effects are limited to shadow, blur, glow and duotone. The medium looks below that need halftone,
scanlines, noise or a vignette have to be built on a timeline, so on a still the look comes from the
preset, the wheels and duotone. Library items themselves can't be graded.

## What to leave alone

- Screen recordings, interfaces, slides, logos and a product's brand color are the thing being shown.
  They get framing and pacing, not color, unless a camera filmed a screen with a real cast, and then
  you fix only that.
- Deliberate light (a sunset, neon, a stage wash) is a look someone chose. Correcting it toward
  neutral destroys it.
- A clean source needs no correction. Changing numbers is not evidence of work.
- Footage that arrives flat and gray with no real contrast was shot in a camera's log profile on
  purpose. It needs the conversion its camera expects, a .cube LUT kept as a library document. The
  grade has no built-in log conversion, and nothing here reads an item's color profile, so the picture
  is your only tell. iPhone footage can be Apple Log or Apple Log 2, and each needs its own conversion
  LUT; the wrong one under-corrects the color, so find out which phone and setting when it
  matters. A phone clip that looks flat and gray next to the others may have been shot in HDR;
  correct it toward the reference like any other shot.
- Phone footage arrives already processed: sharpened, saturated, shadows lifted. When it's mixed with
  camera footage, calm the phone clips down toward the camera's instead of pushing them further.

## LUTs

A camera's conversion LUT goes alone on the clip's own grade at full intensity. Because the LUT runs
after the knobs, any correction on that clip would happen on the flat log picture before conversion.
So correct that shot on an adjustment clip spanning it, whose grade applies after the clip's. A
creative LUT is a look: it goes where the look goes and is blended well under full strength. Keep
exposure, contrast and warmth on the knobs, where the next pass can read and adjust them.

## The order of work

1. **The reference shot**: the one closest to right already, or the one the piece can't do without
   (the hero, the face, the product). Everything else comes to it. There is no absolute neutral, only
   agreement.
2. **Exposure, then contrast**: match brightness across shots before color, because a cast reads
   differently at a different exposure. Then contrast, lightly.
3. **Temperature and tint, small**: they pull a cast toward the reference; they don't rebuild a shot's
   color. Viewers check faces first: hold the highlights, open the shadows a touch, keep warmth near
   zero. On a product, keep both at zero unless a frame shows a real cast, because the product's color
   is the claim.
4. **Saturation last of the corrections**, usually down. Generated footage arrives more saturated than
   photographed footage, and that is what gives away which shots in a mixed sequence were made.
5. **The look, once**: one preset at one intensity across every shot, one look built from effects on
   the adjustment clip, or none. A look picked per shot is just drift.

If the material comes from one source and already agrees, steps 1 to 4 collapse and the job is step 5.

## Matching in practice

- Judge on rendered frames, not numbers. Two clips can carry the same grade and still disagree,
  because the grade is what was added, not what is there. `inspect_timeline` draws the frames.
  `inspect_color` measures one (black and white points, clipping, the balance of shadows, mids and
  highlights, saturation, bias), and against a reference frame it returns the gap as hints on the same
  fields the patch carries. `reference_at_seconds` is in the reference item's own seconds: the clip's
  trim plus its offset into the clip. Without a reference it returns `correction`; when its gain levels the neutral areas but the frame
  stays colored, `correction.whole_frame.gain` is the stronger number. The numbers narrow the
  correction; the frame decides it. Hints from a reference can overshoot (large lift and gamma moves left a face gray in a test), so apply part of them, a third or so, read again and judge the frame.
- Take frames either side of the joins where the source, the camera or the light changes, not the
  middle of each shot: a mismatch lives at the join, and cuts within one shot or one camera's run
  don't need a pair. In a long shot where the light moves (a cloud, a door, a walk out of shade), also
  take early, middle and late frames.
- When a run of shots comes from one source and a single shot from another, correct the single shot,
  not the run. Which clips came from which camera or model is in `get_items` provenance and technical
  data; the timeline read has no source label.
- Shots from one camera and one setup share a correction: settle it on one, `copy_clip_settings` onto
  the rest, then measure each, since two shots rarely want exactly the same values. It is a match, not
  a merge: framing, opacity, masks, effects, fades and transitions travel with the grade, and whatever
  the source lacks is cleared on the target. So for a clip with its own mask grade, effects or framing,
  apply the grade with updateClip instead.
- `glow` spreads light, so a bloomed frame looks blown out while measuring as less clipped. Judge
  `halftone` and `scanlines` at delivery size, since a reduced frame invents moiré or hides it.

## Looks built from effects

Some looks are a medium rather than a color: shot on a home camera, shown on an old TV, printed. Build
each from the effect stack on the adjustment clip and commit to it fully; a medium at half strength
looks like an accident. Use one medium per piece. "Vintage" could mean any of these, and mixing them
muddles when the footage is supposed to be from.

- **Home video**: contrast up and saturation down, so the picture is hard but dull (hard and vivid
  looks like a phone app), `noise` for grain, a `vignette`, static `scanlines`, a touch of
  `chromatic`, a gentle `glow`.
- **Tube**: `fisheye` to curve straight lines, `scanlines` close together, moderate `chromatic`,
  more contrast and a gentle `glow`, or it reads as an overlay on modern footage. The curve pushes
  content toward the edges, so a `vignette` on top grays out anything bright that lands there: keep it
  light, or use a wider `radius`.
- **Film memory**: warmth, grain and a soft vignette, kept low, for a flashback the story calls for.
  It is not what "cinematic" means by default.
- **Print**: `halftone` for dots in the picture's own colors, or `duotone` for two inks as exact
  hexes. Halftone works on light, high-key material with the subject in the lights, with a `dotSize`
  about one and a half times its `dotSpacing`; a dark shot sinks into black.
- A damaged cassette, with the picture rolling and breaking up, can't be built here; home video or
  tube is the closest.

Each effect needs something to work on: `halftone` and `duotone` need tonal range, `fisheye`
straight lines, `glow` bright areas.

## The tools in order

`get_timeline` detailed returns every clip's current grade, which is where a change starts.
`inspect_timeline` and `inspect_color` are how you judge. `edit_timeline` updateClip writes `grade`
and `effects` whole (null clears). For the look, `addTrack` {track: {kind: "visual", index: one
above the highest picture track, since a higher index draws on top}} and a hand-built `addClip`
{trackId: that track, clip: {id, type: "adjustment", from, durationInFrames, grade, effects}} spanning
the cut (`skill-timeline-operations` has the fields). Doing the grading pass as its own revision
leaves `undo` a clean step back.

## The check

On frame pairs either side of the joins where the source or the light changes:

- Nothing jumps at a cut: brightness, warmth, saturation. The look sits on every shot at the same
  strength.
- People who appear in more than one shot look like the same person in the same room; dark skin isn't
  flattened or grayed; walls and clothing have no cast they didn't have before.
- Screens, logos and brand color are as they arrived.
- A stranger can't tell which shots came from where. If the generated shots are the bright, saturated
  ones, the correction pass didn't happen.
- The darkest parts are still dark, not crushed; the brightest still hold detail. Phone screens crush
  very dark tones and can tint them, so keep a little detail in the shadows.
- The look hasn't pushed skin orange or magenta, or oversaturated skies and skin; if it has, pull it
  back.
- With the look on, check every shot again: a look can pull one shot away from others it matched
  before.

## Handover

- Every shot corrected before any look; any look the user asked for on one adjustment clip over the
  whole cut, so it can be changed in one place.
- The piece is in the app from its first media, where it plays; the result's app_url is the link
  the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: a look from the filters (`get_templates` kind filter), a match to a reference the
  user brings (`skill-remake-from-a-reference`), a medium look (home video, old TV, print), a mask's grade on
  the subject or the room.

## An example plan

A cut from mixed sources, corrected, and given one look in step 5 only where the user asked for a look:

1. get_timeline {timeline_id, response_format: "detailed"}
2. inspect_timeline {timeline_id, seconds: a frame either side of each join where the source or light changes}
3. inspect_color {timeline_id, at_seconds: a frame of the shot to correct, reference: the reference shot's item, reference_at_seconds: the moment in that item's own seconds (the clip's trim plus its offset into the clip)}
4. edit_timeline {timeline_id, if_revision: from step 1, operations: [updateClip per corrected clip with its grade whole]}
5. edit_timeline {timeline_id, if_revision: from step 4, operations: [addTrack {track: {kind: "visual", index: above the pictures, id: "look"}}, addClip {trackId: "look", clip: {id, type: "adjustment", from: 0, durationInFrames: the cut's length, grade: the look, effects}}]}
6. inspect_timeline {timeline_id, seconds: the same frames as step 2}
7. check_timeline {timeline_id}
8. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over
