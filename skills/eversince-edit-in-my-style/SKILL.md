---
name: eversince-edit-in-my-style
description: "The user's own finished videos read into a written editing style, new footage cut to it and compared."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-edit-in-my-style"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill reads how a user already edits, from their own finished videos, writes it down as a style, and cuts new footage the same way. It is what a creator does when they hire an editor: a guide with the rules they hold to, and reference videos to match.

## The plan

1. Agree which videos: three to five finished videos of one format that the user thinks are their best, or the ones that did best
2. Bring them in and read each one (below)
3. Write down what repeats across them as the style, with numbers
4. Show the user the style in the chat; they correct it
5. Keep it where the user wants it kept (below)
6. Cut the new footage to it, then compare it with one of their videos at the same kinds of moments

## One style per format

A creator who posts talking-head shorts, screen walkthroughs and polished ads edits each differently. A style read across mixed formats averages them into something the user never makes. Read one format at a time, and keep one style per format.

## Bringing the videos in

- **An Eversince timeline** is the best source: `get_timeline` returns the exact values (the caption template and its style, zoom and position keyframes, transitions, each track's volume, the grade), so nothing is estimated.
- **An item in the library**: `search_library`, or `get_items` by ref.
- **A posted video**: `pull_posts` {source, account: the post's link} then `keep_posts` (credits, a fraction of one per page). A YouTube post comes without its video: `pull_posts` transcript_of gives its words, and its pace, captions and look are read once the user adds the video file itself.
- **Media elsewhere**: `import_media`.

## Reading a video

- **Pace**: `inspect_media` {ref, cuts: true} returns every cut with the median, shortest and longest shot. Record the median and the range, not only the average: one long hold among quick cuts hides inside an average. Note where the fast and slow stretches sit (often fast in the opening, slower where a point lands).
- **Speech**: `transcribe_media` {ref, format: "phrases", min_gap_ms: 250} splits the words at every pause a quarter second or longer, and each phrase's `gap_after` is the pause the user left in. From it: words per minute, the pauses kept, whether fillers stay.
- **The opening**: `inspect_media` {ref, mode: "window", window: {start: 0, end: 3}}: what is on screen in the first seconds, whether there is text, how fast the first cut comes.
- **On screen**: window frames over a typical stretch show the captions (size against the frame, case, punctuation, words per line, position, the word picked out and how), text overlays and when they appear, zooms and punch-ins (how often, how far), B-roll (how often, from where), layouts.
- **Look**: `inspect_color` {ref, at_seconds}, once for each of two or three moments: contrast, saturation, warm or cool.
- **Sound**: `detect_beats` where there is music, for its tempo and whether cuts land on it. How far the music sits under the voice, and what the sound effects mark, are asked of `inspect_media` with a `prompt`, one question at a time.
- **Length**: the running time of each.

## Writing the style

What holds in most of the videos is the style; what varies is written as a range or left out. A rule from one video is not a style. Write it as a guide an editor could follow without asking, with a number wherever one exists:

- **Length** and **pace**: the running time, the median shot length and its range, the longest hold.
- **Opening**: how fast it gets to the point, what is on screen, what text appears.
- **Speech**: pauses kept up to so many tenths of a second, fillers out or kept.
- **Captions**: the font style, size, case, punctuation, lines and words per line, position, the highlight.
- **Text overlays**: when they appear, how long, where, their case.
- **Movement**: punch-ins and zooms, how often and how far; transitions, which ones and how often (a signature effect about once every thirty seconds is a different style from one on every cut).
- **B-roll and graphics**: how often, what kind, where it comes from.
- **Look**: the grade in a line.
- **Sound**: music or none, its level under the voice, what the effects mark.

## Keeping it

Saved only where the user wants it kept:

- **The written style**: a document in the library (`save_document`), named for the format ("Talking-head shorts: editing style"), found by later sessions with `search_library`. Or in the agent's own memory.
- **The caption look**: `captions` generate with `save_template_as`, so later captions reuse it by name.
- **A graphic the user reuses** (a lower third, a title): `compose_motion` with `save_as`.
- **The brand kit** only if the user asks for the style there.

## Cutting new footage to it

The edit runs as its kind of video runs (`skill-video-editing`, `skill-short-form-videos`, `skill-clipping` and the rest), with the style's values in place of their defaults. The style wins wherever the two differ; the footage still decides what the story is.

## Comparing

Draw frames of the new cut (`inspect_timeline`) and of one of the user's videos (`inspect_media`) at the same kinds of moments: the opening, a caption line, a zoom, a B-roll cut-in, the end.

- **Pace**: the new cut's median shot length within about a tenth of the style's.
- **Captions and text**: the same size against the frame, position, case and highlight.
- **Look**: `inspect_color` on the new cut with a frame of their video as `reference` returns the gap on the grade fields.

Fix one kind of thing per pass, timing first. The user's eye is the last check: what they correct after watching the first cut goes into the written style, so the next cut needs fewer notes.
