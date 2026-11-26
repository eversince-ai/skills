---
name: eversince-product-videos
description: "The film where the product is the hero and the picture carries it: reveal, detail run, demonstration, few words or none."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-product-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Desire comes from specifics: the weight implied by how the object moves, the material caught by
light, the detail that shows care. A film should not have to say "premium"; a chamfered edge
catching light says it. A voice arguing the value beat by beat is `skill-launch-videos`; here the
object makes the argument, with few words or none. A brief, reference or brand from the user
outranks every default here.

Keep the product true. Real footage does this by nature; a generated shot does it only when built on
the product's real images. A beautiful shot of a slightly different product is worthless. Keep time
true too: a demonstration that closes the gap between a tap and its result claims a speed the
product does not have, so hold a step that takes time or skip it plainly.

## The plan

1. inspect_media {refs: the photos and clips, mode: "overview"}: the strong takes marked against the shot list
2. The user's track; else generate_audio {kind: "music"} where the user has credits, or a stock track the user picked (search_stock {kind: "music"}, import_media {mode: "url"}); with none, no music and the cuts follow the picture; detect_beats {ref: the track's item}
3. manage_timelines {create: {name, aspect_ratio: the destination's, clips: [{ref: each shot, source: its span} in shot order]}}
4. edit_timeline {timeline_id: from step 3, operations: [addTrack {track: {kind: "audio", index: 0, id: "music"}}, addClip {trackId: "music", ref: the track}, moveClip and trimClip so each shot lands on its beat frame, cameraMove drift or slow-push on each still, updateClip volume 0 on clips whose sound fights the track]}
5. Only where the user asked for sound effects: search_stock {kind: "sfx", query: the click} and import_media {mode: "url"}, edit_timeline addClip on its frame … per effect
6. check_timeline {timeline_id: from step 3}: the one check
7. inspect_timeline {timeline_id: from step 3, sheet: true}: the one picture
8. publish_preview {timeline_id: from step 3}: returns the preview the film already has, its app_url the link to hand over

Generated shots, where a move a still cannot make is wanted (credits, priced first): generate_image {prompt: the shot's defining frame and the light and world block, reference_refs: [the product's images]} per shot, judged as a still, then generate_video {type: "image-to-video", source_refs: [that frame], end_ref where the landing matters, prompt: the one move}, each placed in step 3 among the clips and graded onto the footage.

## What the job needs

- The material: product footage, photography, the best images from every angle. Cut from footage,
  generate, or both; real hero shots with generated world and detail shots around them is a common
  mix. Use whichever source serves each shot.
- A digital product's real images are its screens. Use screenshots as references and render them
  inside devices. Their motion is a recording or a composition on the timeline, not a generation;
  rebuild a part of a screen that has to move as its own `compose_motion` graphic over the still.
  Check every screen for private data.
- The brand's look, where one exists.
- The destination (a launch page autoplaying silent, a keynote moment with sound, a social cut). It
  decides length, ratio and whether sound leads.
- The one thing this film says about the product, named before any shot exists.
- With no script, reference or shots given, describe three different films in words before building
  anything, each with its own world, device and form (the result first, one object traveling across
  apps, the feature happening to a recognizable image; one continuous take, each shot ending on the
  thing that opens the next, match cuts). Make the one a stranger would remember, and carry one real
  thing from its first shot to its last.

## Structure

One argument leads; any others serve it.

- **The reveal**: fragments, silhouettes and details build to the full view, held at the end. For
  launches, where the object is the news.
- **The detail run**: macro to macro, material, mechanism, edge, finish. When craft is the argument.
- **The demonstration**: the product doing its thing, feature by feature. When what it does is more
  remarkable than how it looks.
- **The world**: the product in the environment that says who it is for. For lifestyle positioning.
- **The assembly**: the product building itself from its parts, or coming apart to show what is
  inside. For engineering stories.

Whatever the structure, build toward the fullest view and end held on the product; the last frame
is the one that lingers. On a store page or in an ad, proof beats polish: the product doing its job
answers a buyer's questions faster than a beautiful shot of it standing still.

## The shots

Write the shot list before making anything: per shot, what is in frame, how the camera moves, how
long, and whether it comes from footage or generation. Each shot earns its place through the one
thing. Use the fewest shots the structure needs.

Keep the vocabulary small and strong: orbits, push-ins, light sweeping a surface, a rack to a
detail, the object turning against darkness or in its world. Take tempo from the product's energy,
not a house style: heavy and slow reads premium, fast and hard reads kinetic; choose from the brand.
One intention per shot: an orbit that also pushes is one gesture; an orbit that pushes, racks and
whips puts the camera ahead of the product. Keep one light and one world across the film unless the
structure needs a change; the light binds the shots into one film.

How commercial product shots are made, worth naming in a generated shot's prompt: a macro or probe
lens gliding along a surface with very shallow focus, for texture and mechanism; the product turning
on a turntable for the full 360; a soft key light with a rim light behind, which outlines the edges
and lets glass and metal show reflections without glare; a slow, repeatable move on a slider or a
motion-control arm rather than a handheld one.

Look at existing material before choosing: `inspect_media` overview for the whole item, window into
what moves, frames as pictures. Mark strong takes against the shot list. A missing shot can be
stock, a still with a camera move, or a generated shot (credits); generate only the shots that need
it, not the whole film.

## Stills before motion

Move a real photo on the timeline before generating anything. `cameraMove` (drift, slow-push,
rise, reveal) on an image clip needs no generation, carries no fidelity risk, and covers most of a
film made from six photos. Generate only the moves a still cannot make: an orbit, a rack to a
detail, the product turning. Fit photos and clips of different shapes into one frame with updateClip
objectFit "contain" over a ground, or a crop through transform, both free. Extending a still to the
film's ratio is `reframe_image`, a generation in credits; offer it where a crop loses the product.

Decide generated shots as stills, where a change costs one image. Generate each shot's defining
frame first (`generate_image`) with the product's real images in `reference_refs` and the
product kept as shown, and get it right (fidelity, light, composition) before animating. For a shot
whose landing matters, the reveal or the last shot, decide both ends, the start frame and the frame
it lands on, on a model that takes `end_ref`. Then `generate_video` from the chosen frame. Shots
from footage skip this: their frames exist, and generated shots are graded to match them, not the
other way round.

Where the real images show the product from one side only, a product sheet keeps it consistent
across the film: the product from the front, three-quarter and side on a seamless neutral gray, soft
even light, no props. Make it from the real photo as the reference, check it against that photo,
then use it as the reference in every shot. Name the label's words, the logo and the exact colors in
the prompt, in one line, as things that hold, since a model can drop small text and logos it was
shown; leave the shape to the reference.

The model's own guidance in `list_models` decides reference modes, multi-shot rules and durations.
The corrections that hold across models:

| The default | Why it fails | Instead |
|---|---|---|
| Describing the product's shape, materials or label | The reference carries it; describing invites a redraw | The real images as references, the product kept as shown; on video the chosen frame carries it |
| A new world per shot | Light and environment drift; the film reads as a mood board | Light and world fixed in one reusable prompt block, the same references across shots |
| Every shot generating its own audio | Per-shot ambience fights the track | Generate silent where the model has the switch; otherwise the clip's volume at 0 |
| Text inside a generated video shot | Lettering in motion is unreliable and cannot be revised | Names, specs and claims typeset in the edit |
| One long take per scene | Some models hold a multi-shot sequence, others drift | The model's guidance shows where its seams are; cut where it is weak |

## Sound

A product video is usually music-led. Settle the track before assembly (the user's own, then
`generate_audio` music, which runs as a job, where the user has credits, `search_stock` music only where the user picked it) and cut the video to it:
cuts on the beats, the reveal on the drop or the quiet after it. `detect_beats` gives every beat
and downbeat in source seconds; reading that grid and finding a brought track's strongest start are
in `skill-sound-design`. Where the user asks for sound effects, they sell physicality (the click,
the snap, the hum) placed on their exact frames: `search_stock` kind sfx first, `generate_audio`
sound_effect when none fits and the user wants one made. Voiceover only when the destination
expects it.

## Assembly

`manage_timelines` create with the destination's `aspect_ratio`. Put the track first on its own
lane, then the shots against it, each on its beat frame; grade generated shots onto the footage with
updateClip grade (`skill-color-grading`). Cut details fast; hold the reveal. Use `captions`
generate only where the destination needs spec lines; most product films carry no text.

## Delivery

Review the film in the app (`publish_preview`), with comments on it. For people outside the
workspace, a review page is `manage_public_links` create with the rendered film, opened without an
account; comments on it arrive as threads on the link (`get_comments` with `link_id`). A cut
going to a finishing house leaves through `export_project` (Resolve, Premiere, Final Cut), written
by the desktop app; color and effects do not carry over.

## The check

- The fidelity pass: compare every generated shot's product with the real images at full size, at
  the frame where the product reads largest (`inspect_media` frame, then the reference beside it):
  proportions, materials, label.
- One `check_timeline`: gaps, missing media, a shot too short to see, text off the frame or too
  small, the levels. It does not read the look, and a product film is judged by its look alone, so
  draw one picture: `inspect_timeline` with `sheet: true`. Look at single frames either side of
  a cut only where a move carries across it, a finding names a clip, or the user asked for the work
  to be verified. Above all, the last frame.
- Sound off: the object still reads as desirable from light and detail alone.
- After one viewing, a stranger can say what the product is and the one thing the film claimed. "It
  looked expensive" means the film said nothing.

A last pass over the whole film is in `skill-finish`.

## Handover

- The film at its destination's ratio and length; one for silent autoplay reads with the sound off.
- Where it is cut to a track: the reveal on a downbeat, and the track ending on its landing, not at the file's edge.
- It is in the app from its first media and plays there; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: generated shots for a move a still cannot make, a social cut at other shapes, a silent loop for a page, sound effects on the product's own actions, spec lines typeset over it, a version cut to a different track.
