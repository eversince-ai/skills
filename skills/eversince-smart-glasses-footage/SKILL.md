---
name: eversince-smart-glasses-footage
description: "Clips from camera glasses into a short first-person video: what to keep and cut, the 3:4 to 9:16 crop, sound and captions."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-smart-glasses-footage"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A clip from camera glasses is what the wearer looked at, for as long as they looked, often with
their hands at work. Nothing was framed for the cut, and each clip starts and stops with a tap at the
temple or a spoken command. The viewer becomes the wearer. A brief, a reference or a brand from the
user outranks every default here.

## The plan: a 60-second vertical video from a day's clips

1. Bring the clips in: `import_media` {mode: "local", paths, new_project: "Glasses", folder: "<day>"} through the desktop app, or {mode: "link", project, folder} for a page opened on the phone; `search_library` {project, folder, type: "video"} lists them with size and length
2. Log the batch: `inspect_media` {refs: up to twelve, mode: "overview"}, then {ref, mode: "window"} where it matters; `transcribe_media` where the wearer talks
3. `manage_timelines` {create: {name, aspect_ratio: "9:16", clips: [{ref, source: [start + 1, end - 1]} per select, in story order]}} → the timeline
4. `auto_reframe` {timeline_id, subject: "any"}; `edit_timeline` updateClip {objectFit: "contain"} on a wide that should play whole
5. `clean_audio` {ref, mode: "denoise", timeline_id, clip_id} on a windy clip
6. Where the user wants them: music (`duck_music` {timeline_id, ref}), a place card (a text clip), a sped-up walk (setSpeed), captions (`captions` generate)
7. `level_audio` {timeline_id, platform}, then `check_timeline` {timeline_id, platform}
8. `publish_preview` {timeline_id}: the link to hand over; `render_timeline` for a file

## The medium

- The camera sits at the wearer's eye, so a head turn is a pan the viewer did not choose.
- Portrait, 3 wide by 4 high, on an ultra-wide lens (about 100°, 122° on Oakley Meta Vanguard). Up to 3K at 30 frames a second or 1080p at 60 on current models; 1080p on the first Ray-Ban Meta. A clip's real size and length are in `search_library`'s `technical`; a 16:9 clip in the batch is from a phone and is cut like phone footage.
- Clips are short: 30 seconds by default, up to 3 minutes; on Vanguard up to 3 minutes at 3K and 5 at 1080p, a hyperlapse up to 30 minutes and slow motion up to 1. A day is many short clips.
- Stabilization, slow motion and hyperlapse are set on the glasses. The timeline cannot steady a shaky clip, so the cut goes around shaky stretches.
- The microphones sit near the ears: the wearer's voice close and clear, the world around it, wind where there was wind. Exposure is automatic: daylight is good, low light is grainy, a bright sky burns out and a backlit subject goes dark.
- Clips reach the phone through the Meta AI app, as HEVC on iPhone and MP4 on Android. Clips copied to a computer are imported where they sit (`import_media` `local` through the desktop app), with a cloud copy unless `cloud_copy` is false; from a phone they are shared into the Eversince app or dropped on an `import_media` `link` page.

## First-person grammar

- **Hold on what the wearer looked at.** That is the subject. Cutting away from it breaks the one thing this footage has.
- **The hands carry it.** Stay on the hands while they work, and cut when they finish, not when the shot looks nice.
- **Cut after a head turn, not during it.**
- **Say where we are first.** Open on a held wide of the place (the kitchen, the street, the trail), then go to the hands.
- **The reaction is the hold.** There is no shot of the wearer's face; holding a beat a second longer carries the reaction.
- **Keep the world audible.** The ambient sound is what makes it feel like being there. Music sits under it.
- **Short and dense.** Two to five seconds a shot, longer while the hands do something worth watching. 30 to 90 seconds a video. Past about ninety seconds of one point of view, a viewer wants to see the person. You can mix in phone clips of the wearer or the place where they have them.

## The log

Look at every clip before choosing. A clip that looks dull in a list is often where the hands finish the job.

- `inspect_media` `overview` on up to twelve clips a call shows the place, the hands, and a smudged lens or a hair across it. `window` looks closer at a stretch that matters.
- `transcribe_media` with `refs` and `format: "states"` says which clips have words; read a talking clip with `format: "phrases"`, whose pauses are cut points.
- `search_footage` with the `project` and `folder` finds one moment across the whole batch, after the clips are indexed.
- `inspect_media` with `cuts: true` on a long clip shows where the picture jumps, which on glasses is usually a head turn. The steady stretches between are the usable ones.

Cut around a smeared or very dark stretch unless that moment is the point.

## Selects and shape

Order clips by the story, not by the time of day. Three common shapes:

- **Mini vlog:** arrive (the held wide), do (two or three moments of the hands), result (the thing, held).
- **A cook or a make:** the parts laid out, the moments where the thing changes, the finished thing turned in the hands.
- **A walk or a visit:** the entrance, one detail up close, the thing bought, eaten or found, a hyperlapse between places if there is one.

Trim the first and last second of every clip: that is the wearer reaching for the temple or turning toward the subject. Use `addClip` with a `source` span that starts a second in and ends a second early, or `trimClip` both edges. A clip that swings in the middle becomes two selects.

## The frame

A 3:4 clip in a 9:16 video loses its sides; three quarters of the width shows. Hands low in frame survive, and a wide of a place loses its edges.

- `auto_reframe` with `subject: "any"` follows a person where there is one, and otherwise the main thing in shot: the hands and what they hold.
- Where the whole width matters (a room, a view), play the clip whole as a band over black (`updateClip` `objectFit: "contain"`), or over a blurred copy of itself (`auto_reframe` with `clip_id` and `background: "blur"`) if the user prefers.
- For 1:1 or 16:9 the crop takes the top and bottom, where the hands are, so play the clips as a band instead.
- `setSpeed` can speed up a walk or a wait. A hyperlapse from the glasses is already sped up. Keep slow motion for one gesture, and cut fast head movement rather than slowing it.

## Sound

- The wearer's voice is the dialogue and the world is the background.
- `clean_audio` `denoise` (cloud processing) on a windy clip, with `timeline_id` and `clip_id` so the clean item takes its slot, before cutting on its words.
- Clips from one day arrive at different levels; `level_audio` with the platform evens them once the cut is final.
- Music where the user wants it, low under the world: `duck_music` places, levels and ducks it in one call.
- Silent clips (`has_audio` false in `get_items`) can take ambience (`search_stock` kind ambience), music, or nothing, as the user likes.
- A voiceover recorded later, for a place too loud to talk in, goes on its own track and ducks the world rather than replacing it. Mixing is in `skill-sound-design`.

## Words on screen

- Captions where the wearer speaks: `captions` generate with `style.position` low, so no line covers the hands in the middle of the frame.
- A place card on the first frame, where the user wants one: two to four words (the place, the time, the dish) as a text clip, at least 4% of the frame's height (about 77 on 1080 by 1920), white with a shadow or stroke.
- Keep text off the moment the hands finish.

A day's clips shot in sun and shade can be matched into one look with `skill-color-grading`.

## The check

One `check_timeline` with the platform reads the cut; `ok: true` is the pass, and each warning is still read and fixed or accepted. Where the wearer speaks, read the words with `get_transcript`. Draw frames only where a finding names a clip, the cut has no speech, the user asked for it to be checked, or nobody sees it before it is used: `inspect_timeline` with `sheet: true`.

- Someone who was not there knows where this is in the first second and what happened by the end.
- No frame mid head turn, and no clip opening on a hand at the temple.
- The hands are on screen while anything is being done, and the cut waits for them to finish.
- Nothing that matters is cropped out by the vertical frame.
- Captions sit below the hands, and any place card reads on the first frame.

A last pass over the whole video is in `skill-finish`.

## Handover

- The video at 9:16 unless another shape is named, the world audible under any music, captions below the hands where the user wants them.
- With it: the check's result and whether frames were drawn.
- In the app, with the result's app_url as the link the user opens; `render_timeline` makes a file for use elsewhere.
- Also possible: a mini vlog from several days, a voiceover, one look across the day's clips, a square or wide version, music or ambience, a place card, a sped-up walk.
