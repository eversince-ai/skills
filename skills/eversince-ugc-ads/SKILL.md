---
name: eversince-ugc-ads
description: "A creator-style video ad, to camera or over footage, from real takes or generated. A set of variants: ad variations."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-ugc-ads"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A UGC ad cannot look like an ad. A produced one fails whatever else is right; a rough one with a real first line works. One creator, one argument, a phone's picture, 9:16, usually fifteen to forty seconds. For a set built to test arguments, the six angles, the hook, proof and close, and what may be claimed, read `skill-ad-variations`. For a still that sells, `skill-image-ads`. A brief, a reference or a brand from the user outranks every default here.

## The plan

Real takes: the first plan. No footage: the generated plan after it, its cost said first.

### From real takes

The takes exist as items; hook, script and beats are chosen from what was said.

1. `transcribe_media` {ref: a take, format: "phrases"} per take: the lines, with their spans (refs only reads words already stored)
2. `manage_timelines` {create: {name: "<product> · <angle>", aspect_ratio: "9:16", clips: [{ref: a take, source: [start, end]} per beat, in order]}}
3. `auto_reframe` {timeline_id: from step 2}, where a take is horizontal
4. `remove_silence` {timeline_id: from step 2} (the clips carry the words step 1 read); `get_timeline` for the length against the target
5. `captions` generate {timeline_id: from step 2, template: "Social"} unless the user says no captions; `duck_music` {timeline_id, ref: the track} where they asked for music
6. `check_timeline` {timeline_id: from step 2}
7. `publish_preview` {timeline_id: from step 2}: returns the preview the ad already has, its app_url the link to hand over; `render_timeline` {timeline_id, idempotency_key} for the file an ad platform takes

### Generated, with no footage

The cost of the takes is said first (`estimate_cost`). The creator is made once as a still, then `generate_video` {type: "image-to-video", source_refs: [the creator still], prompt: each beat's line and action in the model's own form, settings: {duration, aspect_ratio 9:16}, idempotency_key} per beat, `get_jobs` for the items, which then go into step 2 as the clips with steps 4 to 7 the same.

## What the job needs

- Whether footage exists. It decides the branch: cut real takes (a creator's raw takes, a customer's clip); with no footage, generate the ad (credits), build it from stock, or wait for takes. Generated is as common and as good when done well. With a real creator, the person, room and register are given; the work is finding the line that already works in the takes. The shot plan and model notes below are for the generated branch only.
- The product, who buys it and the words they use: reviews, comments, what the user knows. You can offer to read what competitors run in the ad libraries (`pull_posts`, credits). In a generated shot, the product goes in as a reference image in `reference_refs` where the model takes one, or as its own B-roll still over the line that names it.
- Two facts before the first decision: the outcome a buyer gets, as something that changes in their day, and the objection that stops them believing it.

## The angle

Before the creator or any script, decide the argument: the reason to act. The angles that differ in kind: pain (the problem, sharp and specific; the product ends it), outcome (life after the change), proof (someone it worked for, a result shown happening), switch (this against what they use now, on one difference), mechanism (why it works when others did not) and deal (the offer leads). Match the angle to what the audience already knows: unaware of a solution, pain or outcome; let down before, mechanism or proof; choosing between options, switch; knowing the brand, the deal. One ad argues one angle. Testing several, and what may be claimed, is `skill-ad-variations`.

## The order

Creator, angle, shape, hook, script, shot plan, generate, assemble. Settle each before the next. The commonest cause of a mediocre UGC ad is a generation prompt written before deciding who is talking and why anyone would listen.

## Creator

The viewer believes the creator or not in the first second. Keep them identical across every shot: age within a couple of years, place, wardrobe, room, and their relationship to the product (someone who owns it and is annoyed nobody told them sooner is a different ad from someone opening it for the first time). The fatal mismatch is a life that does not plausibly contain the problem: a spotless kitchen selling a stain remover.

## Shape

Pick one; a hybrid reads as neither.

- **Talking head.** Face to camera throughout. Trust; one strong line can carry it. The costliest to generate: the face has to hold and stay in sync.
- **Voice over b-roll.** The creator's voice over product footage, the face never seen or seen once. Reliable, no lip sync to hold; less intimate.
- **First use.** Unboxing, setup, the first time it works. For value visible on contact.
- **Problem rant.** The complaint for most of the runtime, the product arriving late. For problem-aware audiences.
- **Before and after.** The state change is the argument, only as far as the product really goes.
- **Comparison.** This against what they use now, on one defensible difference.
- **Reply.** Answering a question or a comment. It starts mid-conversation, the most native opening there is.
- **Over the screen.** The creator's voice over what they do in the product. Needs the real recording or interface.

## Hook

The first two seconds decide whether anyone sees the rest. Write the hook before the script: no greeting, no introduction, no wind-up; the sentence is already running when the viewer joins. Name or signal the plain category (the audience has never heard the brand's name for its thing) and open on a situation, not on the viewer. Two cold tests: with the picture covered, a stranger knows what this is about and that it is for them; and the next few seconds pay it off, or the hook completes on its own.

Hooks that keep working in ad accounts: a pain called out in the buyer's words ("my skin hated every SPF until"), a contrarian line ("stop buying protein bars"), a specific number ("I spent $400 on mattresses before"), a reply to a comment, and a visual interrupt (the product doing something unexpected in the first frame). Keep the spoken hook under about twelve words. The text on screen in the first seconds can say something different from the voice, which gives a second hook to the people watching muted.

## Delivery and script

Keep one register through every line: unimpressed for a product that sells on function; evangelical for genuine surprise (it burns out on a small claim); conspiratorial for a shortcut others don't know; exasperated for a long-standing problem; deadpan for a category sick of being sold to, only when the copy carries the joke.

Write speech, not copy read aloud: contractions, an unfinished thought, a sentence that starts over. The start is the hook. The middle is the one thing that makes the claim believable, usually a specific moment rather than a reason. The end lands and stops: no slogan, no sell. Time it at speaking pace: about 2.5 words a second for a real person, 2.2 for a generated voice, so fifteen seconds holds 33 to 37 words. Say one thing, and leave out what the buyer can already see. A common timing that works: the hook in the first 3 seconds, the problem to about 8, the product shown working to about 20, the ask in the last 3 to 5. TikTok ads tend to do best at 15 to 25 seconds, Meta at 15 to 30. The table of default lines against their real counterparts is in `skill-image-ads`.

## Shot plan, generated branch

One generation per beat, each holding one line or one action: what is on screen, what is spoken, how long. A single unbroken take often reads more authentic than a cut sequence, and some models hold a long multi-shot better than a chain of short ones. `list_models` says what a model does well and whether it takes shots as a list or in the prose.

Keep lettering out of the picture. Baked-in text cannot be retimed, restyled or translated, and a hook has to read against a moving frame, so add anything that must read in the edit. Use phone camera language: handheld, arm's length, slightly low and off-center, the small drift of a hand. No gimbal, no rack focus, no move that needed equipment.

## Generating it

- **Lock the creator before any shot.** Make them once as a still and carry it into every shot as the start frame or in `reference_refs`, whichever the model takes (`list_models` says; some refuse the two combined). A face regenerated per shot is a different person per shot, and the room and wardrobe drift as easily as the face.
- **The model performs the line.** Native audio is what makes a talking head read as a person: put the spoken words in the prompt the way the model's guidance asks. About 35 words fill 15 seconds at a natural pace; a line longer than its clip comes out rushed and loses lip sync. Lip sync holds best on a face toward the camera, mouth uncovered, little head movement, a simple background. Where voice and picture are separate (voice over b-roll, over the screen), `generate_audio` voiceover reads the script with a voice from `list_models` type voice. Use the same voice id for every ad that creator carries.
- **What holds across models.** Quality anchors ("8k, HDR, shot by") push faces into editorial polish; realism markers do the work: skin texture, hair strands, fabric. Describing the face in words each time drifts it and can trip filters; let the still carry it. Naming an unwanted thing ("no extra fingers") often renders it; describe the wanted state. Negative prompts, unasked subtitles and unstated ethnicity behave per model, in its guidance.
- **An ad longer than one generation:** cut it at a beat into parts, never mid-sentence, since a sentence stitched across two takes is the commonest tell. Each part's prompt repeats the creator, the room and the light word for word, with only that part's lines. Re-describing them in other words recasts the person.
- Across a batch, vary the creator, room and shape: viewers spot the same synthetic person saying the same thing in a different order faster than they spot any single ad.

## Worth knowing for a generated creator

In the US, the FTC's rule on reviews and testimonials treats an AI person speaking as a customer who
used the product as a fake testimonial; an AI presenter delivering the brand's own script reads as a
dramatization. TikTok asks for an AI label on realistic AI content in ads, and some places require
one by law (New York for synthetic performers in ads). Tell the user which way the script reads, so
they choose the framing.

## Assembly

`manage_timelines` create with `aspect_ratio` 9:16 and the takes as `clips` in beat order, in one call. The timeline's ratio is its own, not inherited from the clips, and a horizontal clip is center-cropped until `auto_reframe` frames it on the person. Uploaded speech gets its words with `transcribe_media` timeline_id before any word tool runs. `remove_silence` takes out the dead air (real speech without its pauses is the sound of a posted video), then `remove_words` the fillers.

Add B-roll where the ad needs it (a product shown, a result, a step), not by habit. Two ways: **intercut**, the b-roll replacing the face on the same track a few seconds at a time with the voice running under it, the native default since phones edit with hard cuts; or **stacked split**, face and b-roll sharing the frame with `apply_layout` split-v, for when the viewer needs the reaction and the thing at once. For a split, cut the a-roll at the span's edges first (splitClip) and lay out only the overlapping pieces. Either way, mute the b-roll (updateClip volume 0), and cut away from a generated face every few seconds: held long, it invites inspection.

`captions` generate (the Social template, burned in) unless the user says no captions: most viewers watch silently. Music, where the user asks for it, sits low under the voice (`duck_music` with `ref`: the user's track or a `search_stock` one the user picked), a trending-sounding bed rather than a polished score. A rant or a reply often plays better with none.

## The check

One `check_timeline` reads the cut; no findings means it passed. It does not read text inside a generated shot, or whether a take is the right one. Judge a generated take by its look when it lands (`inspect_media`). Draw the cut with one `inspect_timeline` with `sheet: true` when a finding names a clip, the user asked for the work to be verified, or nobody sees the ad before it runs.

- **Sound off.** It looks like something a person filmed, not something a brand made. If not, fix the room and the camera.
- **Full screen, on a phone.** A generated face and mouth judged at full size, the way the ad plays; a mouth that looks right small can look wrong full screen.
- **Picture off.** It sounds like a person talking, not copy being read. If not, fix the register and the line lengths.
- **The first two seconds alone.** A stranger stays.

Trace a failure back to the decision that caused it, and rebuild the prompt whole from the decisions.

For a last pass over the whole piece, see `skill-finish`.

## Handover

- One ad: 9:16, fifteen to forty seconds, unless the placement sets another shape or length.
- Captions (`captions` template Social) unless the user says otherwise: a feed plays it muted first.
- The voice at delivery level on `check_timeline`; music, if any, under it.
- The ad is in the app from its first media, where it plays and is reviewed; the result's app_url is the link the user opens. `render_timeline` makes the file an ad platform takes.
- Also possible: music under the voice, variations on the hook or the angle (`skill-ad-variations`), a still version (`skill-image-ads`), other placements' shapes, a version in another language, what competitors run, read from the ad libraries.
