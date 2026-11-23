---
name: eversince-content-repurposing
description: "One finished video re-edited for each platform: its shape, length, opening and captions, each a version beside the original."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-content-repurposing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Each variation keeps what made the original work: its subject, its hook and its payoff. A new ratio means reframing each shot, not squeezing the frame. A shorter length means re-editing, not trimming the end. Leave the master untouched: every variation is a fork. A version re-voiced in another language is `skill-translation-and-dubbing`; moments lifted out are `skill-clipping`. A brief, reference or brand the user gives outranks every default here.

## The plan

The matrix repeats it per destination.

1. `manage_timelines` {create: {from: the master's id, parent: the master's id, aspect_ratio, platforms}}, the response naming its tab: or, for a file, {create: {name, aspect_ratio, clips: [{ref}]}}, then `transcribe_media` {timeline_id} where the file has no words.
2. `auto_reframe` {timeline_id: the id from step 1}: a clip whose subject is unread returns a job and lands when it finishes; `apply_layout` {timeline_id, layout: "split-v", slots: the clip's id in each} where the answer names a split-screen source.
3. `edit_timeline` {timeline_id: the id from step 1, if_revision, operations: [updateClip objectFit "contain" on a wide that plays whole or a screen recording that cannot be cropped; splitClip and rippleDeleteRange only where the destination is shorter than the master]}.
4. Only for a shorter variation: `remove_silence` {timeline_id: the id from step 1} then `get_transcript` and `remove_words` for the lines it gives up.
5. `captions` {action: "generate", timeline_id: the id from step 1, template: the master's, or "Social" for a feed, "Subtitles" for a player}: after the cut is final, where the master carries captions or the user asked for them.
6. `level_audio` {timeline_id: the id from step 1, platform}: its result says whether the target is reached.
7. `check_timeline` {timeline_id: the id from step 1, platform}: the one check; a sheet only in the four cases below.
8. `publish_preview` {timeline_id: the id from step 1}: returns the master's preview with the variation as its tile, the link to hand over; `render_timeline` {timeline_id, idempotency_key} for a file used elsewhere.
9. For a subtitled language version, where the user asked (`skill-translation-and-dubbing` writes it free): `captions` {action: "translate" (credits), timeline_id: the id from step 1, target_language, as_new_timeline: true}, then steps 7 and 8 on the timeline it returns.

## What the job needs

- The master: a timeline in the workspace, or a rendered file as a library item.
- The destinations: only the platforms the user posts on, as they name them; ask if they have not said. Each has its ratio, length and language. These multiply, and each combination is its own fork and render, so write out the matrix before the first fork. A brief that names a ratio sets it; "for social" alone means 4:5 for a near-square source, the feed ratio that crops it least.
- What carries the video: the subject of each shot, the line or moment that is the point, and where the payoff lands. Read these off the master (`get_timeline`, or `inspect_media` overview of a file) before deciding anything.

## Ratio

Decide shot by shot what the frame is for, then crop to that. A centered subject survives most crops. An off-center subject, a two-person frame or lettering near an edge does not, and each needs its own call.

- The source decides the approach. One speaker: `auto_reframe`, each shot framed on its own. Two people in one wide shot: `auto_reframe` with `follow: "speaker"` after `speakers`, so the crop cuts to whoever talks. A split-screen recording (each person in their own half): `apply_layout` with the clip's id in every slot, one tile per person. Leave a wide of three or more people centered, and name `background: "blur"` with `clip_ids` in the answer: it shows several clips whole in one call, on one layer. First-person footage or scenery with nobody in it stays centered, with a static `view` where the subject sits off center.
- Re-place text and captions for the new frame; do not scale them with it.
- Letterbox only when recomposing fails. A wide that dies in every crop plays whole as a band across the new frame: over black with the title and any captions in the space (updateClip objectFit "contain"), or over a blurred copy of itself if the user asks (`auto_reframe` `background: "blur"`). A screen recording goes in whole the same way, since a cropped interface cannot be read.
- To take a still to a new ratio, crop it on a canvas at that size (`edit_canvas`), free. `reframe_image` extends it generatively, with credits, when the whole picture must stay.

## Length

A shorter variation keeps the hook and the payoff and gives up the middle. Cutting from the end loses the point. A teaser poses the question without the answer; a cutdown compresses the whole arc. Decide first which one was asked for.

Before cutting content, tighten pauses and trim entrances and exits. A shot with no speech can run faster (setSpeed on the setup, real time through the payoff); sped-up speech sounds hurried. If the user wants it and the destination's audience knows less than the original's, the order can move too: a video that opened on the brand because the room knew it opens on the problem for a room that does not, and the proof comes forward.

## Platform

Re-edit for each platform rather than resizing: posting the same file everywhere is the most common reason repurposed clips underperform. Each variation stays recognizably the same video: same message, same footage, same voice and music, the user's own words. Change only what the platform needs: the shape, the length, a tighter first two to three seconds where the opening drags, and the pace that platform rewards. Add nothing new (lines, music, graphics) unless the user asks. Lengths that work: TikTok clips under a minute usually hold best; Reels 15 to 60 seconds; Shorts take up to three minutes but short still wins; LinkedIn tolerates one to three minutes and is watched mostly muted, so captions matter most there. No variation carries another app's watermark: Instagram has said it shows less of content reposted from other apps. Render each variation clean from its own timeline.

The same video on two platforms differs in what the first frame has to do, whether sound can be assumed, the caption look and the safe zones under the platform's interface. A vertical feed covers the top 12% and bottom 24% with its interface, so keep captions and text clear of both and place the subject a little above center. A variation keeps the master's captions, re-placed. A master without captions gets them only if the user asks, and the fork needs its words first. Loudness is per destination: read each fork with `check_timeline` and level it with `level_audio` for where it plays (`skill-sound-design`).

## Language, as subtitles

`captions` translate with `as_new_timeline` makes the fork for a language version, so run it on whichever variation you are localizing. Its response carries the new timeline's id; the source stays as it was. Only caption lines are translated. Wording in the picture, titles, lower thirds and text on a product stay in the original language; remake them on the fork or leave them. Translated lines break at different lengths, so check the fork again (`check_timeline` reports text off the frame or overlapping). Re-voicing is `skill-translation-and-dubbing`.

## The order across tools

There are two starting states. A master that is a timeline here is forked with `manage_timelines` create with `from` and `parent` (the master's id). The master stays untouched, each variation is its own timeline as a tab beside the master, and the copy's clips carry the master's words, so the word tools read the fork without another transcription. In the app each variation plays as its own tile on the master's preview, after the master: `publish_preview` with its id returns that preview and names the tile. A master that arrived as a file becomes a fresh timeline with the file on it, in one call (`manage_timelines` create with `aspect_ratio` and `clips`). It lands as one clip, and `auto_reframe` frames each shot inside it on its own, so it needs no splitting first. For a file never read, get its words with `transcribe_media` with `timeline_id`, or let `captions` generate transcribe what is unread in the same call.

Make a fork at its shape (create with `from` takes `aspect_ratio` and `platforms`), then reframe per shot. Where speech carries it, cut on the words: `remove_silence` for pauses, `get_transcript` then `remove_words` for the lines the short variation gives up. Where it does not, cut on the frames with splitClip and rippleDeleteRange, then read `get_transcript` again, since every cut moves the word positions. Ripple closes the hole on every track the cut point is clear on; a bed running through the cut keeps its shape, so re-lay it where its length mattered. Generate captions once the cut is final, since each run replaces the caption track whole. `render_timeline` makes a variation's file for use elsewhere.

A variation can also nest the master instead of copying it: `edit_timeline` addClip with `timeline_id` places the master as one clip that trims like any other. A teaser is then a trim of the nested master plus its own titles, and a fix to the master reaches every variation that nests it.

## The check

One `check_timeline` covers all variations (`timeline_ids`); no findings means it passed. It does not read text burned into the footage, or whether the hook and payoff survived. Read those from each variation's first and last lines (`get_transcript`): each should stand as if made for its destination.

Look at frames in four cases: a finding names a clip, the look alone is the judgement (no speech), the user asked for verification, or nobody sees it before it is used. One picture covers a variation: `inspect_timeline` with `sheet: true`.

A last pass over the whole video is in `skill-finish`.

## Handover

- Each variation: the destination's shape and length, the subject held in every frame, any captions clear of the platform's buttons, speech at delivery level.
- Each in the app from its first media, as a tile on the master's preview; the app_url is the link the user opens. Files and a calendar follow where the user wants them.
- With each: the check's result and whether frames were drawn.
- Also possible: other formats from the same video (a carousel of its points, `skill-making-carousels`; a thread or a LinkedIn post written from its transcript; a thumbnail), captions on a master that has none, a wide over a blurred copy of itself, a subtitled version in another language, a teaser or cutdown, a week of posts from the same material on the calendar (`skill-content-calendar`).
