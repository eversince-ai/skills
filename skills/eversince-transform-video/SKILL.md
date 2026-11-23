---
name: eversince-transform-video
description: "Real footage changed by a video model, the performance kept 1:1: a new angle, light, location, outfit or object, one change a pass."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-transform-video"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Real footage changed by a video model: the same take with another camera angle, other light, another location, a different outfit, an object added, replaced or removed. The performance (face, lips, hands, timing) is the user's own and stays 1:1 with the source, which is what makes the result read as filmed rather than generated, and what lets the edit cut back to the real take on any frame.

The request decides the passes: one change asked for is one generation. The method below holds for one pass or many.

## The plan

1. Read the take: `inspect_media` {ref, mode: "overview"} for its length, the room, the person and the moments that carry it, with their seconds
2. Cut it to the stretch that changes, 4 to 30 seconds, since a pass changes the whole clip it is given
3. Decide the change on a still first, where a try costs one image: `extract_media` {ref, mode: "frame", at_time} and `generate_image` {type: "image-to-image", source_refs: [that frame], prompt: the change, the person kept exactly as shown}
4. The pass: `generate_video` {type: "video-to-video", model, source_refs: [the take], reference_refs: [the still from step 3 and any reference picture], prompt, settings: {omniTaskType: "edit"} on Seedance}; `estimate_cost` first; `get_jobs` returns the result
5. Check it against the source at the same seconds (below), then place it beside the source on the timeline

## The source

The result is only as good as the take it starts from. A take that suits this: one continuous shot, the whole person in frame, steady (locked-off or slow), even light, clean sound. Wide framing gives the model the most to re-photograph; a tight close-up leaves little room for a new angle. A shot the user already has is used as it is; one being filmed for this is worth those notes.

`inspect_media` reads the source: its length, frame rate, resolution, the lens feel, the room, the person, and the moments that carry the take (a gesture, a look, a laugh) with their seconds. Those numbers and seconds are what a prompt holds the take with.

## The models

Two models take a source video and an instruction: Seedance 2.5 (`model` "seedance-2.5") and Kling 4.0; a call without `model` runs on the user's default.

- **Seedance 2.5**: `generate_video` with type video-to-video, the source in `source_refs`, and `omniTaskType: "edit"` in settings. The source runs at least 4 seconds and under 30; the result keeps its aspect ratio and its length (rounded up to a whole second), neither of which can be set. Reference images (a location, an outfit, a prop) go in `reference_refs` and are cited as @Image1, @Image2 in the order given; the source is @Video1. 720p is the safe delivery format: 1080p returns 10-bit H.265, which some players do not open. 480p costs a little under half of 720p and is available as a draft. A take of 30 seconds or more is cut into pieces under 30 seconds (`cut_clips`, or trims on a timeline), each sent on its own, none under 4 seconds (a short end joins the piece before it). A camera that moves somewhere the source never went, with the take as the performance only, is ingredients-to-video: the take in `reference_refs` as @Video1, with the duration and ratio set by the call.
- **Kling 4.0**: video-to-video edits the source by instruction, cited as @Video1, with reference images as @Image1 and on. The source runs at least 3 seconds and under 30, and is at least 700 px on each side; the result runs the source's length; up to 4 reference videos ride with it as @Video2 and on, 30 seconds together with the source; sound is generated on every result. Video input is charged on its own length as well as the result's.

`list_models` has each model's current limits and `estimate_cost` the price of a pass before it runs.

## Changes per pass

One change per pass (the angle, or the light, or the location, or the outfit, or a prop) holds the performance best. Several changes in one prompt compete, and the model trades the performance away to satisfy them. Several changes can also run as a chain, each pass taking the previous result as its source, at one generation each; a result that drifted carries its drift into the next pass.

What a transform holds and what it does not: it keeps the take's action, its length and its pacing. A different action or performance, another duration or structure, a replacement of very different size or shape, a hand now touching something that was never there, or several connected things at once (camera, light, motion and objects together) are a new generation from frames, not a transform.

## The two kinds of change

- **Adding to the plate** (an object, an effect, a creature, a prop): the whole plate is kept and the addition is described as behaviour over time: where it starts, how it spreads or moves, what light it throws and onto what, the contact shadow it casts. Scale is stated against something in frame ("as tall as the doorway"), or it comes out life-size. A person in the take carries on as filmed unless the change is about them.
- **Replacing the world around the person** (location, weather, time of day): the person, what they hold or sit in, the camera and its move are kept; the new world moves past with the parallax the original motion implies. A daylight world keeps the face most reliably; night and neon force the light on the face to change, so the face is held harder in the prompt.

## Light: keep it or relight it

Decided before the prompt. Keeping the subject's light and lighting only what is new is the lowest risk to the face. Relighting the whole frame under one look changes the face's light too, so identity, expression and wardrobe are held explicitly while only light and grade change. Either way, matching colour alone leaves a pasted look; the new and the kept share the same key direction (screen-left or screen-right, named), the same softness and shadow density, the same colour temperature in Kelvin, the world's bounce on the subject, the same haze, depth of field and grain, and no hard edges or halos.

## The prompt

A long, exact prompt holds the performance where a short one invites the model to re-act it. The structure, in this order, using only the blocks the change needs:

1. **The edit, in one line**: "Edit @Video1: the same take, [the one change]. Everything else stays exactly as in @Video1." The verbs edit, add, remove, replace and change say it is an edit.
2. **The source**, described as measured: length, frames and frame rate, resolution, the lens (wide, normal or long, how high, which way it looks), the room in foreground, midground and background, the person's visible features. Left and right are named from the camera, or from the person, and said which.
3. **The change**, in what can be seen and measured, not in mood words. A camera move as direction, height, distance, aim and timing ("from the front it curves to his left at desk height, about 1 m from him, passes behind him and settles on his right profile at 7.0 s, then holds still"). Light as source, direction and colour temperature in Kelvin. A location from an image as what to take from it.
4. **Each reference**: what to take from it and what not to. "From @Image1 take only the window wall: the white blinds, the plant, the light through it. Not the desk, not the person." An image without that line is copied whole, person included.
5. **The performance lock**, the most important block: the source drives the person frame by frame for its whole length. Hands, fingers, head, face, lips and posture are in the same place at the same second as in the source; second N of the result is second N of the source; nothing added, removed, smoothed, re-timed, mirrored or swapped between hands; the same face, not beautified. A camera move changes only the angle the same motion is seen from.
6. **The key moments**, second by second: each gesture that carries the take, written with its seconds and which hand, so the model reproduces it rather than inventing a similar one. The take's whole length can be written as a track (0.0 to 0.3 s, both hands up at chest height; 0.4 s, they lower), as dense as the take is busy.
7. **What to hold**: a few short lines restating the critical facts once (the person, the room, the length, the timing), in positive form, the face and identity line last.

Everything is written as what should happen ("feet stay planted"), not as what should not; measures replace adjectives (km/h, metres, percent of haze, Kelvin); a design that is added is generic, not a brand's or a character's. A real texture the model keeps faking (an animal's fur, a material) gets a reference photo of the real thing, cited as appearance only, its background and light ignored.

A timed camera move is anchored twice, by the words and by the second ("on the line 'and that is why', at about 6.2 s, a slow push-in"), so it lands if the model's timing drifts; the source's speech is kept in the prompt for this, and lips that must match a line quote it exactly. A move that ends where the original framing was demands a 100% match of it (angle, headroom, horizon, lens), or it lands on a near miss that no longer cuts against the take. A beat added before the take pushes the take later inside a fixed length: the time added comes off its end, said and settled before the pass runs.

An example, for its level of detail rather than its content (a take of a woman explaining a recipe at a kitchen counter, its light moved from midday to evening):

"Edit @Video1: the same take, the light changed from midday to early evening. Everything else stays exactly as in @Video1.
THE ORIGINAL: 12.0 s, 300 frames, 25 fps, 1920×1080, one static shot at chest height, about 35 mm, centred. Maya, short dark hair, round glasses, a green linen apron over a white T-shirt, stands behind a pale wood counter talking to camera. Foreground: a bowl of lemons, camera left. Behind her: open white shelves with jars, a window camera right.
THE CHANGE: the window light becomes low early-evening sun from camera right, 3200K, warm, raking across the counter; long soft shadows fall to camera left; a pendant lamp above the counter, already in frame, glows at 2700K; the room beyond the window darkens to dusk blue. Her face keeps a soft warm key from camera right.
RULE 0: @Video1 drives Maya frame by frame for all 300 frames: hands, fingers, head, face and lips in the same place at the same second, in sync with the original speech. Nothing added, smoothed or re-timed. The same face, not retouched.
KEY MOMENT: 6.4 to 7.6 s, her right hand lifts a lemon from the bowl, turns it once toward the camera and puts it back; the same hand, the same turn, the same timing.
HOLD: Maya, the counter, the lemons and the shelves exactly as in @Video1; 12.0 s; the camera does not move."

Writing the prompt from a few lines of intent is a job for the agent itself: the change described briefly, then expanded into the blocks above with the source's real numbers. Iterating changes only the named thing and keeps the rest of the prompt word for word; rewriting the whole prompt loses what already worked.

When the look of the change is uncertain, the first frame transformed as an image (`generate_image` image-to-image from a still of the take, `extract_media`) settles it for a fraction of a video's price before the pass runs.

## Checking

`inspect_media` on the result at the key moments, against the same seconds of the source: are the hands, the lips and the gesture where they were, is the face the same person. A new line of the prompt naming the drift in positive form ("both hands stay exactly where they are in @Video1 at 5.1 s, the right index finger at the temple") is what a further pass carries. The same failure across several attempts means the source has nothing for the model to hold (the gesture is not in it, the angle reveals what no frame shows) and more attempts only spend; a still of the location as a first frame (image-to-video) with the action written from scratch is the other route, or a reference image of what the new angle should reveal.

## Putting it together

Where results are cut with the source, they sit on a timeline beside it (`edit_timeline`, the source on the track below at the same seconds). A pass run on a timeline clip (`generate_video` with `timeline_id` and `clip_id`) replaces that clip when ready, so the source that stays is a copy of it on its own track, or the pass runs from the library item rather than the clip. The real take is the anchor: the edit cuts between the original and the transformed versions on the same frame, since the performance matches, and back to the original wherever a result drifted. The sound stays the source's; a transformed pass's own sound is muted unless the change was meant to make new sound.
