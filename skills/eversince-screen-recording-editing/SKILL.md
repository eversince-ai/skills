---
name: eversince-screen-recording-editing
description: "A screen recording into a demo someone can follow: dead time cut, zooms on the actions, each step named, the interface kept true."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-screen-recording-editing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A screen recording holds the work and the waiting in equal measure, and the viewer came for the work. The demo is the same session at the speed of understanding: every wait, wander and false start gone, a zoom where the action is too small to follow, each step named by a voice or a label, every result held long enough to land. The interface itself is the claim, so its pixels stay as recorded: framed, cut, zoomed and pointed at, never restyled. A narrated film about a product is `skill-launch-videos`; a piece that explains an idea is `skill-explainer-videos`. A brief, a reference or a style from the user outranks every default here.

## The plan

1. Read the recording into beats: each action, when it starts, when its result is on screen
2. Cut the dead time and speed the waits that matter
3. Zoom on the actions that are too small to follow, starting and returning wide
4. Name each step: the voice, captions, or a short label
5. Frame it (a background and rounded corners, where wanted) and make the vertical version if asked
6. Check readability and private data, then hand over

## What the job needs

- The recording as a library item. A zoom enlarges what was recorded, and text softens first: 2x on a 1080p capture is effectively 540p, so past about 1.5x it wants a 4K or Retina capture.
- One workflow, end to end. Five features in a row is a tour, and a tour reads as a slideshow.
- Where it plays and how long: a homepage demo 60 to 90 seconds, a paid social cut 15 to 30, a help article 2 to 4 minutes with each step under about 40 seconds, a sales follow-up 3 to 5 minutes. A raw ten-minute session usually holds about a minute of demo.
- Voice or none. A narrated demo is paced by the voice, a silent one by the actions.

## Reading the recording

You have the pixels, not the cursor's clicks, so find the actions from what changes on screen: a field filling, a menu opening, a page changing, a modal appearing. Start with `inspect_media` `overview` for the session's shape and `window` to grid the stretches that matter. Where someone narrates, `transcribe_media` with `ref` gives the words the cut follows. `inspect_media` with a `prompt` is cloud processing and can wait in a queue, so ask one only for what the frames and the transcript cannot answer, kept narrow (when each action starts and its result shows, when the screen waits, loads or idles); it returns the seconds.

Write the beats down before cutting: the action, the frame it starts, the frame its result is on screen, and the area of the screen where it happens. A beat whose result cannot be named is not a beat yet.

## Cutting dead time

`remove_silence` cuts pauses in speech and knows nothing about a spinner. Dead time on a screen is visual: a page loading, a progress bar, the mouse wandering, a menu opened and abandoned, a typo typed and deleted. Cut it with `edit_timeline` rippleDeleteRange, which closes the gap on every track so the voice and picture stay together, working from the last span to the first since each cut moves every frame after it.

- **A wait that is part of the claim stays, faster.** Split it and speed it (`setSpeed`, 4 to 8 for a progress bar) to under two seconds, and mark it (a small "4x" label) so it does not suggest a speed the product does not have.
- **The rest can play a little faster.** Cutting the dead parts and playing the rest at 1.25 to 1.5x is common; a narrated stretch tops out around 2x, and needing more means there is still dead time to cut. Keep the voice's pitch.
- **Typing with no story in it goes faster or goes.** The one line that causes the result (a command, a prompt, a search) is typed on screen at a speed that can be read.
- **Late in, early out.** Each beat starts as the action begins and ends once its result is readable. Leave about a quarter to half a second after a click before cutting, and hold a result at least a second and a half, longer when it has to be read.

## Zooms

Zooming on the action is the look viewers now expect from screen demos, and done badly it is the most common complaint about them. The craft:

- **Start wide** so the viewer gets their bearings, and **return wide** between steps that move around the screen.
- **Zoom on what is too small to follow:** a click on a small control, typing in a field, a value that changes, a line of text that has to be read. Stay wide on a dashboard where every panel matters, on a spreadsheet or timeline that spans the screen, and on anything already full-frame.
- **How far:** about 1.5x for emphasis, up to 2x where text has to be read, no further than the capture's resolution allows.
- **How:** a glide in with `setKeyframes` on `scale` and `position` with "smooth" easing, about 0.35 to 0.5 seconds, aimed at the element, not the screen's center. A cut-in to the region (`splitClip` at the beat's edges, then `auto_reframe` with `view` {x, y, width}) is the alternative where a glide would cross too much. `auto_zoom` is for talking footage and lands beside the action on a screen, so do not use it here.
- **Aiming the glide:** for a full-frame clip, to put the point at (px, py) (fractions of the frame, read off an `inspect_media` frame with `grid: true`) in the center at scale s, set position x = (0.5 - px) × s and y = (0.5 - py) × s. Keep each within ±(s - 1) / 2, or the frame's edge shows; near an edge, the point lands off center instead. Draw one frame of the first zoom (`inspect_timeline` at its hold) to confirm it landed, then reuse the numbers.
- **How long:** hold about a second on a quick result (a button changes, a modal opens), one and a half to two seconds on text, never under about 0.6 seconds or it reads as a glitch. End the zoom when the action ends, and at the end of a sentence where there is a voice.
- **How often:** the recording plays whole unless a moment needs a zoom; a minute of demo usually needs two to four, and a dense stretch at most one every three to four seconds. Merge quick clicks in one area into one zoom, and pan from one zoom to the next nearby one instead of going out and back in.
- **Once zoomed, hold still.** A slow drift across a beat pulls the eye off the interface.

## The interface stays true

- **Not graded.** Interface and brand colors are what is being shown; only a screen filmed with a camera is corrected for the cast the camera added.
- **Not re-rendered.** A screen passed through a video model comes back as a picture of an interface. The recording is the file.
- **Not invented.** Never add a result the product did not produce or suggest a capability it does not have. Where something has no real screen, a drawn stand-in that moves the way the feature behaves (a graphic, never a pretend product page) or a terminal for a command.
- **Private data does not ship.** Read every beat's frames for names, emails, customer records, keys and tokens, and cover what shows with a pixelate or blur effect confined to a rect mask (updateClip effects with maskId), for as long as it is visible. A mask does not move, so where the window scrolls, split the clip and cover each piece.

## Words, cursor and sound

- **A narrated demo** carries its words in the voice, at about 130 to 150 words a minute with a short pause after each major action. Captions for a feed or where asked (`skill-animated-captions`): Lit box reads over a light or busy interface; over a dark one, Social, or Lit box with a light `backgroundColor` and dark `color`.
- **A silent demo** names each step with a label: a text clip over the beat, six words or fewer, sentence case, one position and one look throughout, on screen two to three seconds. The default white text fails over a light interface: over a light screen, the label takes dark `color` or a box (`backgroundColor` and `backgroundOpacity` on the text clip).
- **Click marks and callouts** where the user wants them: a ring on the click point or a short line naming what happened (`skill-motion-graphics`), placed from a frame with its grid, never by eye. One callout per beat at most.
- **Music,** where asked, low and even with no vocal: `duck_music` with `ref` (`skill-sound-design`).

## Framing

The look of current demo tools: the recording scaled down a little (`transform` scale about 0.88 to 0.94) with rounded corners (`transform` radius about 8 to 12 at 1080) over a quiet, mid-tone or desaturated background (a solid clip on a track below). A bright or busy background competes with the interface. A face camera, where there is one, sits in a corner at about a fifth of the frame's width and moves when it would cover a control.

## The vertical version

A desktop squeezed whole into 9:16 is unreadable. Open on the result with a short line of text at the top, zoom harder than in 16:9, and crop to the area of the action for most beats; the whole screen plays as a band (updateClip objectFit "contain") only where the viewer needs the map. A face, where there is one, stacked with the screen holds attention. Keep text and the action out of the bottom fifth and the right edge, under the platform's buttons. Build it as its own timeline (`skill-content-repurposing`), since the zoom plan of one shape does not serve the other.

## The check

One `check_timeline` reads the cut: no findings means it passed. It does not read the interface's own text, so readability and private data are judged on the picture. `inspect_timeline` with `sheet: true` gives the whole piece; `sheet` and `tiles` space frames evenly and cannot be aimed at beats, so look at specific moments with `inspect_timeline` `seconds`, and at single frames where a finding names a clip.

- **Readable where it matters:** the words a beat depends on read at the size it is watched.
- **Bearings:** it starts wide and returns wide between moves; no zoom on a stray click.
- **Nothing dead:** no stretch where the screen waits or wanders without a reason.
- **Cause and effect:** every result follows the action that caused it, on screen.
- **True:** colors as recorded, nothing invented, every sped-up wait marked.
- **Private:** no name, address, record or key visible anywhere.

A last pass over the whole piece is `skill-finish`.

## Handover

- The demo with its dead time out, zooms on the actions that need them, each step named, nothing private on any frame, colors as recorded, the voice at delivery level. In the app from its first media; the result's app_url is the link, and `render_timeline` makes the file.
- You can also make: a vertical cut, a GIF of one step (`render_timeline` format gif), a framed version with a background, click marks and callouts, a voiceover, a thumbnail, music.

## An example plan: a demo from one screen recording

1. inspect_media {ref, mode: "overview"}, then {ref, mode: "window", window: {start, end}} over the stretches that matter; {ref, prompt: when each action starts, when its result shows} only for what the frames and transcript leave open
2. manage_timelines {create: {name, aspect_ratio, clips: [{ref}]}}
3. transcribe_media {ref} where there is narration
4. edit_timeline {timeline_id, if_revision, operations: [rippleDeleteRange per dead stretch, last first; splitClip and setSpeed per wait that stays]}
5. The zooms: edit_timeline {timeline_id, if_revision, operations: [setKeyframes on scale and position per zoomed beat: wide, in over about 0.4 s, hold, out]}
6. Words: captions {action: "generate", timeline_id, template: "Lit box"} for a narrated feed cut, or edit_timeline addClip {trackId, clip: {id, type: "text", content: the step, from_seconds: the beat's start, duration_seconds: its length}} per beat, dark text or a box over a light screen
7. duck_music {timeline_id, ref} only where asked; level_audio {timeline_id}
8. check_timeline {timeline_id}; inspect_timeline {timeline_id, sheet: true}, then {timeline_id, seconds: the beats to read}
9. publish_preview {timeline_id} → the link; render_timeline {timeline_id, idempotency_key} for a file
