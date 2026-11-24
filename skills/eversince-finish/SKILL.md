---
name: eversince-finish
description: "The last pass over a whole video before it leaves: rhythm, restraint, consistency, the first and last frame, read across the cut."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-finish"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Finish is what separates a polished piece from a competent one, and it is the same on a podcast, a
vlog, an ad and a launch film: fewer things, each done fully; every cut on a reason; nothing that
moves without a job; the first and last frame chosen. It is a pass over the whole piece once the cut
is final, reading across it what each job's check reads inside it. It is not a style: the look stays
the piece's own, and the user's brief or reference outranks every default here.

## The plan

1. get_timeline {timeline_id} and get_transcript {timeline_id, granularity: "sentences"}
2. edit_timeline {timeline_id, if_revision, operations: the trims, moves and removals the reads named}
3. auto_zoom, duck_music or captions again on any stretch a change in step 2 touched
4. level_audio {timeline_id, platform}
5. check_timeline {timeline_id, platform}: a finding is fixed and the call repeated; then look for what it does not see (The reads)
6. inspect_timeline {timeline_id, sheet: true}: only in the four cases below
7. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over; render_timeline {timeline_id, idempotency_key} for a file used elsewhere, offered with its estimate first when the piece is long

## The reads

- `get_timeline` concise for the whole: the length, the tracks, the longest stretch
  without a cut, where the clips sit; `get_transcript` sentences for where each cut falls against
  the words, the first line and the last.
- One `check_timeline` with the platform, after the last change. It catches structure (gaps,
  missing media, shots too short, text off the frame or under platform buttons) and speech
  loudness. It does not see everything, so look for these yourself:
  - Text over a face: it reads faces only on clips already read for faces, and says so ("read on 0 of N").
  - Captions that cover only part of the cut, after a J-cut or a detached sound.
  - Two voices overlapping at a join, and a sound effect nearly as loud as the voice.
  - Music sitting too close to the speech when its track was never measured.
  - Subtitles that read too fast for their language, and chapters that break the platform's rules.
  - Text burned into the footage, and whether a cut lands on the right moment.
- Frames in four cases: a finding names a clip, the look alone is the judgement (a cut on
  movement, a graphic), the user asked for the work to be verified, or nobody sees the piece before
  it is used. One picture covers the piece, `inspect_timeline` with `sheet: true`; `span`
  shows one join against its sound; about five full frames a call at 1080p.
- Professional quality checks also step through every cut for a stray single frame (a flash frame or
  a black frame between shots) and listen for a pop or click at each join. `span` over a join shows the
  sound and the words but samples about every fourth frame, so draw the cut's own frame and one either side (`inspect_timeline` with their seconds) where a stray frame matters.

## Rhythm

- Every cut sits on a breath or on a payoff, never inside a word: the quiet between phrases that
  `transcribe_media` phrases marks, or the beat after the line lands.
- Entrances late, exits early: a shot starts where the viewer has what they need and ends within a
  breath of the payoff, not where the take ran out.
- A shot breathes. Two and a half seconds is the floor on talking footage; a cut on every sentence
  reads as a machine.
- No two things arrive on one frame unless one causes the other: a cut and a title, a title and a
  sound, a card and a punch-in are spread by at least a few frames, and a pause of 0.3 to 0.75
  seconds sits before a payoff.
- Pace varies on purpose: faster where the subject is already understood, slower where there is
  text to read, a reveal to land, geography to follow. A piece at one pace throughout sags.

## Restraint

- One primary transition carries most cuts, one or two accents kept for the opening, a change of
  topic and the climax; a dissolve on every cut is the slideshow.
- One accent color across the piece; a second hue is a decision with a reason.
- Nothing flashes more than three times in a second (the accessibility limit, WCAG 2.3.1): strobes, glitches and flashes on hits included.
- Punch-ins land on the claim, the reveal and the joke, and nowhere else; a rhythm of punches reads
  as a pattern, a punch on the line reads as intent.
- Nothing drifts, breathes or pulses during a hold. A held frame is still; the only life in it is
  the subject doing something.
- Every graphic, effect and sound has a job it can name (directs the eye, carries something across
  a change, shows what changed); one with no job comes out.

## Consistency

- Framings repeat on purpose: the same punch scale each time, the same lower-third position, the
  same caption template and accent from the first line to the last.
- Takes from different days, uploads and generated voices sit at one level: `level_audio` matches
  each clip's speech to the target, and its result says the target is reached
  within 1.5 LU.
- Shots from different sources agree before any look goes on, and the look sits on every shot at the
  same strength (`skill-color-grading`).
- Text sizes come from one scale; a headline, a label and a caption never fight for the same size.
- The same thing is named the same way everywhere on screen: a product, a person, a number.

## The edges

- The first frame is chosen. Speech starts 0.1 to 0.3 seconds in, not on frame 0; the picture on
  frame 0 is the strongest still the piece has, since a muted feed shows it before anything moves.
- The last frame is held. An end card, where there is one, runs as long as its platform asks (a YouTube end screen 5 to 20 seconds); a closing pose holds longest; nothing
  is mid-animation on the final frame, and no caption is up on it.
- Music starts on a downbeat and ends on the track's own landing or a fade of about a second and a
  half; no silent tail, no track cut mid-phrase.
- Fades of 10 to 15 frames on music and effects, none on speech; a video's own sound is either
  mixed on purpose or off.

## Words on screen

- On screen about 0.5 seconds plus 0.3 seconds a word, the last line at least 2 seconds; when it does not fit, fewer words, larger.
- Nothing small to read in the caption band, nothing over a face, and names, terms and numbers
  spelled right in every caption and title.

## The order

The pass runs once the cut is final, after the passes each job's skill names, and anything it
changes reopens what depended on it: a trim after `auto_zoom` or `duck_music` means running them
again on the stretch; a cut after `captions` means the captions are drawn again; `level_audio`
comes last of the edits, then the one `check_timeline`.

## Handover

- The piece at its destination's shape and length, leveled for its platform, `check_timeline` clean after the last change, with whether frames were drawn.
- In the app from its first media, where it plays, the result's app_url the link the user opens; `render_timeline` makes a file for use elsewhere, a long piece offered with its estimate before it starts.
