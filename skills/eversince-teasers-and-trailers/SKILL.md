---
name: eversince-teasers-and-trailers
description: "A teaser or trailer that makes a release wanted without explaining it, cut to its track, landing on the title and where to get it."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-teasers-and-trailers"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A teaser or trailer makes people want something without explaining it. Wanting comes from what is
held back: a cut of the best moments in order spends what it was meant to sell. Every shot raises a
question, and the only answer the teaser gives is where and when to get the thing. Use it for
anything with a release: a product launch, a video, an episode, a course, an event, a collection, a
film or a game. It sits between a highlight and a rough cut: moments lifted from existing material,
ordered to build. A teaser runs 6 to 20 seconds: one question and the landing. A trailer runs 30 to
90 seconds through the four movements below. A narrated film that explains a product is
`skill-launch-videos`. Exaggerate, but do not misrepresent: if the date is not settled, the last
card says what is true ("out now", "coming soon", the platform), not an invented date. A brief,
reference or brand from the user outranks every default here.

## The plan

1. transcribe_media {refs: the material, format: "lines"} (an item that comes back unread is read with {ref}, cloud processing) and inspect_media {refs, mode: "overview"}: the moments that raise questions marked
2. The user's track; else generate_audio {kind: "music"} for a track made to fit the cut exactly, where the user has credits; else a stock track the user picked (search_stock {kind: "music"}, import_media {mode: "url", items: [{url}]}); with none, no music and the cut runs on picture and sound effects
3. detect_beats {ref: from step 2}
4. manage_timelines {create: {name, aspect_ratio}}
5. edit_timeline {timeline_id: from step 4, operations: [addTrack {track: {kind: "audio", index: 0, id: "music"}}, addClip {trackId: "music", ref: the track}, addMarker per structural beat from step 3]}
6. edit_timeline {timeline_id: from step 4, operations: [addClip per shot with source span, on its marker frame]}
7. edit_timeline {timeline_id: from step 4, operations: [addClip {trackId: a visual track above, clip: {id, type: "text", preset: "title", content, from, durationInFrames}} for the title and landing card, last; cards on the gaps between beats only where the user asked]}
8. duck_music {timeline_id: from step 4} where dialogue lines are kept
9. Only where the user asked for sound effects: search_stock {kind: "sfx", query: <impact or riser>} then import_media {mode: "url", items: [{url}]} and edit_timeline addClip on its frame … per effect
10. check_timeline {timeline_id: from step 4}: the one check; a sheet only in the four cases below
11. publish_preview {timeline_id: from step 4}: returns the preview the trailer already has, its app_url the link to hand over

Where no cut has to land on a beat (a teaser on a calm track, a cut led by its lines), steps 2 to 8 are three calls: manage_timelines {create: {name, aspect_ratio, clips: [{ref, source: [start, end]} per shot, in order]}}; the cards as in step 7; duck_music {timeline_id, ref: the search_stock id or the user's track, end_at: the second of the track's last hit or phrase end}, which places the track with that second on the last frame, levels it and ducks it under any lines kept.

## What the job needs

- What is sold and how it lands: the title, and where or when to get it (a date, "out now", a
  link, a place). The landing is the payoff; build toward it.
- The material, usually existing footage: the finished work, the shoot, the launch video, a
  screen recording, the event's footage. With little footage (one image, a logo), build around it
  with stock B-roll (`search_stock` kind video), motion graphics (`compose_motion`) and, if the
  user wants them, generated shots in credits.
- Where it runs: 9:16 for a feed, 16:9 for a player or a screen.
- Two lists. Must not show: the twist, the ending, the price before launch, any shot not cleared.
  Must appear: the title, the landing, a logo, each with the time it needs. They land last, so they
  get squeezed first when the cut runs long.

Read the material before choosing anything: stored words with `transcribe_media` `refs`
`format: "lines"`, `inspect_media` overview then window into the stretches that matter, and
`search_footage` for a described moment. Mark the moments that raise questions, not the ones that
are merely good.

## The track decides the structure

A trailer is cut to its music. A teaser can lay a track under a finished cut, where no cut has to
land on a beat. Settle the track first: the user's own or one in the library; where the user has credits, music
made to the trailer's length and beats (`skill-voiceovers-and-music`); `search_stock` music where the user picked it,
chosen by what each result says it holds (a turn, a lift, a stop). Trim a stock or user track at a phrase
end; a track cut mid-phrase is audible however clean the picture edit is.

`detect_beats` gives every beat and downbeat in source seconds. Land cuts on them: a cut just off
a beat is audible. Reading the grid, and when not to trust a calm track's grid, is in
`skill-sound-design`. Read the track for its shape: where it lifts, drops out, goes quiet and
ends. That shape is the trailer's structure. A track with no turn gives a trailer with no turn.

## The movements

Four, and their proportions are the form:

- **The world**: where this is and who it is about, in as few shots as possible; slower cuts,
  wider frames. Spending a third of the runtime here is the standard failure.
- **The turn**: what goes wrong, the question that opens, the arrival. One moment; everything
  after it moves faster than everything before.
- **The escalation**: consequences stack, each image bigger, worse or stranger than the last, cuts
  tightening as the track climbs. The signature moments go here, one per beat.
- **The button**: after the peak, one last thing (a joke, a shock, a line, a held image), then the
  title and the landing, last on screen. The button is what people quote on the way out.

Shots shorten with each movement: held in the world, about a second each in the escalation, the
hardest cuts on the track's hits. The strongest device between movements is the drop out:
everything falls to near silence for a beat before the escalation. It costs nothing and makes
everything after it loud.

## Choosing shots

Keep shots that open questions; cut shots that close them. A face reacting to something off screen
stays; the thing it reacts to goes. A door opening stays; what is behind it goes.

- No shot that resolves: not the reunion, the win or the reveal.
- Nothing twice: a returning location, setup or kind of image reads as thin material.
- Faces early, spectacle late: people make the stakes clear, and scale means nothing until someone
  stands in it.
- Fragments: half a second from the middle of a shot, with no context, is what a trailer can do
  that a cut cannot.
- To find a shot like one already chosen, search by look, not words: `search_footage` with
  `like` on that moment returns the visually closest moments across the indexed material.

## Cards

Every trailer carries the title and landing cards. Add cards between beats (the premise in three
words, the escalation in three more) only when the user asks. Keep them to fragments, one idea
each, held long enough to read at their size. Put a card in a gap the track leaves: on a musical
hit it reads as designed, mid-phrase as an afterthought. The last card holds longest. Cards usually sit on black between shots; over footage, keep them clear of faces. Build a card
as a text clip (`edit_timeline` addClip with clip: {id, type: "text", preset: "title" or
"end-card", content, from, durationInFrames}, fields in `skill-timeline-operations`), or as a
`compose_motion` graphic when the title is a design. The title is the largest thing on screen:
the preset draws it smaller, so set `fontSize` on the clip to a tenth of the frame height or more.

## Sound

The track carries it, with silence at the drop out. If the user asks for sound effects, put impacts
on the hardest cuts and run a riser into the escalation, each on an exact frame, the riser peaking
where the change lands. Take effects from `search_stock` kind sfx, brought in with
`import_media` url; use `generate_audio` sound_effect only when the user wants one made. If the
material has dialogue, keep one or two lines: the one that states the stakes and the one that is
the joke. A trailer that explains itself in dialogue is a synopsis with a score.

## Assembly

`manage_timelines` create with `aspect_ratio`; the track first on its own lane; addMarker on the
beats the structure uses (a beat's frame is its file second minus the clip's trim plus its place,
times the fps `get_timeline` reports); then the shots against the markers, each a single addClip
with its `source` span. Generate shots at the beat lengths the structure needs instead of cutting
them down. Match the color of shots from different sources (`skill-color-grading`) when the user
asks.

## The check

- One `check_timeline`: `ok: true` means it passed, and each warning it lists is still read. It does not read text burned into the
  footage, or judge whether a shot is the right one.
- The question test and the spoiler pass, on the shot list (`get_timeline`) and the kept lines:
  every shot leaves something unanswered, and nothing from the must-not-show list is in, including
  what a fragment implies.
- Draw frames in four cases: a finding names a clip, the look alone decides (the title card), the
  user asked for the work to be verified, or nobody sees the trailer before it is used. One picture
  covers it: `inspect_timeline` with `sheet: true`.
- The landing: the title and where or when, held long enough, right after something worth quoting.
- A stranger can say what kind of thing this is, who it is about and when it arrives, and wants it.

A last pass over the whole teaser is in `skill-finish`.

## Handover

- A trailer or teaser: 16:9 unless a feed is named, nothing from the must-not-show list, the title and landing last on screen.
- It is in the app from its first media and plays there; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere.
- With it: the check's result and whether frames were drawn.
- Also possible: the usual lengths (60, 30, 15 and 6 seconds), each re-cut from the trailer rather than trimmed; a 9:16 version re-cut rather than cropped; a countdown set of teasers; captions on the kept lines; cards between beats; impacts and a riser; shots matched in color; a title designed as motion graphics; generated shots or music where the footage runs short.
