---
name: eversince-launch-videos
description: "A narrated case for a product, a release or an update, with real screens as proof: one claim, clear by the second beat, one ask."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-launch-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A launch video makes a case out loud. A product page lays everything out side by side; a video
walks someone through it in an order that moves them. The usual failure is the page read top to
bottom: one feature after another, the value lost in the middle, the ask tacked on. Put the story
first. By the second beat the viewer knows who this is for and what changes for them, and everything
after backs that one claim. Teaching how something works is `skill-explainer-videos`; letting the
object make the argument with few or no words is `skill-product-videos`. A release, a pull request
or a changelog turned into a video belongs here, in its own section below. A brief, a reference or a
brand from the user outranks every default here.

Invent no number, customer, logo, quote, date or version. Proof is what the material establishes; if
a beat needs proof the material lacks, cut or reshape the beat.

## The plan

1. The voice per beat: the user's recording, or generate_audio {kind: "voiceover", text: <beat 1>, voice_id, stability} when a generated voice was chosen
2. manage_timelines {create: {name, aspect_ratio, clips: [{ref: each voiceover} in order]}}
3. get_transcript {timeline_id: from step 2, granularity: "words"}
4. edit_timeline {timeline_id: from step 2, operations: [addTrack {track: {kind: "visual", index: 1, id: "pictures"}}, addClip {trackId: "pictures", ref: each screen or footage, from: its beat's first word's from_frame from step 3, durationInFrames: up to the next beat's first word (the last one to the end of the voice)}]}: a picture with no length comes in at its default and leaves black or lands on a new track
5. compose_motion {timeline_id: from step 2, source_code, placement: {from_seconds: the beat's frame over fps, duration_seconds}} … one per graphic beat
6. A bed where the film calls for music: duck_music {timeline_id: from step 2, ref: the track}, the track in the manual's order (the user's own, then generate_audio music where credits and their way of working allow, a search_stock id only where the user picked it; none of these, no music)
7. For a feed, or where the user asked: captions {action: "generate", timeline_id: from step 2, template: "Subtitles" for a player, "Social" for a feed}
8. level_audio {timeline_id: from step 2}: the voice to delivery level
9. check_timeline {timeline_id: from step 2}: the one check
10. inspect_timeline {timeline_id: from step 2, sheet: true}: only in the four cases below
11. publish_preview {timeline_id: from step 2}: returns the preview the film already has, its app_url the link to hand over

## What the job needs

- The material: the product page, screenshots, screen recordings, footage, the brand's look, the
  numbers the maker stands behind. A digital product's pictures are its real screens, placed as
  files, not a video model's redraw. If part of a screen has to move, rebuild just that part as a
  `compose_motion` graphic over the still, with the screenshot as the graphic's `image` control,
  set in `values` to its ref. Check every screen for a real customer's name, email or data before using it.
- Six facts, written before any beat: who the video talks to; what they want gone or want to have;
  the promise in one sentence; what the product does in their story; the proof (demos, results,
  numbers, names); the ask, one next step. The page's feature list decides nothing.
- The destination: a launch runs 30 to 90 seconds; past three minutes it is a tour. 16:9 for a site
  or player, 4:5 or 1:1 in a feed, 9:16 for short-form.
- A script the user brings is either fixed or open to rework. Split a fixed script into beats only
  where a sentence or clause ends, and change no word. Release notes, a changelog or a page are
  material for the beats, not a script to read.

## An open brief

With no script, reference or beats given, sketch three different films in words before building
anything. Make the one a stranger would follow and remember, over the one with the most striking
picture. Different means another world, device and form, not three titles on one film. Devices: a
moment in someone's day, a cursor or character doing the work, the feature happening to a
recognizable image, the result first and then where it came from, one object traveling across apps,
before and after. Forms: one continuous take, each shot ending on the thing that opens the next, an
endless zoom, match cuts, one frame that keeps rebuilding itself.

One viewer does one real task, and one real thing (a file, a prompt, a number) travels from the first
beat to the result.

## The arc

Pick one arc and keep its order. Name a combination ("the problem, with the proof told as feature
then gain"); don't stitch parts of two arcs together.

| Arc | When | Beats |
|---|---|---|
| The problem | the pain is felt and wanted gone | tension, the pain, what it costs, the turn, the product, proof, ask |
| The new way | a different way of working, or a new category | picture it, the product named, the old pain gone, how it works, what they end up with, ask |
| Show it working | watching the interface is the explanation | a question, the product, a first run, a second run, reasons to trust it, ask |
| Then and now | moving people off an old workflow | how it was, a glimpse of how it could be, the product as the way across, the first step, the second, the result, ask |
| Feature, then gain | many capabilities, or something bought for desire | the category, a feature and what it gives, again, building to a peak, ask |

Say a feature as what the viewer gets: "search across every call" becomes "find the moment a
customer said it". An interface usually needs three or more beats on one screen: what goes in, what
comes back, the result, why it matters.

## The beats

Give every beat one named job: the opening, the pain, the first look, a feature in use, proof,
value, the ask, the sign-off. A beat whose only job is "and also" has none. Each beat carries the
sentence the viewer takes away, how it persuades (a before against an after, a demonstration in
place of a claim, a figure with stakes, a customer's words, a known name, the risk removed, the
first step made small, three of a kind) and the feeling it moves toward (unease or curiosity into
relief, ease, trust or excitement; the ask comes once that feeling has arrived).

- **The opening** has three to five seconds to make the viewer want, wonder or feel a problem, in
  their terms: what they get, what they escape, what finally makes sense. Not the company describing
  itself, not a feature's name, not a logo animation before anything happens. Open on a figure only
  with stakes ("checkout drops from four seconds to one"), not as a count ("three hundred templates").
- **The value is clear by beat two.** Two tests on the beat list: remove every proof beat and the
  case still stands; remove the value beats instead, and if the video still seems fine, it was a
  feature list.
- **The ask** is one concrete action with the offer said plainly, and only a true offer.

## The script

A generated voice speaks about 2.2 words a second, a person about 2.5. Keep a beat to 19 words or
fewer, around nine seconds. Up to two beats a film can stretch to about 26 words for the one thing
that needs explaining. A 60-second launch holds roughly 130 words once its silences are counted. A
beat with no voice, the interface speaking for itself, is allowed and often the strongest.

- Break lines into cues ("drafts, approvals, publishing, all in one place" gives the picture three
  reveals), and show each thing as the voice names it.
- Spell words the way they should sound, since `generate_audio` reads the text exactly: "about four
  and a half million", "C S V", "example dot com", a name not a handle, "yam-el" for an abbreviation
  said as a word, "post-gres" for a name that blends letters into one sound. Speak no version number,
  hash or file extension. A comma or a dash makes a pause. Put a precise figure on screen and keep it
  out of the narration.
- Where a feature starts from one line (a slash command, a typed command, the sentence given to an
  agent), the script says it word for word while the picture types it.
- Use contractions, short sentences among longer ones, the viewer's language. Rewrite a line that
  reads as marketing copy; reading it faster doesn't fix it.

The beat table is the plan: per beat, what is spoken, what is seen, and its purpose in terms of the
promise. Drop any beat you can't tie back.

## From a release

A diff records how the code was edited; the video explains what the edit does. Impact comes before
code: by the second beat the viewer knows what the change makes possible, repairs or speeds up, and
the code follows as proof. Don't open on code.

- **Read the whole source** before writing a word: the diff, the commits, the release notes, a
  changelog as a document (`read_document`). Nothing here fetches a repository; read it with your
  own tools.
- **The state is a fact.** Proposed, merged and released are different claims. Take a version from a
  release tag or the manifest on the default branch; merged without a tag is "in the next release";
  with neither, the close shows the address and no version. Same for dates.
- **The angle** sets the order: a changelog (the release in a line, a beat per change, where to get
  it); a new feature (what someone can now do, its name, the code, the feature running, back to the
  promise); a fix (the symptom, the bug happening, the cause and the fix, the behavior now right); a
  refactor (what the old structure cost, old beside new, the gain measured). Tell a feature that also
  fixes a bug as the feature.
- **Two to four real hunks across the whole video**, four to twelve lines each, never a whole file;
  leave out lockfiles, formatting and generated files. Alternate code with behavior: a retry is a
  call failing and a longer pause each time; caching is the same request twice, the second answered
  at once; concurrency is one queue splitting into several; performance is old and new side by side.
- **Show each change in a changelog; don't read a list**: one visual per theme, at most three items
  said aloud, about ten seconds a theme, the rest behind the address at the close. Use the first
  honest visual in this order: the real product; a stand-in that behaves the same; a terminal; a
  checklist of six rows at most.
- **The code is the diff's own text**, copied into the graphic as data. The motion compiler scans
  the whole source, strings included, and refuses `fetch(`, `window`, `document`, `process`,
  `setTimeout(`, `eval(` and `Math.random`; split a real line containing one across a string
  join so the screen still shows it exactly. Four to twelve lines a panel, code no smaller than about
  24px at 1080, ligatures off, each hunk landing then holding at least a second. Show a change as a
  diff (removed lines tint and close, added lines open in turn, the line that matters marked); type
  new code at 15 to 20 characters a second; morph a refactor. Use plain windows, not a real editor's
  chrome.
- A credits close is optional and names real people only.

## Building it

Lay the voice down first; it sets the clock. Use the user's own recording, or a generated one when
they want it (`generate_audio` voiceover, in credits, priced before it is made): one item per beat,
the same `voice_id` and `stability` throughout, finishing inside the call. Each beat lasts as long
as its voice; if a line runs past a capped shot, keep the delivery and extend the picture. Set the
delivery with tags before the line (`skill-voiceovers-and-music`), not to fit a line.

Then build the picture beat by beat. The shot's shape follows its role, and `skill-motion-graphics`
holds the rules for it; type, color and composition are `skill-graphic-design`. Place real screens
and footage as they are: an image or video clip under a graphic when they are the ground, inside the
graphic when it has to move with them. The music bed and the duck under the voice are
`skill-sound-design`; captions for a feed that plays muted are `skill-animated-captions`.

On the timeline: `manage_timelines` create with the destination's `aspect_ratio` and the voice
clips as `clips` in beat order, in one call, back to back on their own lane; then place each beat's
picture against them. A voiceover made with `generate_audio` carries its words and length on its
item (the job id is its ref; `get_items` gives the duration), so `get_transcript` reads every
word's frames once the clips are placed, with no transcription pass. For an uploaded voice, get its
words with `transcribe_media` `timeline_id`. Land every cut and reveal on the frame of its word.
Captions cut from a respelled voice show the respelling; `captions` rewrite restores the written
form and keeps the timing.

## The check

- One `check_timeline`: `ok: true` means it passed, and each warning it lists is still read. It does not read text inside a screen, a
  graphic's own layout, or whether a picture shows what its line says.
- Look at frames in four cases: a finding names a clip; a picture can only be judged by its look (a
  graphic written as code, a thing carrying across a cut); the user asked for the work to be
  verified; or nobody sees the video before it is used. One picture covers it: `inspect_timeline`
  with `sheet: true`.
- Every number, name, claim and version matches the source; no screen shows private data; the ask
  points somewhere real; a respelled term shows its written form in the captions.
- Both removal tests hold on the finished cut. Muted, the picture and any captions carry the
  argument; unwatched, the voice does.
- After one viewing a stranger can say who it is for, what changes for them and what to do next; a
  developer can say what changed and why it matters. If they can only list features or file names,
  it was a tour.

A last pass over the whole film is in `skill-finish`.

## A changelog video

A shipped feature is often one short clip of it in use plus the post announcing it. For release
notes, make the clip a GIF: `render_timeline` format gif, silent, up to 30 seconds. Work from what
shipped (the release notes, the pull request, the changelog, the product itself), read all of it,
and claim nothing it does not establish.

## Handover

- The video in its destination's shape, with each platform the user names as its own cut.
- The voice at delivery level; captions on a feed cut where the user wants them; any music bed ducked under the voice.
- The last frame holds the ask: the product's name and where to go.
- It is in the app from its first media, where it plays; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: a short teaser for the days before the launch (`skill-teasers-and-trailers`), cuts for each platform, stills and banners from the same screens, the launch copy, a GIF for the release notes, a music bed, captions on a player cut, a generated voice or music.
