---
name: eversince-vlog-editing
description: "A day or trip cut into one story in the order it happened: a short day in my life, a long YouTube vlog, a travel or silent vlog."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-vlog-editing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A vlog is a day, a trip or a stretch of life cut so the viewer lives it in the order it happened. The person filming is the through-line and the scenes are the content. It is neither a montage of B-roll with music nor a talking head with cutaways. A brief, a reference or a style from the user outranks every default here.

## The plan

1. The user's style, where they have one or show a reference; then log everything: the to-camera takes, the scenes, the details and the in-between shots, with what happens in each
2. Pick the shape (below) and the one question the vlog carries: will the move happen, how does the launch go, what is this city like
3. Build the cold open last, from the strongest moments; the day in order after it
4. Cut each part at its shape's pace, with voice, text and music as the shape uses them
5. Sound to delivery level, stamps or captions where they belong, one look across every camera
6. Check, and hand over

## Logging

See everything before choosing anything. `transcribe_media` with `refs` and `format: "lines"` returns the words of every item that has them; an item with none is read with `ref` (over five minutes, a job). Action-led clips are read with `inspect_media` `overview`, then `window` into anything alive; the overview also shows footage that already carries text, and a `prompt` read (credits) hears music already in a clip. Log a scene as a scene: where, who, what happens, the one line that says why it matters, how it ends. Log the to-camera takes apart, since they are the narration, and mark any line that states the day's promise for the cold open. `search_footage` over the day's `project` finds a spoken moment across every item.

## The user's own style first

If the user has made vlogs before, or points to one they like, that is the style: their pace, how they open, how they talk to camera, their music, their text and their look. Read one frame by frame before cutting (`inspect_media` window over its opening and a scene, its words with `transcribe_media`), match it, and use the defaults below only where it says nothing (`skill-remake-from-a-reference` holds the method). A user who has posted vlogs but not said so can be asked for a link. With nothing to go on, the defaults below give a good first cut.

## What most vlogs share

These are the defaults when the user has no style of their own.

- **Chronology is the spine.** The day happened in an order and the viewer lives it in that order; a scene out of place reads as staged.
- **One question carried through,** where the day has one: open it early and answer it before the end.
- **The cold open** is the strongest or most unusual moment of the day, then the day in order. Waking up and making coffee is a weak opening unless it is the point.
- **The ending** is the last real moment. A sign-off the user recorded stays; a closing line that only summarizes can go.
- **Keep what really happened,** including a setback or a mistake, unless the user wants it out. Nothing is staged or reordered to look better.

## The shapes

**A short vertical day in my life** (TikTok, Reels, Shorts; most run 45 to 90 seconds, up to three minutes allowed)
- Open on the day's most unusual frame and its premise in the first second; a flash of the whole day in a fraction of a second is one way.
- Clips of about 1.5 to 4 seconds, the day in order, faster through the middle and slower at night.
- Voice in one of three ways: text on screen only, a voiceover recorded afterward (often written first, with the edit built to it), or short takes to camera. Captions on any speech (`skill-animated-captions`).
- 9:16 (`auto_reframe` on horizontal clips), ending on the payoff of the premise.

**A long YouTube vlog** (a day, a week, a project; often 10 to 40 minutes)
- A cold open of the best moments in fast order, about 30 to 40 cuts in the first two minutes, with a line to camera that says what today is.
- Then the scenes, held. Scenes run two to five minutes and are not retention-edited: no `remove_silence` over a scene, since the pauses are part of it. Shorten a scene by dropping whole passages between two lines that join (`get_transcript` on the cut, then rippleDeleteRange), never by tightening every gap. Inside a scene, cut to a listener's reaction or a detail where the words allow.
- Between scenes, a passage of thirty to ninety seconds with no speech (a drive, a walk, a detail) carries the time that passed; that is where music lives. A short time-lapse (under five seconds) works as a chapter break.
- In the opening stretch, change something visual every ten to fifteen seconds: an angle, a punch-in, a detail, a line of text.

**A travel vlog** (often 5 to 10 minutes)
- Arrival and first impressions, then discovery, then a resolution; organized by day or by place, a day condensed to a few minutes.
- Fast cuts for energy against lingering or slowed shots for feeling.
- The place's own sound (streets, rain, a market) under and between the talk.
- Location names on screen as they change; an animated map or route where the user wants one (a `compose_motion` graphic, or the shelf's place and date graphics through `get_templates` {query: "place and date"}).

**A cinematic or silent vlog**
- A reflective voiceover, or none: in a silent vlog the sound carries it (footsteps, cooking, rain, the room), with sparse text. It is not a talking vlog muted.
- An unhurried pace, held shots, one consistent look, few graphics.

## Craft that makes it feel edited

- **Moving between places:** a wide, a medium and a detail in under four seconds (the street, the door, the hand on the handle) gets the viewer there faster than one long shot.
- **Sound leads the picture:** start the next place's sound a beat to two seconds before its picture (`detachAudio`, `unlinkClips`, then `trimClip` on the sound's edge), so the cut feels like going somewhere.
- **On the beat:** in a passage with music, `detect_beats` on the track gives its beats; land the cuts of the passage on them. Talk is never cut to the beat.
- **A punch-in on the funny or important line:** `auto_zoom` with `clip_id` and `moments_seconds` on that line, not a rhythm of punches everywhere.
- **Speed:** a short time-lapse or a speed ramp through a walk or a drive (`setSpeed`; a ramp is `splitClip` into parts and one `setSpeed` each).
- **B-roll by what is in it:** `search_footage` over the day's project finds the shot a line mentions (the coffee, the street, the screen) when the log did not tag it.
- **Silence as a beat:** cutting the music out for a second lands a moment harder than any effect.

## Voice, music and sound

- **The scene's own sound** is the dialogue and the room. Clean a phone-mic recording where the user asks (`clean_audio`, cloud processing).
- **Music** where the user wants it, under the cold open, the passages and the close, dipped under talk: `duck_music` with `ref` (the track, in the manual's order: the user's own first, stock only where the user picked it) places and ducks it in one call, measuring the track first (cloud processing); where it cannot be measured its level is kept with a warning, so check that `check_timeline` does not flag the music as loud, and lower the music clip's volume if it does. Change the track when the story shifts rather than looping one song under everything. A scene that should stand on its room alone has the music split out of it (splitClip and removeClip on the music).
- **Rights:** a business account on TikTok can only use the commercial library, and that license does not carry to Reels or YouTube. Music that is not cleared on a long YouTube vlog sends its revenue to the rights holder. Use music the user can use, or leave room for a sound added in the platform's app.
- `level_audio` with the platform once the cut is final (`skill-sound-design` for the mix).

## Words and stamps on screen

- **A vertical vlog** is captioned over its speech (`skill-animated-captions`), since feeds play muted.
- **A long vlog** usually is not captioned over its scenes; a subtitle file goes out separately (`captions` export, format srt, after `transcribe_media` with `timeline_id`).
- **A day or time stamp** on the first exterior, and a card naming each scene (the place, the time) where the vlog uses them; the shelf's `place-dateline` graphic is a ready version (the place at about 4% of the frame's height, the date or time beneath). Cards enter and leave on a cut and hold still; a stamp or place line at 4% of the frame's height or more, in a color clearly apart from what is behind it.

You can also build one channel's signature devices where the user wants that style or shows a reference: a to-camera take as an inset on a white matte with its sentence beneath, a single word popped on screen in the cold open, a scene card with a still of the room, a split panel with an agenda beside the footage, a stat card with the number that matters. Build them as clips on `edit_timeline` (solids, scaled clips, text clips timed from `get_transcript`), in the user's fonts and colors where they have them.

## The look

Footage from a phone and a camera, or from different light, has to read as one film. Correct each clip's white balance and exposure before any look, or the look makes the mismatch worse; then one grade over the whole cut on an adjustment track (`skill-color-grading`). Leave the footage as shot unless the user wants a look or the clips visibly do not match.

## The check

- One `check_timeline` with the platform: `ok` true is the pass, and each warning in `issues` is still read. It does not read text burned into the footage or whether a scene is the right one.
- **The day test** (`get_timeline`): scenes in the day's order, any stamp on the first exterior only.
- **The room test** (`get_transcript`): no scene cut shorter than the thing that happened in it.
- **The open test:** every moment in the cold open recurs later in the day; a promise the day never delivers comes out of the open.
- **The question test:** the question opened in the first minute is answered before the end.
- Frames when a finding names a clip, the look alone is the judgement (a card, a matte, a silent vlog's shots), the user asked for it to be verified, or nobody sees it before it is used: `inspect_timeline` with `sheet: true`. A last pass over the whole piece is `skill-finish`.

## Handover

- The vlog at its shape: 16:9 for a channel, 9:16 for a feed; scenes in the day's order; music dipped under talk; speech at delivery level. In the app from its first media; the result's app_url is the link, and `render_timeline` makes the file.
- You can also make: a vertical teaser cut from the cold open, chapters at each scene, a subtitle file, a look over the whole day, the channel devices above.

## An example plan: a long vlog for YouTube

1. search_library {project: "<the day's project>", type: "video"} for the items; transcribe_media {refs, format: "lines"}; inspect_media {ref, mode: "overview"} on the action-led items
2. The log: each scene, each to-camera take, the details by the passage they can carry; the question the day carries
3. manage_timelines {create: {name: "<the day> · vlog", aspect_ratio: "16:9", clips: per scene in the day's order, the to-camera bridge then the scene, each {ref, source: [start, end]}, a passage between scenes the same way}}
4. get_transcript {timeline_id} and edit_timeline rippleDeleteRange on whole passages a scene loses
5. The cold open: edit_timeline insertClip per moment at the start, the to-camera line first, each moment two to four seconds from the scenes' own spans
6. Stamps and cards where the vlog uses them (text clips, or the shelf's place and date graphics with compose_motion)
7. duck_music {timeline_id, ref} where the user wants music; level_audio {timeline_id, platform: "youtube"}
8. check_timeline {timeline_id, platform: "youtube"}
9. publish_preview {timeline_id} → the link; render_timeline {timeline_id, idempotency_key} for a file

## An example plan: a short day in my life

1. transcribe_media and inspect_media over the day's clips, as above; the premise and the day's most unusual frame chosen
2. manage_timelines {create: {name: "<the day> · short", aspect_ratio: "9:16", clips: the opening frame, then the day in order, clips of 1.5 to 4 seconds, each {ref, source: [start, end]}}}
3. auto_reframe {timeline_id}
4. A voiceover where the user wants one (recorded, or generated per `skill-voiceovers-and-music`), or text on screen
5. captions {action: "generate", timeline_id, template: "Social"} over any speech
6. duck_music {timeline_id, ref} with music the account can use; level_audio {timeline_id, platform: "tiktok" or "instagram"}
7. check_timeline {timeline_id}; publish_preview {timeline_id} → the link
