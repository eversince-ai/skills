---
name: eversince-make-videos
description: "Editing recordings: what is possible (cutting, sound, captions, framing, look), checking in proportion, and the skills by footage."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-make-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill covers editing recordings: a talking head, a podcast, a screen recording, phone clips, an interview filmed on several cameras, a recording whose sound needs fixing.

The edit the user asked for is the job. Nothing below is a checklist: a step the request does not need is left out. What the agent already knows about the user and what they expect decides how far to go. Anything that would change the video in a way the user did not ask for (a new aspect ratio, a color grade, music, graphics) is offered in one line, not done.

## Getting files and sharing the result

A file the user means but did not attach is found with `search_library` (newest first). A file elsewhere comes in with `import_media`: an upload link the user opens on a computer or phone, a public web address, or a path on the user's computer through the desktop app. A ProRes or MKV file plays after `convert_media`.

The result is in the app from its first media, where it plays without rendering, and every result carries its app_url, the link the user opens; `render_timeline` makes a file for use elsewhere. A change asked later is made on the same timeline (`get_workspace_overview` recent_work); another cut of it is a variation (`manage_timelines` create with parent).

## Checking

`check_timeline` is free and reports what a render would show wrong. `inspect_timeline` draws real frames of the edit (cloud processing, except through the desktop app), worth it where a change is visual and its result uncertain: a reframe, a layout, text placement. A simple cut or a sound fix does not need frames drawn.

## What is possible

A menu, used when the request calls for it:

- **Cutting**: by the words (`remove_words`, `remove_silence`, with words from `transcribe_media`), by hand (`edit_timeline`: trim, split, move, speed), a new timeline from clips (`manage_timelines` create); one recording into many clips (`cut_clips`), clips or a look taken from another timeline (`copy_clips`, `copy_clip_settings`), several cameras of one event (`sync_recordings`, `switch_angle`).
- **Sound**: speech cleaned (`clean_audio`) and levelled (`level_audio`), music in the manual's order (the user's own, then generated to fit with `generate_audio`, stock only where the user picked it, else none), ducked under a voice (`duck_music`).
- **Captions**: `captions` (`skill-animated-captions`).
- **Another aspect ratio** with the subject kept in frame: `auto_reframe`.
- **Reading footage**: words with timing (`transcribe_media`), frames or a log of every shot (`inspect_media`), who speaks when (`speakers`), the beat of a song (`detect_beats`), several files at once (`prepare_media`), a moment across many recordings (`search_footage`).
- **Framing**: split screen, picture in picture or grid (`apply_layout`), zoom on emphasis (`auto_zoom`), a person cut out from the background (`remove_background`); named effects such as text behind a person, a shatter or a 3D screen: `skill-video-effects`.
- **Look and sound**: color (`skill-color-grading`), titles and graphics (`compose_motion`, `skill-motion-graphics`), sound design (`skill-sound-design`), higher resolution (`upscale_media`).
- **Handing off**: `export_project` gives the edit to DaVinci Resolve, Premiere Pro or Final Cut Pro.
- **Every timeline operation's fields**: `skill-timeline-operations`.

## Skills for common kinds of footage

Starting points when the request matches; some edits need none of them.

- **Raw footage into a finished video**: `skill-video-editing`; one person talking to camera is its part `skill-talking-head`.
- **A launch, a feature or a product shown off** from screens, photos or clips: `skill-launch-videos`, `skill-product-videos`; a piece made mostly of graphics and type: `skill-motion-graphics`.
- **A long recording into short clips**, or one highlight: `skill-clipping`; a supercut of one subject across the library: `skill-searching-your-backlog`.
- **A podcast or any conversation**, on one camera, several, or a file per person: `skill-podcast-editing`.
- **A screen recording into a demo**: `skill-screen-recording-editing`.
- **A day of clips as a vlog**: `skill-vlog-editing`; camera-glasses clips: `skill-smart-glasses-footage`.
- **A finished video in other sizes**: `skill-content-repurposing`; in another language: `skill-translation-and-dubbing`.
- **Changing the footage itself** (a new angle, light, location, outfit or object, the performance kept): `skill-transform-video`.
- **A Reel, Short or TikTok built around one idea**: `skill-short-form-videos`.
- **Footage, photos or type cut to a song**, a performance included: `skill-music-videos`.
- **One idea explained with a voice and pictures**: `skill-explainer-videos`; a channel of them with nobody on camera: `skill-faceless-videos`.
- **A voiceover or music made for the piece**: `skill-voiceovers-and-music`.
- **Generated video that looks like film**: `skill-ai-filmmaking`; drawn, painted or rendered worlds: `skill-ai-animation`; a person or character kept the same in every scene: `skill-ai-influencer`.
- **Matching a reference video**: `skill-remake-from-a-reference`; cutting the way the user already edits, read from their own videos: `skill-edit-in-my-style`; a final pass before it goes out: `skill-finish`.

## Handover

- The edit in the app (`publish_preview`).
- Also possible: captions, music, other sizes, clips from it, a thumbnail, the edit handed to another editing app.
