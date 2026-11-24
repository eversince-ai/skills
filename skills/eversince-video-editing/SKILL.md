---
name: eversince-video-editing
description: "Raw footage into a finished video, following what is said: read all, the story, a loose cut, passes, levels and captions. Clips: clipping."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-video-editing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill turns raw recordings into a finished video. The story is in the material. If there is a script or plan, it is the spine and your work is choosing between takes. If there is none (an interview, an event, a shoot that went its own way), you find the through-line in what was said and captured. Work in the order editors do: see everything before choosing anything, build the story from the words, assemble loose, tighten in passes over the whole piece, and finish last. Polishing a scene before the structure is settled means doing it twice. One person talking to camera has its own part, `skill-talking-head`. Separate clips from a long recording are `skill-clipping`; several cameras on one conversation are `skill-podcast-editing`. A brief, reference or style from the user outranks every default here.

## The plan

1. Read everything: the words of every item, and the footage where the picture carries it
2. Choose the story in the words before placing anything: the opening, the order that builds, the ending
3. Assemble the selects on one timeline in story order, cut loose
4. Make passes over the whole piece: structure, then rhythm, then the trim
5. Finish: sound to delivery level, captions where it plays on a feed, music and color only where wanted or needed
6. Check, then hand the video over

## What you need

- The recordings as library items. If the user means a recording they did not attach, find it with `search_library`; bring one from elsewhere in with `import_media`.
- What the video is for, who it is for, where it plays (a player or site at the source's shape, a feed at 9:16), and its length. The length is a budget every select is charged against.
- Any moments the user already knows matter, as a starting point.

## Reading everything

Choose nothing until you have seen everything.

- **Speech-led material:** read it as words. `transcribe_media` with `refs` and `format: "lines"` gives one timed line per sentence, about 100,000 characters a page per item for up to three items; for the next page, pass `window` {start: next_start, end: the file's length}. Read an item that has never been read with `ref` (over five minutes, it is a job). Run `speakers` on a recording with more than one voice so the words carry who said them.
- **Action-led material:** read it as frames. `inspect_media` overview, then window into anything alive.
- **Many hours of footage:** `search_footage` with the theme narrows which stretches to read. You still choose by reading.

As you mark each moment, note the job it does (open, reveal, prove, compare, raise, relieve, bridge, humanize, close). A moment with no job is what the structure pass cuts. Mark the unusable too. Transcripts get names, products, numbers and speech under music wrong, so check any line a cut depends on against the picture. Brand kit words with `heard_as` correct later reads.

## The story

Pick the strongest through-line the material supports, in the words, before placing anything. Editors call this the paper edit: the film written as an ordered list of passages. Test it as a radio cut: read or heard with eyes closed, does the story work? If it does not work in words, pictures will not save it.

- **The opening** is the moment that earns attention, from anywhere in the material. The best intro does not feel like an intro: it delivers what the title promised as early as it can (Paddy Galloway).
- **The build** is the order where each part raises the next. Chronology is the strongest spine when the material has a real journey through time, and the weakest when it only records the order things happened. Question and answer, tension and escalation are the alternatives.
- **The ending:** choose it before assembling so the middle does not wander, and end on the landing.

Emotion leads every choice. Walter Murch ranks it above story and rhythm combined, so a cut that keeps the feeling and the meaning beats one that is only smooth.

## Following what is said

Cutting speech is quoting, and the meaning stays the speaker's. The standards documentary and news editors work to (PBS, NPR):

- Condensing is fine when it fairly represents what was said and its context.
- Do not build a sentence's subject, verb and object from different moments.
- Do not put a question before a different answer, or make separate recordings look like one.
- Keep any qualifier the meaning rests on (in our test, for this customer, at the time).
- A seamless join (which `remove_words` makes) makes these easy to break by accident. Where a jump is real, it can show, as a cut or a change of framing.

## The assembly

Put the selects on one timeline in story order, cut generously: into the setup and past the landing. Trimming later is cheap; finding a lost frame again is not. `manage_timelines` create with `clips`, each {ref, source} as you recorded it in the read, builds it in one call; add a later select with one `edit_timeline` addClip. Selects carry their items' words onto the cut, which `get_transcript`, `remove_silence` and `remove_words` read. A marker (addMarker kind note) keeps a note on the cut itself.

## The passes

One concern per pass, over the whole piece each time.

1. **Structure.** Does the order work? Does anything sag? Does each part still earn its place against the length? Move or cut whole sections. The hardest call is cutting good material that does not serve this piece.
2. **Rhythm.** Pace is how much the viewer is asked to take in, not how often you cut. Go faster where the subject is understood, the beat is emotional, the action repeats or music carries it. Go slower where there is text or an interface to read, a reaction or reveal has to land, or the talk is dense. Slower pacing that lets a person's character show can hold viewers better than constant cutting.
3. **The trim.** Mechanical, after the judgement passes, and as far as the piece wants: a user who asked for the pauses or the fillers kept gets them kept.
   - **Pauses:** `remove_silence` cuts every pause longer than `min_silence_ms` and keeps `speech_padding_ms` on either side. About 800 with 150 sounds natural; about 600 with 120 is the pace of a posted talking head; much below 400 takes the breaths, and a piece with no breaths or pauses tires the ear. Keep the pause before a payoff and the laugh after a line.
   - **Fillers:** read the words with `get_transcript`, then `remove_words` with `matches` for the ones this person says. Removing all of them sounds robotic: in conversation, keep fillers that show thinking or soften a claim; in a voiceover or formal piece, cut them. Read `get_transcript` again between calls, since every cut moves the words.
   - **Restarts:** keep the take that finishes its thought without a stumble, choosing energy over polish when they disagree. Cut the rest as [start, end] spans.
   - **Cut points** go in the quiet between words, at the end of a phrase where the voice falls, never inside a word or on a rising line.
   - **Jump cuts** on a locked camera are part of the look of edited talk, and can stay. If the user wants them hidden, `auto_zoom` holds alternate pieces closer, or a cutaway covers the join.
   - **Into a new speaker or scene,** a J-cut or L-cut softens the join: `detachAudio` on the clip, `unlinkClips`, then `trimClip` on the sound's edge a beat past the picture cut. Generate captions before these cuts: generated after, they can cover only the detached sound and leave stretches uncaptioned, so check that the caption track covers the whole cut. Nothing flags the overlap where both sounds play; check each one on frames (`inspect_timeline` with `span`).
   - **Every join in speech** gets two or three frames of sound fade on each side so nothing clicks: `setKeyframes` on `volume`, 0 at the clip's first and last frame and 1 two frames in.

## Finishing

- **Sound.** Run `clean_audio` first if the room is noisy (cloud processing), then `level_audio` once the cut is final. It brings every recording to the platform's delivery level and makes takes from different days sound like one sitting. Online video plays back at about -14 LUFS with peaks under -1 dB; delivered quieter, it just plays quiet.
- **Captions** go on a feed cut unless the user says no, since feeds often play muted: `captions` generate, Social for a feed, Subtitles for a long player piece (`skill-animated-captions`). On a long video for a player, captions are the user's choice.
- **Music** only if the user wants it: `duck_music` with `ref` (the user's track, or a `search_stock` id the user picked) places it, levels it and ducks it under the voice in one call (`skill-sound-design`).
- **Color** only where shots visibly do not match or exposure and white balance are off. Correct before applying any look (`skill-color-grading`).
- **Titles and graphics** where the user asks (`skill-motion-graphics`).

## Moves on a clip

Add a punch-in, a push across a still or a framing that follows where the user asks, once the structure holds. Use keyframes on position, scale, crop or focus (`setKeyframes`), or a named move (cameraMove, listed by `get_templates` kind move; fields in `skill-timeline-operations`).

- Set the poses first, including the last: where the frame starts, where it must be readable, where it ends. The final pose holds; it springs back only when the return is the point.
- A key is a stop. "smooth" eases into and out of every key, so a move routed through a middle key rests there; "linear" runs through it. A push across a still is two linear keys across the clip, already moving when the cut arrives.
- Keep a face, or the thing a line names, framed long enough to be seen.
- One change at a time on one clip: a crop track and a scale track over the same frames fight.

## The check

Nothing here watches the cut, so split the reads:

- **What would render wrong:** one `check_timeline`. No findings means it passes.
- **The words alone:** read `get_transcript` as the radio cut. The story holds, the first line earns attention, no sentence was joined into something not said, no word is clipped at a cut, and the last line is the landing with nothing after it.
- **The shape:** `get_timeline`. It fits its length, opens on the strongest moment, and has no stretch of minutes without a change.
- **The sag test:** the first stretch where attention has nothing new to hold is a structure problem. Go back to the structure pass.

Look at frames in four cases: a finding names a clip, the look alone decides it, the user asked for the work to be verified, or nobody will see it before it is used. Use `inspect_timeline` with `sheet: true`. `undo` walks back a pass that made it worse. For a last pass over rhythm and restraint, use `skill-finish`.

## Handover

- The video is in the app from its first media and plays there without rendering. The result's app_url is the link the user opens; `render_timeline` makes a file for use elsewhere. Make later changes on the same timeline; another cut of it is a variation (`manage_timelines` create with parent).
- One sentence on the story you chose and what you left out.
- Also possible: a vertical cut (`skill-content-repurposing`), clips from it (`skill-clipping`), music, a thumbnail, chapters, or the edit handed to DaVinci Resolve, Premiere Pro or Final Cut Pro (`export_project`, through the desktop app; cuts, trims, speed, framing, levels and dissolves carry over, color and effects do not).

## An example plan: recorded talk into a finished video

1. transcribe_media {refs: every source, format: "lines"}; read a source that comes back unread with {ref} (a job over five minutes), then repeat the call
2. speakers {ref} on a source with more than one voice
3. Choose the story in the words; transcribe_media {refs, format: "phrases", min_gap_ms: 150, window} over each select for its cut points (at the default 400, fast speech can come back as one phrase)
4. manage_timelines {create: {name, aspect_ratio: the source's shape or 9:16 for a feed, clips: [{ref, source: [start, end]} per select in story order]}}
5. edit_timeline {timeline_id, if_revision, operations: [moveClip, removeClip, trimClip]} for the structure and rhythm passes
6. remove_silence {timeline_id, min_silence_ms: 800, speech_padding_ms: 150}, tighter where the piece wants pace, left out where the user wants the pauses kept
7. get_transcript {timeline_id, granularity: "words"}, then remove_words {timeline_id, matches} for this speaker's fillers
8. level_audio {timeline_id}
9. captions {action: "generate", timeline_id, template: "Social" or "Subtitles"} unless the user says no
10. check_timeline {timeline_id}
11. publish_preview {timeline_id} → the link to hand over
