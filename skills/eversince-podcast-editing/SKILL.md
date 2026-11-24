---
name: eversince-podcast-editing
description: "A podcast episode, video or audio: the cut follows whoever talks, pauses and fillers trimmed, sound leveled, chapters, an audio master."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-podcast-editing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

In a conversation the sound leads and the picture follows: who is on screen is decided by who is talking, and every cut, trim and wide shot serves the exchange. This skill edits a podcast episode, video or audio, from the recordings to the files the user publishes. Clips lifted from it are `skill-clipping`.

## The plan

1. Line up every recording (cameras and microphones) on one timeline
2. Cut the picture to whoever is talking, following how it was recorded (below)
3. The content edit, if the user wants it: tangents out, a cold open, intro and outro
4. Tighten: shorten the long pauses and remove the fillers that break a sentence, keeping the conversation natural
5. Clean and level the sound
6. Chapters, then the video episode and, where wanted, an audio master

## How it was recorded

- **Several cameras** (a studio): `sync_recordings` with every camera and microphone item lines them up by sound, then `switch_angle` with `on_speech` cuts the whole conversation in one call (below).
- **One camera with two people in frame:** there is nothing to switch between. `speakers` on the recording first, then `auto_reframe` with `follow: "speaker"` crops to whoever is talking, and the wide frame is your safety shot for crosstalk.
- **Remote, one file per person** (Riverside, Zoom, SquadCast): sync the items, then choose a layout. Side by side (`apply_layout`, a tile per person) suits a fast back and forth; one person full frame suits long answers. Most editors mix the two.
- **Audio only:** no picture to protect, so cuts can be tighter, since there are no jump cuts to hide.

What the room gave decides how the cut can be made:

- **A camera and a close microphone each** (a lav, a boom, a USB mic per person): each item mostly hears its own person, and the cut is made from that.
- **Cameras only, across a room:** every item hears everybody, so the cut rests on which microphone is loudest, and a weakly matched item drifts.
- **One shared microphone and several cameras:** nothing says which face a voice belongs to, so give the cut by hand as stretches, or stay on one angle.

## Cutting several cameras

1. **Line them up.** `sync_recordings` with every item of the take, cameras and separate microphone items alike (two to twelve items, video or audio), places each on its own track and remembers the group; the microphone items are what `on_speech` listens to. The response says how surely each was matched. Re-sync a weak match before cutting anything, since a drifting angle means rebuilding the cut. A vertical angle on a 16:9 timeline can lose the face; check its framing on a frame.
2. **One sound.** The microphone tracks carry the whole piece, one per person where each had a lav. Sync sets each camera clip's volume to 0 when a microphone item is in the group (a cameras-only take keeps the camera sound), so a camera switch never changes what is heard. The wide is whichever angle holds everyone; read it off one `inspect_media` frame per item when the labels do not say. A take with no wide leaves `wide` off and the cut rests on the last talker.
3. **Words on the cut.** Clips carry their items' words and keep them after `switch_angle`. `transcribe_media` with `refs` and `format: "states"` says which items have words; with `timeline_id` it reads only the clips without them; when every item already has words, nothing is charged and each clip comes back in `transcribed` with `cached: true`. A recording nobody has listened to is left out of the cut.
4. **Cut the conversation.** `switch_angle` with `on_speech` follows the talker, holds through a laugh or a "yeah", holds its floor so a run of short answers does not strobe, and rests on the wide (named by label) when the room goes quiet. `min_shot_seconds` is 2.5 by default; use 4 for a considered interview, 2 for a fast exchange. Where every angle carries the same sound (one board feed under each camera), `on_speech` refuses: run `speakers` on one recording, map each speaker to an angle, and pass the turns as `switches`.
5. **The seams.** Check the joins around interruptions and laughter: `get_transcript` with `speakers: true` names who is talking on each row from what `speakers` found, and leaves out items it has not picked anyone out of; where it returns no speakers, read the turns with `speakers` {ref, transcript: true} on each recording. `inspect_timeline` with `span` shows a join against the sound. When people talk over each other, the wide or a two-shot shows both. A listener's reaction worth holding (a laugh, a wince, a nod) is a stretch you add by hand with `switch_angle` `switches` after the automatic pass.

What makes it watchable:

- **A shot breathes:** two and a half seconds is the floor, four to six normal for a considered answer. A cut on every sentence reads as a machine.
- **Cut on the turn, not the word:** change shot when the exchange changes, a question ending, an answer beginning. Cut inside a sentence only when the reaction is the moment.
- **The wide is punctuation:** it opens, closes and covers the pauses. A long answer on the wide is dead air with two people in it.
- **Cut between framings that differ** (wide to close), not between two similar ones.

## The content edit

The user's request may be only "take out the silences and fillers". A content edit is more: cutting tangents and dead stretches, moving a section, a cold open. Ask or offer it; do not assume it. Do it before tightening, since there is no point polishing a stretch you are about to cut.

- **A cold open:** the strongest moment from later in the episode, played before the intro as its promise. Keep it short; a long branded intro or guest bio before the conversation starts loses YouTube viewers.
- **Intro and outro:** `edit_timeline` insertClip at the top pushes the cut right on every track; addClip puts the outro after the last clip. An intro or outro built once is its own timeline nested in every episode as one clip (addClip with `timeline_id`), so a change to it reaches every episode.
- **Following what was said:** never join words from different moments into a sentence the person did not say, and keep the qualifiers and the "not"s a meaning rests on.

## Tightening

The goal is a conversation that moves, not one with every breath taken out. Removing every pause and filler sounds robotic.

- **Pauses:** `remove_silence` shortens long gaps rather than deleting them. Cut pauses past about half a second to a second, and keep a beat; `keep` with a [start, end] stretch leaves a pause before a payoff or a laugh as it is. A video episode keeps more air than a clip; audio only can run tighter.
- **Fillers:** `get_transcript` words, then `remove_words` with `matches` for the fillers this person says. Remove the long ones and the ones that break a sentence; leave the short ones that sit inside the flow. Read the words around each cut so nothing needed went with it. Run `remove_silence` first, then the words, since every cut shifts the word indices.
- **Laughs and crosstalk stay.** They are the chemistry of the show.
- **Jump cuts:** a cut inside one person's shot makes their head jump. On several cameras, `switch_angle` can put the listener or the wide across it; on one camera, `auto_zoom` holds alternate pieces closer to hide it, or it stays as a jump cut.

## Sound

- **Clean:** `clean_audio` where the user says the room is noisy (cloud processing), before any cut. Use the lightest touch that works: noise reduction pushed hard sounds hollow and watery, and a little room tone is better.
- **Joins:** a hard cut in speech can click. Every join gets two or three frames of fade each side (`setKeyframes` on `volume`), and a gap is filled with the room's own sound, never digital silence.
- **Level:** `level_audio` brings each speaker to the same level and the whole episode to its target in one call. `platform: "podcast"` targets -16 LUFS (what Apple asks for, with peaks under -1 dB); `youtube` targets -14. One master at -16 plays fine on YouTube, about 2 dB quieter; if the user wants both, level the video episode for YouTube and render the audio master from a version leveled for podcast.
- **Music:** `duck_music` where a bed runs under the intro or outro (`skill-sound-design`).

## Chapters and ad breaks

- **Chapters** at the turns of subject, not of speaker: `edit_timeline` addMarker kind chapter. YouTube needs the first at 0:00, at least three, each at least ten seconds long. `check_timeline` does not check these rules; check the chapter markers against them yourself. `get_timeline` returns them, which is what the timestamp list in the description is written from.
- **Ad breaks**, if the show runs sponsors: at a natural pause, never mid-explanation, often around the middle of the episode. Mark them (addMarker kind note) so an ad can be inserted later, with no music running across the spot.

An episode headed for a grade or mix in another app leaves through `export_project` (Resolve, Premiere, Final Cut), written by the desktop app; color and effects do not carry over.

## The check

- One `check_timeline`: no findings means it passed. It does not read text burned into the footage, or whether the shot is on the right face.
- `get_timeline` beside `get_transcript` with `speakers: true`: every shot change matches a change in who is talking, none shorter than the floor, no angle on screen while its own person is silent for long unless it is the wide, and picture and sound cut together wherever the conversation was trimmed.
- `get_transcript` read around the cuts: no sentence joined into something not said, no word clipped.
- Frames in four cases: a finding names a clip, the look alone is the judgement, the user asked for the work to be verified, or nobody sees the episode before it is used: `inspect_timeline` with `sheet: true`.

A last pass over the whole piece is `skill-finish`.

## Handover

- The video episode at the source's shape (16:9 for YouTube and Spotify, which asks you not to add black bars yourself), the microphones playing through every camera switch, speech at delivery level, every shot change on a change of speaker. In the app from its first media; the result's app_url is the link the user opens, and `render_timeline` makes the file.
- The audio master, where the user publishes to podcast apps: `render_timeline` with `format: "mp3"` or `"wav"`, stereo, leveled for podcast. Lines like "as you can see" may need a cut there.
- The chapter list, ready to paste.
- You can also make: vertical clips of the best moments (`skill-clipping`), a thumbnail per episode (`skill-making-thumbnails`), captions, a music bed, show notes from the transcript, a project file for Premiere, Resolve or Final Cut.

## An example plan: a multicam episode

1. manage_timelines {create: {name, aspect_ratio: the cameras' shape, usually "16:9"}}
2. sync_recordings {timeline_id, refs: every camera and microphone item of the take}; a long take answers that it is still listening, and the call after returns the offsets
3. transcribe_media {timeline_id}: reads only the clips with no words yet
4. switch_angle {timeline_id, on_speech: {wide: the wide's label, min_shot_seconds: 2.5, listen: true}}; where it refuses (the angles carry one sound), speakers {ref: one of them} then switch_angle {timeline_id, switches: [{from_seconds, to_seconds, angle}]}
5. The content edit, where the user wants it: edit_timeline {timeline_id, operations: [removeClip, moveClip, insertClip the cold open and intro at 0, addClip the outro]}
6. remove_silence {timeline_id}, then get_transcript {timeline_id, granularity: "words"} and remove_words {timeline_id, matches}
7. edit_timeline {timeline_id, operations: [addMarker kind chapter at each turn of subject]}
8. level_audio {timeline_id, platform: "youtube" or "podcast"}
9. check_timeline {timeline_id}
10. publish_preview {timeline_id} → the link; render_timeline {timeline_id, idempotency_key}, and with format "mp3" for the audio master
