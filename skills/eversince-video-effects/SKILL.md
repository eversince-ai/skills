---
name: eversince-video-effects
description: "Named effects on a video: text behind a person, a new background, speed ramps, shatter, a 3D screen, designed transitions."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-video-effects"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

An effect lifts one moment: a reveal, a turn, the line that matters. Used once or twice in a video,
it reads as intent; on every cut it becomes noise, and "doing too much" is the most common complaint
about edited content. If the user names an effect, build that one. If they ask for effects in
general, pick the moments where one helps and leave the rest of the cut clean. A trending effect wears out once viewers can predict it, so the moment matters more than the effect. A brief, a reference
or a brand from the user outranks every default here.

## The plan

1. Read the video: `get_timeline` and `get_transcript` (or `inspect_media` where there is no speech) to find the moment each effect serves
2. Build each effect as its entry below says: on the timeline (`edit_timeline`), or as a motion graphic (`compose_motion`)
3. Draw frames before, in the middle of and after each effect: `inspect_timeline` {timeline_id, seconds}, one call
4. `check_timeline` {timeline_id}, then `publish_preview` {timeline_id}: the link to hand over

## Built on the timeline

Timeline effects stay editable: the user can trim, move or remove them in the editor.

A `person` mask written in the same `edit_timeline` call that adds its clip is stored but not read. Add the copy first, then set the mask with `updateClip` in a second call.

**Text behind a person.** A word sits between the person and the room. Three layers, bottom to top: the shot; the word (a text clip or a graphic); a copy of the shot at the same trim with a `person` mask (set by `updateClip` in a second call) and its sound off. The word is about a fifth of the frame's height, wide enough that the head covers a third to half of it, on the side with more space, starting on the word's frame from `get_transcript`. Any split, speed change or zoom on the shot is made on both copies. Detail: Masks, cutouts and keys, below.

**Lit room.** The room darkened or blurred so the speaker stands out. One `updateClip` on the shot: `masks: [{id, shape: "person", invert: true}]` with a grade on the mask (exposure and saturation down), or an effect named to it (`blur`, radius 12 to 24). The commonest effect in one operation.

**Color pop.** The person in color, the room in black and white: the inverted `person` mask with saturation at -100 on its grade (0 leaves the color as it is).

**Vertigo.** The background stretches away while the person holds still, the dolly zoom faked. It needs the room without the person: a moment of the shot where they are out of frame, or a still of the empty room. That plate goes on the bottom with `scale` keyframes (1 to about 1.3 over one to two seconds, linear); the shot goes on top with a `person` mask (set by `updateClip` in a second call) and no scale change. Scaling the shot itself instead shows a second, larger person peeking out behind the masked one. It reads on a person framed head and shoulders in a room with depth. `check_timeline` notes that the copy no longer lines up with the shot; here that difference is the effect, so expect the note.

**Textured room.** Grain, scanlines or halftone behind the person only. The same three layers as text behind a person: the shot with the effect (`noise`, `scanlines` or `halftone`) on the bottom, the person-masked copy on top, its mask set in a second call. An effect's number takes keyframes (`fx0.amount`), so the texture can build over the shot.

**New background.** A green or flat-color screen: the `colorkey` effect, measured with `inspect_color`. Anything else: `remove_background` with `as: "clip"` (credits). The new background goes on the track below. Matching its light to the person is what makes it read. Detail: Masks, cutouts and keys, below.

**Freeze and lift.** The frame holds and the person lifts off it. `extract_media` mode frame at the moment, `remove_background` on that still (credits), the still on the timeline for the hold and the cut-out on a track above, rising about 5% in scale over half a second (`setKeyframes`) and settling.

**Speed ramp.** Slow into fast, on a beat. `splitClip` where the pace turns, `setSpeed` on each part (0.5x into 3x, say), a `blur` transitionIn of 4 to 6 frames on the fast part, the beat landing where the fast part starts, the clip's own sound off under it.

**Impact shake.** The frame jolts on a hit: the `shake` effect on an adjustment clip of 6 to 10 frames starting on the hit, its `fx0.intensity` keyframed from about 0.8 to 1 down to 0.

**Stutter.** A moment repeats two or three times: `splitClip` around a short part (4 to 8 frames) and the same span added again right after it, the repeats back to back, then the shot plays on. The parts are too short to sit on separate beats, so the stutter as a whole lands on a beat.

**Echo trail.** A moving subject leaves ghosts behind it: two or three copies of the clip on tracks above it, over the same timeline span, each 2 to 4 frames further behind in its source than the one under it, at opacity about 0.3, 0.2, 0.1, sound off. Stronger ghosts outweigh the live picture. The still background matches in every copy, so only the moving subject doubles. It reads best on a moving subject against a dark or plain still background.

**Slow motion.** `setSpeed` below 1 on one gesture or moment. It plays smooth when the footage was shot at a higher frame rate (60 fps at 0.5x on a 30 fps timeline); 30 fps footage slowed stutters, so keep it short or skip it.

**Zoom transition.** The outgoing shot zooms in fast and the next one arrives zooming out: `transitionIn` style `zoom` on the next clip, 6 to 10 frames, or `scale` keyframes on both sides with `zoomblur` on the frames around the cut.

**Whip pan.** A fast blurred pan hides the cut: `transitionIn` style `whip` with a direction, 8 to 12 frames, or the shelf graphic transition-speed-smear over the cut. It reads best when both shots move the same direction.

**Stop motion.** `holdFrames` 2 or 3 on the clip.

**Match cut.** Two shots joined on the same shape or movement: a ball becomes the moon, a door closes and another opens. It is a choice of shots, not an effect: find the pair (`inspect_media` window on the candidates) and cut on the matching frame, the shape in the same place in both.

**Cover cut.** A hand, a jacket or an object passes over the lens and the scene changes behind it. It has to be shot that way: cut on the frame where the cover fills the screen, the next shot starting with the cover moving away.

**Letterbox.** The shelf graphic frame-letterbox.

**Color pop in a circle.** The shelf graphic lens-color-spot.

**Freeze intro.** The shelf graphic frame-freeze-cutout.

**VHS.** The VHS filter (`get_templates` kind filter) as the grade; the shelf graphic lens-tape-roll for one tracking roll.

**Split screen.** Two or more shots in one frame: `apply_layout` with a layout and a clip per slot. For a before and after, the same moment in both.

**Punch-in.** A zoom on the strong word: `auto_zoom`, or a `scale` keyframe from 1 to about 1.15 over 3 to 4 frames on the word. More than one a sentence tires the eye.

**Face blur.** `pixelate` confined to a drawn ellipse with room around the head, at one strength from the first frame to the last. Where the head moves past the ellipse, confine it to a cutout instead.

**Pull-back reveal.** The shot shrinks into a card inside the next scene. The next shot on the track below, the current one above it with `radius` set (about 24) and `setKeyframes` on `scale` (1 to about 0.4) and `position` to where the card ends, over 12 to 18 frames, smooth. The end position {x, y} is the card's center minus 0.5, in frame fractions: to end centered at 0.75, 0.75, set position 0.25, 0.25. The card can hold there or slide off.

**Infinite zoom.** Each shot sits inside the one before. The outgoing shot zooms into a point (px, py, as fractions of the frame) while the next shot, on the track above, grows from that point to fill the frame, over 12 to 20 frames. For the outgoing shot at scale s zooming into that point, the position is x = (0.5 − px) · s and y = (0.5 − py) · s, kept within ±(s − 1)/2 so no edge shows. The incoming shot grows from that point: position x = (px − 0.5) · (1 − s) and y = (py − 0.5) · (1 − s), with s from about 0.05 to 1. Pick points where something in the frame earns the zoom: a screen, a window, an eye.

**Portal.** A shape opens onto the next shot: `transitionIn` with style `iris` on the next clip (a circle), 12 to 20 frames. Another shape is the shelf graphic transition-shape-bloom.

**Head pop-out.** The person breaks out of a frame. The shot on the bottom with `crop` insets (top about 0.25, sides and bottom about 0.1) and `radius` making it a framed card over a background (a solid clip or a graphic below it); a copy of the shot on top, at the same trim, with the same side and bottom crop but no top crop, and a `person` mask set in a second call, so the head rises above the card's edge. It works when the head sits near the card's top. `check_timeline` notes that the copy no longer lines up with the shot because the crops differ; expect it.

**Clone room.** The same person several times in one shot. It needs footage made for it: the camera locked off, the light unchanged, one take per position, nobody crossing another position. Stack the takes on tracks at the same trim, each above the first with a `person` mask, set in a second call once the takes are on.

## Built as a motion graphic

A motion graphic draws what the timeline cannot: fragments, depth, designed shapes. Code craft is in `skill-motion-graphics`. Footage goes inside a graphic as a `video` control and a still as an `image` control, each set in `values` to the library item's ref.

**Designed transitions.** The Transitions shelf (`get_templates` {shelf: "Transitions"}) lists every designed transition. Place one with `compose_motion` `template`, centered on the cut, so the frame is covered at the cut point. The built-in clip transitions (`transitionIn`: fade, dipwhite, dissolve, wipe, push, slide, iris, clock, zoom, blur, whip, spin) are plainer and need no graphic. Use one style through a video.

**Accents on a cut.** Short, a few frames to under a second:
- Flash: `transitionIn` style `dipwhite`, 2 to 4 frames, on a beat.
- Whip or spin: `transitionIn` style `whip` (with a direction) or `spin`, 8 to 12 frames, the cut on a beat; the motion in both shots runs the same way.
- Light leak or film burn: the `lightleak` effect on an adjustment clip over the cut (`hueShift` changes its color), or the shelf graphic texture-light-leak.
- Glitch: an adjustment clip of 4 to 8 frames over the cut with `chromatic` (amount 10 to 30) and `shake`.

**Screen in 3D.** A screen recording tilted in space. A graphic with a dark ground, the recording inside a box with rounded corners and a shadow, under `perspective` (about 1600) with `rotateX` and `rotateY` animated between views (within about ±25°), springs with no bounce. Scale the box to about 0.65 of the frame so its near edge, which grows with the tilt, stays inside. The shelf graphic device-floating-screen is a ready version.

**Shatter.** The shot breaks into pieces that fly apart, revealing the next shot. Freeze the outgoing shot's last frame as the cut shows it (`extract_media` with `timeline_id` and that frame, cloud processing; from the source item it comes back ungraded and unreframed) and pass it to a graphic as an image control. The graphic cuts it into triangles (a grid of 6 by 4 cells, inner points jittered, each cell split in two) with `clipPath` polygons. After a hold of about 8 frames, each video flies outward from the center, with an ease-in, a little gravity, rotation and a fade at the end, the pieces nearest the center first, over about a second. Random-looking values come from a function of the video's index, since Math.random is blocked. Place it at the cut, with the next shot on the timeline below.

**Photo in depth.** A still photo moving in 2.5D, the subject separated from the background: `remove_background` on the photo (credits) gives the subject; the full photo on the bottom and the subject on top, both slowly scaling over the shot, the subject a little more (1 to 1.08 against 1 to 1.04) so they part. The hole the subject leaves stays hidden while the movement is small.

**Photo stack.** Stills land like prints thrown on a table, on the beats. A graphic with up to twelve image controls, each print with a white border and a shadow, dropping in from about 1.3 scale with a slight rotation (±8°) and settling over 6 to 8 frames, the earlier prints staying underneath.

## Masks, cutouts and keys

The separations most effects above stand on, in depth.

### Three separations

- **A person mask**, for the people near the camera: a mask of shape `person` on the clip
  (`updateClip` `masks`); `invert` with a grade or an effect on the mask treats the room
  instead (alone, a person mask draws the people whichever way round). The region is found over the
  clip's own frames, on cloud processing, never credits,
  and kept, so a second effect on the same clip costs nothing again. Two people in shot are one
  region. A small figure or nobody in shot is refused with a sentence. The first reach for a talking
  head.
- **A cutout**, `remove_background`, in credits. On a video it keeps the subject and drops what is
  behind, as a WebM with transparency at up to 1080p; it separates human from not-human across the
  whole frame, so several people are all kept; it holds on a presenter and comes apart on fast
  motion. With `timeline_id` and `clip_id` only the stretch the clip
  plays is cut, and `as` decides what it makes: `clip` takes the clip's slot with the person alone,
  for playing something else behind them; `mask` attaches the cutout to the still-playing clip as a
  `matte` mask, for what the person mask refuses (a small figure, a product, anything with glass).
  On an image it is saved as a transparent PNG beside the
  original. A cut inside the stretch to other footage wrecks the cutout from that frame on, so the
  clip is trimmed to the shot first.
- **A color key**, the `colorkey` effect, for a background that is one flat color put there on
  purpose: a screen, a painted wall, a paper sweep. Four numbers on the clip, nothing to pay, the
  right tool wherever there is a screen; the cutout is the fallback.

### What a mask does

A mask no effect names shapes the clip: only what is inside it is drawn. A mask an effect names by
`maskId` confines that effect to the region, the rest untouched; each named mask costs the clip
another draw of its media. A mask carries a grade of its own, the only way to hold exposure,
saturation or a LUT to part of the frame. Inverted, the same mask is the room. Both halves treated
at once is the same cutout twice, one inverted, each with its own grade or effects; most shots want
one half treated. An effect that clears pixels away (a key, halftone) is confined to a drawn shape
or to an inverted cutout, not to the person's own cutout.

The fields, as `skill-timeline-operations` lists them: `masks: [{id, shape: "person", invert?}]` on
the clip, and an effect confined to it as `effects: [{type: "blur", params: {radius: 24}, maskId: the
mask's id}]`. A person mask is read from the frames after the write lands: the response's warnings say
the region is being read and `job_ids` names the job per mask (`get_jobs` with them waits), the mask
draws once found, and a frame drawn before that shows the clip plain.

### Something between them and their room

Three layers, bottom to top: the shot; the title, color wash, B-roll or graphic; the same shot again
showing only the person, either the cutout item or the same source added again with the same span
(`addClip` with `ref` and `source`, or `copy_clips` for the whole clip) carrying a `person` or
`matte` mask that no effect names. The copy's own sound is off (`updateClip` volume 0), or the
voice plays twice.

The copy is the same source at the same trim as the clip underneath, or the person on top drifts off
the person below. A split, a slip or a speed change on one is the same change on both, and so is
anything that moves or bends the picture (a shake, a fisheye, a punch-in): on an adjustment clip
spanning both, or the same entry on both. `auto_zoom` and `auto_reframe` write onto one clip, and
`copy_clip_settings` carries an animated framing as its first frame only, so a zoom on the pair is
the same `setKeyframes` track written onto both.

**A word behind someone** is a text clip (`content`, `fontSize`, `position`) or a graphic, starting
on the word's from_frame from `get_transcript`, around a fifth of the frame's height and wide enough
that at its peak their head covers a third to half of it: less reads as a label floating beside them,
more and the word is eaten. Readable comes first: the word shrinks before the head takes whole
letters. It goes on the larger clear side when they sit off center, in the middle only when they are
near the middle with clear space either side and the word runs well past both shoulders. At no
moment is more than about two thirds of their face covered; one such word at a time, a beat apart
from the next.

### One half treated, the other not

A `person` mask with `invert` and a grade is the commonest shot in one operation: the room taken
down (exposure and saturation on the background's grade) so the speaker lifts off it, the cheapest
way to make a flat room look lit. The other treatments are an effect named to the inverted mask:
`blur` on the room and the person sharp, the depth of field a phone lens never had; `noise`,
`scanlines` or `halftone` behind them so they sit in front of a texture rather than in it;
`vignette` or `glow` inside the person's own mask. An effect's number parameters take keyframes
(`fx<n>.<param>`), so the room's blur or grain builds over the shot while the person stays untouched.

**A redaction** is usually a face and needs no cutout: `pixelate` confined to a drawn ellipse with
room around the head (hair, ears, chin), at one strength from the first frame to the last; a
pixelation that fades in reads as a flourish. A drawn mask does not move, so where the head travels
further than the ellipse's margin the pixelate is confined to the person's cutout instead, which
hides the whole person but follows them.

### The key

Four parameters in order, the first three measured, the result judged on one `inspect_timeline`
frame of the composite, since a key's edge is judged by its look.

1. **`keyColor`**, the color the screen actually is. The default is pure green, which no camera has
   recorded; a real screen lands muddier and darker. The hex is measured, not read by eye:
   `inspect_color` on the item at a moment with `points` in the middle of the screen, away from the
   subject and the edges. Most of the job is here.
2. **`similarity`**, how far from that color still counts as screen: the straight distance in RGB
   divided by √3, so black to white is 1. `inspect_color` with `key_color` reads the frame's
   distances: how far the screen's pixels reach, where the rest of the picture begins, and in
   `key.suggested` a `similarity` and `smoothness` that fit between them. A screen whose reach overlaps the subject
   means `keyColor` is wrong or the screen is lit unevenly.
3. **`smoothness`**, the softness of the edge: a pixel under `similarity` minus `smoothness` is
   removed, one over `similarity` plus `smoothness` kept, part transparent between. At 0 the edge
   is a staircase; the default 0.08 suits most footage, about 0.15 a soft or out-of-focus edge. Well
   under `similarity`, or the band reaches into the subject and the person goes milky. There is no
   choke: a fringe that survives is spill.
4. **`spillSuppression`**, the screen's light bounced onto the subject, a green rim on shoulders and
   hair. It clamps the key's channel to the average of the other two on every pixel, so pushed high
   it drains green from a plant, a logo, a shirt. The default 0.25 covers ordinary bounce, about 0.5
   someone who stood close to the screen; raised until the rim stops reading and no further.

Where the light drifts across a shot, `similarity` and `smoothness` take keyframes
(`fx<n>.similarity`); two or three keys following the lighting do what one value cannot. A screen
lit unevenly across the frame takes two or three `colorkey` entries on the clip, each a narrow
`similarity` on the `keyColor` measured in its own region (the bright side, the shadowed side).

**What is not screen** (a light stand, the screen's edge, the floor below the sweep): a drawn
rectangle that no effect names, over the part of the frame that is screen and subject, with the key
unconfined, so the stand outside is not drawn at all (the usual answer, no second draw); or the key
confined to a drawn mask by `maskId`, so everything outside arrives exactly as shot, which costs a
second draw and needs the region to cover all of the screen, or the green it misses lands in the
composite. A drawn mask does not move, so it is loose enough for every frame, or the clip is trimmed
to the stretch where one shape holds.

**Shots a key cannot save**, each a shot to turn down rather than a setting to find: a screen so
uneven or wrinkled that stacked keys still overlap the subject; motion blur, which smears screen and
skin into pixels that are neither; fine hair, smoke, glass, anything half-transparent; a foreground
that shares the screen's color, a reflection included; heavy compression, which smears the boundary
before the key sees it. A key on an adjustment clip reaches every clip below and ignores the layer's
opacity and fades, so the key goes on the clip.

The keyed clip goes on a track above whatever plays behind it, overlapping in time; with nothing
underneath the frame's own black shows through, and `check_timeline` reports a background that
ends under the clip over it: a shorter background is placed again after itself (addClip, a loop
where its last frame meets its first) or slowed to length (setSpeed under 1). What plays behind is the user's shot, footage from
their library (`search_footage`), stock video (`search_stock` kind video, then `import_media` url),
or a generated background, in credits. A grade matching the subject's light to the new
background is what makes the composite read, on the background as often as on the person.

### When not to

- Fast motion, a hand crossing the face, fine hair against a busy background: the edge crawls.
- A corner, a lower third or a spotlight is a drawn ellipse or rectangle, which never crawls; a moving
  region is only for one that has to follow a person.
- The room without the person: a cutout leaves a hole where they were, and nothing here fills it.

### Checking an edge

A cutout that separated nothing still completes and plays, and the timeline reads as though it worked.
`check_timeline` reads the structure in one call and does not see an edge. An edge is judged by
its look alone, so frames are drawn here, in one `inspect_timeline` call: the first and last
frame, a frame with their hands up, the frame where the subject moves fastest. On them: the person
with footage-shaped edges, not straight ones, the title behind interrupted by their head, no rim or
color fringe round the hair. A halo busy footage hides shows against a flat field: a light and then
a dark `solid` clip under the cut-out for a frame each, where the user asked for the edge to be
verified or nobody sees the shot before it is used.

### Two worked plans

A word behind a presenter:

1. get_timeline {timeline_id} on the timeline the work is on, or manage_timelines {create: {name, aspect_ratio, clips: [{ref: the background}]}} from items
2. inspect_timeline {timeline_id, seconds: [the beat's middle]}: where the head sits, for the word's side and size
3. edit_timeline {timeline_id, if_revision: from step 1, operations: [addTrack above, addClip text on it spanning the beat, addTrack above that, copy of the shot on it at the same trim by addClip with the same ref, source and span]} (or a separate `copy_clips` call)
4. edit_timeline {timeline_id, if_revision: from step 3, operations: [updateClip {clipId: the copy's id from step 3, patch: {masks: [{id: "person", shape: "person"}]}}]}; get_jobs {ids: its job_ids} until the mask is read
5. check_timeline {timeline_id}, then inspect_timeline {timeline_id, seconds: the beat's first, middle and last frame}: one call, the edge and the word judged by their look
6. publish_preview {timeline_id}: returns the preview the video already has, its app_url the link to hand over

A presenter keyed over their own screen recording:

1. get_timeline {timeline_id} on the timeline the work is on, or manage_timelines {create: {name, aspect_ratio, clips: [{ref: the background}]}} from items
2. inspect_color {ref: the presenter's item, at_seconds, points: [the middle of the screen]} → the screen's hex; then inspect_color {ref, at_seconds, key_color: that hex} → key.suggested {similarity, smoothness}
3. edit_timeline {timeline_id, if_revision: from step 1, operations: [addTrack {track: {kind: "visual", index: above the recording, id: "presenter"}}, addClip {trackId: "presenter", ref: the presenter, id: "presenter-clip"}, updateClip {clipId: "presenter-clip", patch: {effects: [{type: "colorkey", params: {keyColor, similarity, smoothness: from step 2}}]}}]}: the id given on addClip names the clip for the patch in the same call
4. check_timeline {timeline_id}, then inspect_timeline {timeline_id, seconds: the first frame, the last, the fastest movement}: one call, the edge judged by its look
5. Only where step 4 shows a rim, a hole or a stand: edit_timeline {timeline_id, if_revision: from step 3, operations: [updateClip with the whole colorkey entry again (effects replace whole), spillSuppression raised or a second colorkey entry for the screen's darker side, and a drawn rect mask where the frame holds a stand or an edge]}
6. publish_preview {timeline_id}: returns the preview the video already has, its app_url the link to hand over; render_timeline {timeline_id, idempotency_key} for a file used elsewhere

## What cannot be built here

- Liquid, heat-haze and warped dissolves need per-pixel distortion that neither the timeline nor a graphic draws. A near one: `wave` or `fisheye` on a short adjustment clip, or a `blur` transition.
- Motion tracking (text or a graphic pinned to a moving object) has no tracker. Place the graphic where the object holds still, or follow a person with a `person` mask instead (through the desktop app, an `object` mask follows a thing).
- Datamosh and kaleidoscope have no equivalent; `mirror` reflects one half onto the other.
- Motion blur on moves is not drawn. Keep fast moves short, and use `zoomblur` or `blur` on the frames where it matters.

Name what cannot be built and offer the near one.

## The check

An effect is judged by its look, so draw frames: one before it starts, one in the middle, one after it ends, in one `inspect_timeline` call. On them:
- Masks follow the person's edges with no straight lines and no halo.
- Copies line up: no double outline where a masked copy sits over its shot.
- No black edge in a zoom, a pull-back or a 3D tilt.
- Text behind a person stays readable.

`check_timeline` reads the structure: gaps, a background ending under a clip, sound.

## Handover

- The video with its effects, in the app with the result's app_url as the link; `render_timeline` for a file.
- With it: which effects went where, and that timeline effects can be changed in the editor.
- Also possible: another effect at another moment, a different transition style, the same effect on a vertical version.
