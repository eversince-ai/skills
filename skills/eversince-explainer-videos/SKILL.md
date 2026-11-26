---
name: eversince-explainer-videos
description: "A narrated video that makes one idea understood, beat by beat, with motion graphics, footage, stock or generated pictures."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-explainer-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

An explainer is an argument with pictures, and the argument comes first: one idea, taught in beats,
each beat one sentence the viewer keeps and one picture that shows it. If the script does not teach
on paper, animation will not fix it. To persuade, use `skill-launch-videos`; for a video with no
voice, cut to a track, use `skill-music-videos`. A faceless channel is `skill-faceless-videos`.
A brief, reference or brand from the user outranks every default here.

## The plan

1. The script and the beat list, settled; list_models {type: "voice"} for the voice_id; estimate_cost for the voice
2. generate_audio {kind: "voiceover", text: <beat 1>, voice_id, stability, idempotency_key} … one per beat; each job id is its item's ref. Priced and not made: step 3 creates the timeline empty, each beat's line goes on as a text clip at speaking pace, and step 4 is skipped
3. manage_timelines {create: {name, aspect_ratio, clips: [{ref: each voiceover from step 2}] in order}}
4. get_transcript {timeline_id: from step 3, granularity: "words"}
5. compose_motion {timeline_id: from step 3, source_code, or template with values (the beat's own words and figures in place of the template's samples, its controls read with get_templates {id}), placement: {from_seconds: the beat's first word, duration_seconds}} … per beat or per run of beats
6. search_stock {kind: "video", query} and import_media {mode: "url"} for a beat that wants the thing itself; edit_timeline addClip on a track above, on its word
7. Only where the user asked for music or effects: duck_music {timeline_id: from step 3, ref: the user's track or a search_stock id the user picked}; a search_stock kind sfx on a landing
8. For a feed, or where the user asked: captions {action: "generate", timeline_id: from step 3, template: "Social" for a feed, "Subtitles" for a player, style: {maxWords: 3}}
9. level_audio {timeline_id: from step 3}; check_timeline {timeline_id: from step 3}: the one check; a sheet only in the four cases under The check
10. publish_preview {timeline_id: from step 3}: returns the preview the video already has, its app_url the link to hand over

The user's own recording works the same: it goes on the timeline as the clip in step 3, then transcribe_media {timeline_id} runs before get_transcript.

Fully generated (a faceless channel, an all-AI look): steps 5 and 6 become generate_image per designed frame, checked as stills, then generate_video per section with the frame in reference_refs, placed on its beat's first word; step 7 is generate_audio {kind: "music"} placed with duck_music; the estimate in step 1 covers all of it.

## What the job needs

- The idea the viewer should understand at the end, and what they know now. The script covers the
  distance between them.
- The audience. It sets the register and the vocabulary.
- The destination: 16:9 for a player or a site, 9:16 for Shorts, TikTok and Reels, 4:5 or 1:1 in a
  feed; and whether sound can be assumed.
- The material: an article, notes or a document (`read_document`), screenshots, footage, an earlier
  video, a brand's look. A source is information, not a running order. Its headings are never the
  hook; rearrange, combine and cut its sections into the order the viewer needs.
- A script brought in is fixed or reworkable. Divide a fixed script into beats only where a
  sentence or clause ends, change no word, and keep its runtime.
- The runtime. An explainer works at 30 to 90 seconds; past about three minutes it is a lesson. In a study of 6.9 million edX sessions, viewers watched nearly all of videos under six minutes and about half of those running 9 to 12.
  With too much material, narrow the promise, not the pace.

Explain only what is established. Leave out any number, mechanism, date or claim the material does
not carry, and build the beat around what is known.

## The script

Write it whole before any picture.

- **One arc.** An idea (name it, open it a layer at a time, say what follows); a sequence of steps
  (each step its own diagram, drawn as it is spoken); a set of equal items (three is the strongest
  count, the best last); one example followed from start to lesson; a belief stated then
  overturned; a problem the viewer has, solved at the turn. One arc, or a named combination, never
  parts of two.
- **The hook raises a question**, in the viewer's words: a figure with stakes, a thing the viewer
  has wondered, a claim against what they think, a muddle they recognize, an image that makes the
  abstract physical. Never open on a definition.
- **The idea is clear by beat two.** Two tests on the beat list: remove every supporting beat and
  what is left still states the idea; remove the idea's own beats and, if the video still seems to
  work, it was a tour.
- **One idea per beat, numbered.** Each beat carries the sentence the viewer keeps and how it
  sticks: made tangible, shown a part at a time, set against its opposite, given a shape, backed
  by a figure or a source, or closed off. A beat that needs three pictures is three beats.
- **A beat for what it is not.** Name the neighboring idea the audience will confuse it with and
  set it apart; the boundary teaches more reliably than another example.
- **Written for the ear, as cues.** Six to twenty words a beat, a line broken where the picture
  changes ("the seed, the root, then the first leaf" gives three reveals). Concrete words over
  category words. A beat can have no voice while a chart draws itself.
- **Timed at speaking pace.** About 150 words a minute, nearer 130 where the idea is dense: ninety
  seconds is about 225 words. The runtime follows the script.

The beat list is the production plan: for each beat, its line, what is seen, and its purpose for
the idea. Drop a beat whose purpose does not tie back now, while dropping costs a sentence.

## The voice

Make the narration first; every cut and reveal is timed to it.

- Where the user wants their own voice, use their recording, transcribed with `transcribe_media` for its timing. Otherwise make one `generate_audio` voiceover per beat, with the same `voice_id` and `stability` on every call so
  the takes share one delivery. A voiceover finishes inside the call: the job id is the item's ref,
  `get_items` gives its duration, and its words ride on the item, so nothing needs transcribing.
  Each clip's real length is its beat's length; revising a beat is one regeneration.
- Set the delivery with tags before the line (a pace or a tone, such as "[calm, unhurried]"); `skill-voiceovers-and-music` has the tags. When a line runs longer than its picture, keep the
  delivery and extend the picture: hold the finished frame, or add a second shot.
- For a short video where the flow across sentences matters more, make one file and read its beats
  from `get_transcript` sentences.
- Spell the script the way it should be said (`generate_audio` reads the text exactly):
  "C S V", "about four and a half million", "example dot com". Captions are cut from those words,
  and `captions` rewrite restores the written form with the timing kept.
- When the voice is priced but not yet made, put the script on the timeline as text clips, one a
  beat, each held for its words at speaking pace, and cut the pictures to those. Once real takes
  exist, they replace the text and every reveal is retimed to its word.
- A presenter on camera is one beat, not the whole video: the opening and the close, where a face
  earns trust; the pictures carry the rest. In the edX study, a face cut between slides held viewers
  better than slides alone. Generate the presenter's line as that beat's voiceover
  and hand it to a video model that takes reference audio (`list_models` says which).

## The pictures

What the research on learning from video (Mayer's multimedia principles, over 200 experiments) finds, strongest first:

- **Show it as it is said.** The picture appears on the words that name it, not before or after. This has the largest effect of any principle.
- **Words on screen are labels, not the narration.** Printing the spoken sentence beside the picture lowers what people take in. Keep on-screen text to keywords, figures and names. Captions for sound-off feeds are a separate need.
- **Cut what only decorates.** Extra footage, music or detail that does not carry the idea lowers learning, even when it is interesting.
- **Point at what is being discussed.** An arrow, a highlight or a dimmed surround on the part the voice names.
- **One step at a time.** Beats that each finish before the next starts.

Each beat gets one picture. A run of body beats (the steps of a process, the layers of a mechanism)
is one stage growing across the run, not a new picture per beat. Pictures come from any of these,
alone or together:

- **Motion graphics** (`compose_motion`): the diagram, the layout, the text-led beat, a counter
  counting real numbers, a chart true to data, a callout pinned to a detail, each reveal on its
  word. Code renders what generation only approximates. The shelf's templates (`get_templates` kind
  graphic) are the fast start; `skill-motion-graphics` has the craft.
- **The user's own material**: footage, screenshots and pictures they hold (`search_footage`,
  `search_library`). Only the file shows a real interface pixel-true: the screenshot as an image
  clip, callouts as motion graphics. With no screen available, use the first honest stand-in:
  something that behaves the way the thing does, then a terminal with the command and its output,
  then a checklist of six rows at most. Draw nothing that suggests a screen the product does not have.
- **Stock**: B-roll video and pictures (`search_stock` kind video or image, `import_media` url)
  where the thing itself is the best illustration, chosen by each result's description.

- **Generated pictures**, in credits, instead of or on top of the above. One route that holds a
  layout: generate the designed frame as one image (`generate_image`, copy in double quotes, the
  bottom sixth empty where captions will burn in), then animate it with `generate_video`, the frame
  in `reference_refs` and a prompt that builds the layout in the order the narration names it
  (`list_models` says which models take references and their durations). Check the stills before
  paying for any video.

Decide one design system for every picture before the first frame (`skill-graphic-design`). A short video
collapses to one idea, one picture, one build.

## Sound

The voice leads. Under it, add music where the user asks for it (`duck_music` with `ref`, the
user's track or a `search_stock` id the user picked, placed, leveled and ducked in one call), and a sound effect,
asked for the same way, on a landing the beat list names (`search_stock` kind sfx, `import_media` url, on its word's frame,
quieter than the voice). `generate_audio` makes music or an effect that fits exactly, in credits.

## A faceless channel

A channel of episodes with nobody on camera (finding the topic, keeping one format, staying clear of mass-produced content) is `skill-faceless-videos`; each episode is made as this skill makes an explainer.

## Before anything is paid for

Settle the script before the first paid call: a line changed now costs nothing, a regenerated voice
or picture costs credits. Know the cost of what it will generate in advance (`estimate_cost`).
Whether to show the user the script and its cost first is up to you.

## The order across tools

Narration first, so its frames set every cut; designed frames next, so the video is judged in
stills; video and code last, against the frames the narration gives.

On the timeline: one `manage_timelines` create with aspect_ratio and the narration clips in order, as
in step 3. `get_transcript` words returns every word as [index, text, from_frame, to_frame]; land each cut and
reveal on the frame of its word. Inside a `compose_motion` clip, a reveal's frame is that frame
minus the clip's own start. Get an uploaded clip's words with `transcribe_media` `timeline_id`;
a generated voice already carries them. For feed work, `captions` generate with maxWords 3 to 4 under narration; the rest is in
`skill-animated-captions`.

## The check

- One `check_timeline`: `ok: true` means it passed, and each warning it lists is still read. It does not read text inside a picture,
  a graphic's own layout, or whether a picture shows what its line says.
- Read `get_transcript` once more against the source and the beat list: every number and claim,
  the first line a question and the last the idea. With the picture off, the voice alone teaches.
- Look at frames in four cases: a finding names a clip; the look alone decides (a graphic written
  as code, a generated frame); the user asked for the work to be verified; or nobody sees the video
  before it is used. One picture covers it: `inspect_timeline` with `sheet: true`, read against
  the beat list. Remake or cut a beat where the picture only decorates.
- The cold read: every beat can be said back from its own line and picture. A beat that needs its
  neighbors to make sense is a script problem.

A last pass over the whole video is in `skill-finish`.

## Handover

- A player at 16:9; a feed at 9:16, with Social captions where the user wants them.
- The voice at delivery level (`level_audio`); where music runs, it is ducked under every word (`duck_music`).
- Frame 0 a designed picture, never black; the last frame held on the final idea.
- With it: the check's result and whether frames were drawn.
- In the app from its first media, where it plays; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: music under the voice, a sound effect on a landing, captions on a player cut, generated pictures or video for a beat, a presenter on camera for the open and close, a vertical cut for a feed, a version in another language, a thumbnail, for a channel the next episodes on a schedule (a routine in the user's agent, each reading the channel's format first).
