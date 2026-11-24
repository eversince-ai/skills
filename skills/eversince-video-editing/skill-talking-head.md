# Video editing: talking head

A talking head is one person, one camera and a script or a set of points, recorded in takes. The
structure exists; the edit keeps a viewer through it: restarts and fillers gone, the pauses at the
pace of speech, the sound at a level a phone plays well. This is the part of `skill-video-editing` for one person to camera; the story, the passes and the finishing there apply. Several cameras on a
conversation is `skill-podcast-editing`; a screen recording is `skill-screen-recording-editing`. A brief, a reference or a brand the user gives outranks every default here.

## The plan

Steps 2, 7, 8 and 13 only where the user asked for them. Steps 5, 6 and 10 run on a feed cut unless the user says otherwise, and on a long-form piece where the user asked.

1. `transcribe_media` {ref: the recording, format: "phrases"}; over five minutes a job, the same call after it finishes
2. `clean_audio` {ref: the recording, mode: "denoise"} where the user says the room is noisy; `get_jobs` returns the clean item, the same length as the original, so spans read on the original hold on it
3. `manage_timelines` {create: {name, aspect_ratio: the source's shape, or 9:16 for a feed, clips: [{ref, source: [start, end]} per keeper take in order]}}; the response carries the timeline id
4. `transcribe_media` {timeline_id: from step 3} only after step 2, where the cleaned item is read once; takes from the recording read in step 1 carry its words
5. `remove_silence` {timeline_id: from step 3, min_silence_ms: 600, speech_padding_ms: 120}
6. `get_transcript` {timeline_id: from step 3, granularity: "words"} then `remove_words` {timeline_id, matches: ["um", "uh"]}
7. `edit_timeline` {timeline_id: from step 3, if_revision, operations: [addTrack {track: {kind: "visual", index: 1, id: "broll"}}, addClip {trackId: "broll", ref, source: [start, end], from: its line's frame} per cover, updateClip {clipId, patch: {volume: 0}} on each; addMarker {marker: {from, label, kind: "chapter"}} at each point; addClip {trackId: "visual-main", clip: {id, type: "text", preset: "end-card", content, from: after the last word, durationInFrames}}]}
8. `auto_zoom` {timeline_id: from step 3, intensity: "light" unless the brief names a stronger one}, or `auto_zoom` {timeline_id, clip_id, moments_seconds: the claim, the reveal, the joke} where a hand-set punch matters
9. `level_audio` {timeline_id: from step 3}
10. `captions` {action: "generate", timeline_id: from step 3, template: "Subtitles"}
11. `check_timeline` {timeline_id: from step 3}: the one check; a sheet only in the four cases below
12. `publish_preview` {timeline_id: from step 3}: returns the preview the piece already has, its app_url the link to hand over; `render_timeline` {timeline_id, idempotency_key} for a file used elsewhere
13. `extract_media` {ref: the recording, mode: "frame", at_time: the strongest expression} for the thumbnail, and the cutdowns per `skill-clipping`, where the job asks for them

## What the job needs

- The recording as a library item, or several takes as several items, with words: `transcribe_media`
  with `ref` reads an item (over five minutes, a job); with `timeline_id` it gives the
  clips their words once the takes are on a timeline.
- The destination: 16:9 for a player or a site, 9:16 for a vertical cut; whether it plays with sound
  by default (a feed usually does not).
- The runtime: a creator's weekly piece 6 to 12 minutes, a lesson as long as its material, an update
  under 3.
- Footage or screens the user holds that a line refers to; their own material covers a line better
  than anything found.

## Reading the takes

The transcript is read whole first (`get_transcript` sentences, or `transcribe_media` with
`format: "phrases"`, which marks the pauses where a take starts and ends). It gives:

- where each point starts and ends: the chapters;
- every restart, the same sentence begun twice, the second usually the keeper: the take that finishes
  its thought without a stumble, energy over polish where they disagree;
- the lines that name something the viewer cannot see (a product, a screen, a place, a number): the
  B-roll list.

Names, products and numbers are where a transcript errs; a doubtful word is checked against the
picture with `inspect_media` frame before a cut rests on it. A recording that is already an edit
(two cameras cut together, captions or a logo laid in) shows it in the `inspect_media` overview:
its own cuts stay, and punches or captions are left off where it carries its own.

## The retention edit

The edit asked for is this section: takes, pauses and fillers, the pace kept up, the framing as it was shot. Punch-ins, B-roll, chapters, an end card, captions, a thumbnail and cutdowns are made when the user asks for them, and offered in one line at the handover otherwise.

Structure first, then the mechanical passes, in the order the tools set.

1. **Takes.** The keeper of each restart goes on as one `edit_timeline` addClip with its span as
   source, each landing after the last; the rest never lands. A take that runs into a good line and
   then a bad one is cut at the last word that lands. Every cut sits in the quiet between words
   (`transcribe_media` phrases marks it): a cut on a breath reads as an edit, inside a word as a
   fault.
2. **Words on the cut.** Takes cut from a recording already read carry its words, which every tool
   below reads; `transcribe_media` with `timeline_id` reads only clips without words and answers
   `already: true`, nothing charged, where every clip has them.
3. **Pauses.** `remove_silence` with `min_silence_ms` around 600 and `speech_padding_ms` around 120
   is the pace of a posted talking head: the breath before a line stays, the thinking gap goes. A
   lesson takes longer padding, a creator cut does not. It runs before the words are cut, since every
   cut shifts the word indices; `short_pieces` names a word or two left between two cut pauses,
   with the span `remove_words` takes.
4. **Fillers.** `get_transcript` words, then `remove_words` with `matches` for the fillers this
   person says ("um", "uh", "like" only where it is filler). The pause around a removed word goes
   with it, leaving the beat `cut_aggressiveness` sets: tight about 0.2 s, balanced about 0.3 s
   (the default), loose about 0.4 s. `removed` counts the words; `words_touched` appears only
   where a removed word keeps a few frames because its stored timing overlaps a word that stays. Whole false starts go as [start, end] spans. `get_transcript` is
   re-read between calls.
5. **The jump cut.** Every cut on a locked-off camera shows the head jumping. The cut wears its
   jumps, as a talking head edited anywhere does; where the user wants them hidden, `auto_zoom`
   with no `clip_id` holds alternate short pieces closer for their whole length (`held_closer`
   names them), B-roll covers the join, or a chapter card sits between the points.

What stays: the pause before a payoff, the laugh after a line, the breath that shows thinking. A cut
that removes every gap reads as a machine talking.

## Pace, from what editors use

No study gives one right interval; these are working ranges from creator editors, to set against the user's own style:

- **Cuts**: a creator's piece runs a cut every 3 to 8 seconds; a calm lesson or founder update holds 15 to 25 seconds between cuts, broken by short bursts.
- **Punch-ins**: about 110 to 130% on the key line, held one to three seconds, and not more often than once every three or four cuts.
- **Too much backfires**: constant zooms and cuts tire viewers, older audiences most.
- **B-roll** goes over an explanation longer than a couple of sentences, laid with the voice running under it (an L-cut) so the sentence is not broken.
- **The user's retention graph** (in their YouTube analytics) shows where viewers left; those dips are where the next edit tightens.

## Punch-ins

Punch-ins are a look, made where the user asks for zooms or a reference shows them. `auto_zoom`
reads the words the clip carries and punches on sentence starts and pauses, cycling a few framings,
at the rhythm of `intensity`: light (about one punch per eight seconds at 112%) unless the brief
names a stronger one, medium a creator's piece, strong a fast feed cut. It writes the scale and position keyframes
and replaces what they held, so it runs once the cut is final, and again after any later cut.

Hand-set punches beat the rhythm where a line matters: `moments_seconds` with `clip_id` puts the
punch on the claim, the reveal and the joke and nowhere else, which reads as intent. A punch lands on
the first word of the line. The face stays whole: a punch that crops the forehead or the chin is too
close for the framing the user shot. Where the clip's subject has been read (`auto_reframe`), each
punch holds on the face; a talking head framed off center is reframed before it is punched.

## B-roll

A line that names a thing the viewer cannot see is covered by the thing, from any of: the person's
own footage and screens (`search_footage` by phrase over the workspace's recordings,
`search_library` for the item), a frame or a still they hold, stock (`search_stock` kind video,
brought in with `import_media` mode url), or a generated shot (`generate_video`, in credits; it
cannot show the real product). The real interface is the file itself, never a
model's redraw (`skill-screen-recording-editing`).

- The cover goes on a visual track above the face (addTrack, addClip with its span as source), the
  face kept underneath, so the voice runs through and the cut back is a removal.
- A cover starts on the first word of the line it illustrates and holds two to six seconds; past about
  eight seconds without the face the piece becomes a voiceover.
- The cut back to the face lands on the payoff: the number said, the point made, the joke.
- B-roll's own sound is off (updateClip volume 0) unless a sound from it is the point; then the voice
  track's volume keyframes duck under it by hand.
- One cover per point at most in a lesson; a creator's piece covers more and shorter.
- B-roll that should look like a shot already in the piece is `search_footage` with `like`, a moment
  of a video or a picture, rather than a phrase.

## Chapters and the end card

Chapters go where the points change, not where the takes did: `edit_timeline` addMarker kind chapter
at each point's first word, three or more, the first at frame 0, each at least ten seconds
(`skill-timeline-operations`). `get_timeline` returns them, which is what a platform's timestamp
list is written from; their frames are set after the last cut, read off `get_transcript` sentences.
A chapter card on the picture is a text clip (preset "title", or its own `fontSize` at 4% of the frame's height or more) over the first second of
the point, or a graphic from the Titles shelf (`skill-motion-graphics`).

The end card holds 8 to 20 seconds after the last line: what to watch next, the ask, the mark. A text
clip with preset "end-card" is the plain version; a card from the Calls to action shelf placed with
`compose_motion` the designed one. The piece ends on the card, never on the person reaching for the
camera; the last take is trimmed to the last word that lands.

## Sound

`clean_audio` denoise on the recording where the user says the room is noisy (cloud processing), before any cut, since the cleaned item
takes the clip's slot whole (with `timeline_id` and `clip_id` the clip's words stay). Once the cut is
final, `level_audio` brings the speech to the delivery target and evens the takes, which makes a
piece cut from several recordings sound like one sitting. A bed and the duck under the voice are
`skill-sound-design`; a talking head usually carries no bed.

## Captions, thumbnail, cutdowns, the series

- Captions: `captions` generate
  (`skill-animated-captions`); Subtitles or Broadcast for a long player piece, Social for a feed cut.
- The thumbnail is one of the recording's own frames (`inspect_media` overview, `extract_media`
  frame at the strongest expression), built in `skill-making-thumbnails`.
- The vertical cutdowns are the moments that stand alone, cut as a set with `cut_clips`
  (`skill-clipping`); a vertical version of the whole piece is `skill-content-repurposing`.
- A channel repeats its shape, so the parts are built once: an intro or outro is its own timeline
  nested in each episode as one clip (`edit_timeline` addClip with `timeline_id`); a lower third or
  end card is kept with `compose_motion` `save_as` and placed by name; a caption look with
  `save_template_as`; a whole episode's shape as a template (`manage_timelines` template), the next
  one made with `from_template` and `fill`, the new takes in its holes.

## The check

- One `check_timeline`: no findings is the check passed. It does not read text burned into the
  footage, or whether a take is the right one.
- `get_transcript`: every point whole, no false start survived, no qualifier left with a cut; the
  first lines open on the point, not on a greeting the cut could have removed; nothing after the
  last word. `get_timeline`: any chapters three or more, each long enough, an end card its full time.
- Frames in four cases: a finding names a clip, the look alone is the judgement, the user asked
  for the work to be verified, or nobody sees it before it is used. One picture covers it:
  `inspect_timeline` with `sheet: true`.

A last pass over the whole piece is in `skill-finish`.

## Handover

- A talking head for a feed: 9:16 unless a channel is named, opening on the point rather than a greeting, pauses and fillers out and captions on unless the user says otherwise.
- A long-form piece: the source's shape, three or more chapters where it has them, nothing after the last word but the end card.
- Speech at delivery level (`level_audio`); in the app from its first media, where it plays, the result's app_url the link the user opens; `render_timeline` makes a file for use elsewhere.
- With it: the check's result and whether frames were drawn.
- Also possible, offered in one line rather than made: punch-ins, captions, B-roll, chapters, an end card, a thumbnail, vertical cutdowns, a cleaned room sound.
