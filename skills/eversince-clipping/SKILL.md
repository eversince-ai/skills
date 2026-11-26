---
name: eversince-clipping
description: "Finds the moments in something long and cuts each to stand alone as a set of clips. One finished video from the footage: video editing."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-clipping"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A clip is a moment that works without the recording around it. The viewer never saw the hour before it, so the clip has to carry its own setup and payoff. A moment that only makes sense to someone who was there is an excerpt, not a clip. This skill finds the moments and cuts them as a set of clips, or as one highlight (below). For a video built from the whole material, see `skill-video-editing`; for several cameras, `skill-podcast-editing`; for a short built around an idea, `skill-short-form-videos`. A brief, reference or brand from the user overrides every default here.

## The plan

1. Get the recording's words, and who speaks where two people share the frame
2. Read the whole recording and mark more candidates than you need, noting the shape of each
3. Pick the strongest and cut each to stand alone: in late, out on the payoff
4. Cut the set in one call, in its platforms' shapes, captioned and leveled
5. Read each clip cold against the tests below, then check the set in the app

## What you need

- The recording as a library item with its words. `transcribe_media` with `ref` returns them, immediately if the item already has stored words. An item never read before takes a job if it runs over five minutes; call again after it finishes to get the words. You move in and out points onto the words and build the captions from them.
- Where the clips go. tiktok, instagram-reels, youtube-shorts, facebook-reels, threads, pinterest and snapchat are 9:16; youtube and x are 16:9; instagram-feed, facebook-feed and linkedin are 1:1.
- A bar and a count. A handful that clear the bar beat coverage: publishing a weak clip costs more than skipping a strong one. With no count given, selective clip teams keep about five to ten from an hour of strong talk; agencies that claim twenty to forty an hour are publishing coverage.
- Whose recording it is. The user's own, or one they have rights to, can be clipped freely. Instagram holds back from recommendations, and YouTube counts as reused content, someone else's show reposted with light edits, permission or not. Those clips need something of the user's own (commentary, a voiceover, their framing) and credit to the show and the guest.
- Speakers. Where two people share the frame (including a wide shot in an edited podcast), or the recording is one of several angles, run `speakers` over the recording once first; `reframe: "speaker"` and `get_transcript` with speakers use it.

## Finding the moments

Read the whole recording before judging anything. A line that looks flat alone can be the payoff of two minutes of setup, and the setup can be the clip.

- **Speech-led material** (a podcast, an interview, a talk): read the transcript in order with `transcribe_media` with `ref` and `format: "lines"`. That gives one timed line per sentence, about 100,000 characters per read (an hour or more), and `next_start` for the next `window`. Both the index and the judgement are in the words.
- **Action-led material** (sport, gameplay, an event): the index is still speech, the commentator's call or the streamer's reaction. Use frames to confirm a span, not to find one (`inspect_media` mode window over the candidate). A storyboard over hours is one frame every few minutes, and an eight-second peak falls between them.
- **A library, not an item**: `search_footage` with a phrase returns spoken and visual moments across every indexed recording, with their spans. Use it to gather one subject from a backlog.

What earns a mark: a surprising claim, a story with its own arc, a real laugh, a disagreement, a confession, a number that lands, the line everyone would quote tomorrow. In action: a turn, a peak, a reaction, a change of state. The shape decides the cut:

- **The claim** is fastest and needs no setup, but dies if it is not surprising cold.
- **The question and its answer** brings its context for free, at the cost of its first seconds.
- **The story** holds longest but needs its arc intact and cannot be condensed hard.
- **The disagreement** gets the highest engagement but needs both voices and a clean end.

In OpusClip's 2026 data across millions of clips, opening on story setup ("so there I was") did worst of all openings. Start a story clip on its sharpest line and let the setup follow. For a close call, ask whether someone would send it to a friend: sends carry a clip to people who do not follow the account.

Collect more than the count, since half die on review, and do the choosing in the words. Nothing measures a moment's strength, so the order of the cut is your ranking, strongest first, with the reason in `note`.

## Fidelity

A clip of someone speaking is a quotation, and tightening can break it. A late in point cuts the qualifier first. Keep whatever the meaning rests on (in the pilot, for this customer, we think, at the time) even if it costs seconds; if it cannot fit inside the moment, put it in a caption. A moment that sounds stronger alone than in the recording is a misquote with good timing. Two things kill a candidate instead of shortening it:

- **Words joined across contexts.** The parts of one clip must be one thought the person finished: its setup and payoff, with the detour between left out. A join never makes a sentence the person did not say. Each part starts and ends on a sentence edge, a claim keeps the qualifier said with it, and an answer stays with its question. Joins and `remove_words` both close gaps seamlessly, which makes this easy to do by accident.
- **Meaning that lives outside the clip**: pronouns pointing outside the span (that, they, the thing we did), sarcasm, in-jokes the room supplied. Fix it with a wider in point or a caption, never with a title that supplies the missing referent.

## The cut

- **Enter as late as possible, leave as early as possible.** Start inside the moment and put context in a caption, not thirty seconds of lead-up. The opening line has to earn the next five seconds: a claim, a question, a problem that opens a loop.
- **End on the landing.** The beat after the payoff (the laugh tapering, the nod, the half second of silence) belongs to the moment; the sentence after it does not.
- **Give it the context it cannot open without.** Read the first sentence aloud as a stranger would hear it. If it fails, open on the question that was asked, or put a short line on top ("asked about the layoffs").
- **Use the least length that lands the payoff.** Most strong clips run about 20 to 45 seconds; go past a minute only for a story that holds. A YouTube Short over a minute is blocked worldwide if any part draws a Content ID claim (music or the show's own), so keep clips of material the user does not own under a minute there. With a transcript, `cut_clips` moves each edge up to 1.2 seconds to the nearest gap between words, and the result's final spans give each clip's real length. A clip that must run an exact length is placed with `edit_timeline` addClip and `snap: false`, which keeps the span as given even inside a word.
- **One thought per clip, in one span or several parts.** Most clips are one span, the moment as it was said. When the recording spreads a thought out (setup at 6:00, a tangent, payoff at 9:10), cut it from its parts: `source` as several spans in playing order, tangent left out. Two thoughts near each other are two clips. Past three or four joins, an exchange stops sounding like a conversation.
- **A join shows as a jump.** In a talking head, a join leaves the face jumping at the cut, like any jump cut. If the user wants it hidden, a scale change across the join does it: with `zoom` on the set each join opens punched in; without it, punch in on one side (`edit_timeline` updateClip transform) or lay B-roll over the join. Skip this on serious lines: a push-in on a confession editorializes it. Cut those as one span, which has nothing to hide.

## The set

`cut_clips` cuts the whole set in one call: the parent timeline, the recording, and a span (or parts) per moment, each becoming its own clip under it. Settings shared by every clip are given once, and `edit_clips` changes the whole set later.

- **Shape**: `aspect` for the set, or per clip where one differs. A clip's own aspect wins over its platforms' and the set's; with no aspect, the first platform in a clip's `platforms` sets its shape. For a set going to TikTok and LinkedIn, put 9:16 on the set and `aspect: "1:1"` on the LinkedIn clips, or make two clips of one moment.
- **Captions**: a saved template or a built-in look by name. Social for a talking head cut for a feed, Lit box for bright or busy footage, Slam or Headline for the first seconds of a short, Subtitles for plain lines (`skill-animated-captions`).
- **Emphasis words**, when the user asks for highlights: the two or three terms the moment turns on, in their own color. Add an emoji only when it fits the voice.
- **A hook**, when the user asks for one: the line a short opens on, in a box near the top for its first seconds, under 60 characters. Leave it off where the opening line already does the work. `hook_style` on the set sets the font, colors and box for every hook.
- **Punches**, when the user asks for zooms: `zoom` light unless the brief says medium or strong, planned from the words. Without it, framing stays still.
- **Two people in one frame**: `reframe: "speaker"` moves the vertical crop to whoever is talking once `speakers` has told the voices apart; a remark under a second does not take the shot. A fast back and forth reads better stacked (`apply_layout`, a tile per person) than as a crop cutting every second, but a split held for most of a long clip tires the eye. For a split-screen recording (each person in their own half), use `apply_layout` on the clip with its `clip_id` in every slot, a tile per person. Where a multicam take exists it is the better cut, since `switch_angle` changes camera instead of crop.
- **Sound**: `level_audio` with the parent timeline's id levels every clip in one call, bringing each clip's speech to the target. For a music bed under a clip, use `duck_music` with `ref`.
- **A stream or tutorial with a face-cam**: set `layout` to where the camera sits, read off one frame with `inspect_media`. On tall and square clips the camera takes one band and the screen the rest, in place of reframe and zoom.
- **Bleep**: when the destination or the person asks for it. `bleep` on the cut or on `edit_clips` covers the usual words with a tone and stars them in the captions.

Anything specific to one clip is done with that clip's own `edit_timeline`: B-roll over a line (`search_footage` over the workspace's footage, `search_stock` kind video with `import_media` mode url, or a generated shot in credits), a graphic at a moment, a tighter in point. Remove dead air inside a clip with `remove_silence` on that clip, and fillers with `remove_words` and `matches` such as "um", "uh", "like". Both ripple every track of the clip, captions and punches included, so they can run after the cut; check a hook's timing afterwards.

The first cut publishes the set's preview: every clip plays in place with its number, line and platforms, nothing rendered, and the preview follows the set as clips are fixed or added. When it is no longer needed, take it down with `publish_preview` `unpublish: true`. `render_clips` makes files for use elsewhere, for the whole set or named clips, one job each.

## The check

`check_timeline` with the parent timeline's id checks every clip in one call; `ok: true` passes, with or without warnings. It does not read text burned into the footage or judge whether the moment is the right one. For that, read each clip's words cold, comparing its first and last lines with the transcript around them:

- **Stranger test**: someone who never saw the source gets it, start to end. **First-seconds test**: the strongest thing arrives before a scroller would leave. **Exit test**: it ends within a breath of the payoff.
- **Speaker test**: the claim holds at this width and no qualifier was cut. Names, terms and numbers in the captions are spelled right.

Draw frames in four cases: a finding names a clip, the look alone decides it, the user asked for the work to be verified, or nobody will see the set before it is used. One picture covers a clip: `inspect_timeline` with `sheet: true`.

When a clip fails, fix its in and out points first; most weak clips are strong moments cut too wide.

For a last pass over the whole set, see `skill-finish`.

## A highlight

A highlight is one idea from the long recording, cut to stand on its own: the moment someone would send to a friend, complete enough to need nothing before it. Build it from one passage, or two or three that make the same point, each running long enough to land (five seconds or more), with cuts only where the idea needs a join. A run of one-liners from across the hour is a montage: it trades understanding for energy, so make one only when the user asks. Asked for highlights with no number, make a set of three to five, each its own clip; asked for one highlight, make one. If the user wants music, put it under the idea and end it as the idea lands (`duck_music` with `ref`).

## Handover

- A set of clips: 9:16 unless the platform takes another shape, captioned and leveled unless the user says otherwise, the payoff in the first seconds and each clip ending within a breath of it.
- With it: the check's result and whether frames were drawn.
- You can add B-roll where a clip needs it: from the user's footage (`search_footage`), stock, or generated.
- The set in the app, where `cut_clips` already published its preview and every clip plays; the result's app_url is the link the user opens. `render_clips` makes the files for use elsewhere; for someone outside the workspace, `manage_public_links` create with the items the render made, uploaded first when only on the desktop app's computer.
- Also possible on a set: B-roll over a line, a hook line, emphasis words, punch-ins, pauses and fillers cut inside a clip, a crop that follows whoever talks, bleeped words, other shapes for other platforms, the files themselves.

## An example plan: a set of clips from one long recording

1. `speakers` {ref}: only where two people share the frame.
2. `transcribe_media` {ref, format: "lines"}: over five minutes this is a job; the same call after `get_jobs` returns the words.
3. `manage_timelines` {create: {name}}: the parent timeline the set lands under.
4. `cut_clips` {timeline_id: the id from step 3, ref, clips: [{source: [start, end] or [[start, end], [start, end]], name, note, platforms}], aspect, captions: a look, reframe where the shape differs from the recording's; hook, emphasis and zoom only where the user asked for them}.
5. `remove_silence` {timeline_id: each clip from step 4}: only where the user asked for the pauses out.
6. `level_audio` {timeline_id: the id from step 3}: every clip to delivery level unless the user says otherwise; name any that ends short.
7. `check_timeline` {timeline_id: the id from step 3}: every clip; a sheet only in the four cases above.
8. `render_clips` {timeline_id: the id from step 3, idempotency_key}: files for use elsewhere; the set already plays in the app, and its app_url is the link to hand over.
