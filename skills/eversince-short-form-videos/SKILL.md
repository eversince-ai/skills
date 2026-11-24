---
name: eversince-short-form-videos
description: "A Reel, Short or TikTok around one idea: the shape, beats, safe area, sound and rights, assembly, checks. A video that sells: UGC ads."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-short-form-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Nobody owes a short their attention, and the next video is one flick away. A short has to pay off in the first second and keep paying to the end. Build it around one idea, in the platform's shape, and make it as long as the idea needs and no longer. For writing the hook and script, use `skill-hooks-and-scripts`; for a piece that sells, `skill-ugc-ads`; for a moment lifted whole from something long, `skill-clipping`. A brief, reference or style from the user overrides every default here.

## The plan

1. Settle the one idea, the platform, and the material: the user's footage, stock, generation or a mix
2. Get the script: the user's, a short one you write here (a few beats), or a longer one with `skill-hooks-and-scripts`
3. Pick the shape and hold it for the whole piece
4. Lay the beats on a 9:16 timeline, strongest picture first
5. Add sound, captions and on-screen text inside the platform's safe area
6. Run the check, then hand over the short in the app

## What you need to know

- What the short is about, in the user's words, and what they want it to do.
- Who it is for and where it will be posted.
- What footage already exists. Shorts are cut from existing material (a recording, an event, a stream) as often as they are generated. Read a long source with `transcribe_media` and look at it with `inspect_media` before writing any beats: the moments that exist decide the beats.

## The platforms

- **Frame:** 1080 by 1920, 9:16, 30 frames a second (60 for fast motion). One master serves TikTok, Reels and Shorts; 4:5 or 1:1 is for a feed post.
- **Length:** Shorts and Reels run up to three minutes, TikTok longer. A 2026 study of 3.3 million posts (viral.app) found the median short at 16 to 19 seconds. Within an account, TikToks of one to three minutes did slightly better than shorter ones, Reels showed no effect of length, and Shorts over a minute did about a fifth worse. So keep it as long as the idea, and under a minute for Shorts unless the content holds.
- **Safe area:** each app puts its buttons, caption and handle over the bottom and right of the frame, more as the caption grows. No platform publishes exact figures for posts. Captions and positioned text on a 9:16 timeline already sit clear of the top 12% and the bottom 24% of the frame (230 and 460 pixels at 1920); keep faces and anything placed by hand inside the same band, and about 110 to 160 pixels in from each side. A long caption on the post covers more of the bottom, so what must be read sits higher.
- **Cover:** the platform uses a frame or a chosen cover. Instagram's profile grid crops it to 3:4, so put a cover's title in the middle 1080 by 1440.
- **Originality:** Instagram stops recommending accounts that mostly post other people's content, and a border, speed change or caption does not make it theirs. Another app's watermark lowers reach on Instagram and TikTok (the user's own logo is fine). For several platforms, export clean from one master; never re-upload a file carrying another app's mark.

## The hook

The first second decides whether the rest is seen. The picture, the first words and any text on screen land together and open a question the short then answers. Start inside the interesting part: no intro, no context, no name, no logo. Choose the first frame, since it plays before any sound where a feed is muted. A spoken hook starts mid-thought; a text hook is one line, read at a glance. The test: a stranger who sees only the first second knows what they get by staying, and wants it. Several hooks written and compared, or a longer script, is `skill-hooks-and-scripts`.

## The shape

Pick one and hold it. Each shape needs its own timeline.

- **Talking head:** one person to camera. Cut the pauses, punch in where a point lands, add B-roll where a line names something concrete (`skill-talking-head`).
- **Voice over picture:** the voice track comes first, the pictures are cut to its phrases, music sits under it.
- **Text-led:** on-screen text carries it, one line per beat, over footage or a loop. Hold every line long enough to read. It works on mute by design.
- **Before and after:** a split or a wipe. Keep the before brief and hold the after.
- **Process:** something made or done, sped up. Show the result first and label the steps.
- **List:** a counter stays on screen, and each item is a beat that stands alone.
- **Skit or point of view:** a scene played to one joke or one truth, with a point-of-view line at the top of the safe area. It dies past its punchline.
- **Montage:** the cuts carry it, on the beats of the track (`detect_beats`). No voice.

## The beats

The beats are the short written down: what is on screen, what is said, what text shows, and for how long. For generated material, write the beats first and generate to them. For found material, read it first and cut any beat no moment supports instead of hunting for one. Lead a tip, claim or result with its payoff; few people see a payoff saved for last. A day, a trip or an event can run in order, with its best moment shown first.

Change the picture when the information changes, not on a timer; no study backs a fixed cut rate. A line of text holds for about eight seconds at most unless something inside it changes. End on the payoff, with no end card and nothing after it. Where the content allows, make the last frame lead back into the first so it loops: replays count as views on all three platforms.

## Generated beats

Generation costs credits, so price it first (`estimate_cost`). The chosen model's guidance in `list_models` says how it takes references, whether it does multi-shot, and whether it burns in subtitles unasked. Across models:

- Anchor any beat that must match a character, place or look to a frame, a reference or real footage (source_refs, reference_refs). Use text-to-video only for beats that need to match nothing.
- Carry the same references across every beat that shares a person, room or light, or each generation becomes a new world.
- Keep lettering out of the picture and add the words in the edit.
- Viewers leave during dead seconds between payoffs, so direct movement inside the take or cut the take.

## Sound

Design for sound and keep it readable on mute: TikTok is mostly watched with sound on, Reels and Shorts more often muted.

- **Voice** at delivery level (`level_audio`).
- **Music** only when the shape needs it: the user's own track, then one made to fit the cut exactly where the user has credits (`skill-voiceovers-and-music`), a stock track only where the user picked it, otherwise none (`duck_music` with `ref` places, levels and ducks any of them in one call; `search_stock` kind sfx for an effect). Where a bed starts and how it fades is in `skill-sound-design`.
- **Trending sounds** are added in the platform's own app when posting; Eversince's stock does not include them. Original audio did as well as licensed music in 2026 data, so a trending track is optional.
- **Rights:** a business account on TikTok and Instagram can use only the platform's commercial library. A Short over a minute with music that draws a Content ID claim is blocked, so keep music from outside YouTube's Audio Library to Shorts under a minute.

## Assembly

Call `manage_timelines` create with `aspect_ratio` 9:16 and the beats as `clips`, in one call. Ask for every generated beat at that shape. A horizontal clip is cropped to its middle, or to a person already found in it, until `auto_reframe` follows the person. Put a moment from a long source on with its span as source, taken straight from `transcribe_media` phrases or what `inspect_media` returned. For uploaded speech, get the words with `transcribe_media` timeline_id first, then run `remove_silence` for dead air and `remove_words` for fillers, in that order.

Where the short speaks, `captions` generate burns the words in, inside the safe area and never over a face (`skill-animated-captions`). Ready graphics for a short (a speaker layout, a name, a point card, a headline over the clip) are on the shelf: `get_templates` {query} finds them and `compose_motion` template places one, its words and media set in `values` (`skill-motion-overlays` for which and where). One block of words is on screen at a time: a graphic that carries words sits where nobody speaks, or the caption lines under it are removed for its length (`captions` rewrite lists each line's clip_id; `removeClip` takes it). Where it does not speak, add on-screen lines as text clips with `edit_timeline`, and put a line over bright or busy footage on a backgroundColor panel.

To close a loop's seam, compare the last frame with the first using `inspect_timeline`. If they differ, start the main clip half a second later (trimClip its start). On a track above, add the same source from its original in-point to cover the last half second (addClip with that trimStart), with its opacity keyframed 0 to 1, so the final frame is the frame the loop starts on.

## From a template

The shelf holds finished shorts as templates. `get_templates` kind timeline lists them; called with an id, it lists that template's holes, each with its length and what its sample showed. `manage_timelines` create with `from_template` and `fill` copies the cut, music, styling and transitions with the user's items in the holes, in one call. Words drawn by a graphic are its fields, not holes: set them after create with `edit_timeline` updateClip {clipId, patch: {values}}, each line written for the length its sample was cut for. Choose footage per hole: a moment whose span fits the hole's length, the strongest picture on the first hole, a moving shot where the sample moved and a still where it held. The response names any hole left unfilled; it keeps the template's sample until you fill it.

## The check

Run `check_timeline` once on the cut; no findings means it passed. It does not read text burned into the footage or judge whether a moment is the right one, so check these on the words (`get_transcript`) and, where needed, on frames:

- **On mute:** it still delivers its one thing through captions and picture alone.
- **First frame:** it is the hook itself, with no black or slow lead-in, and it works as the cover.
- **Second one alone:** it opens a question a stranger wants answered.
- **Safe area:** no text, caption or face under the platform's buttons.
- **Leave test:** viewers leave at the first moment that gives attention nothing new. Cut to it or cut it out.
- **Words:** names, terms and numbers spelled right; the last caption gone by the final frame.
- **Music:** cleared for the account and the length.

Look at frames (`inspect_timeline` with `sheet: true`) in four cases: a finding names a clip, the look is all there is to judge (no speech, a loop's seam), the user asked for the work to be verified, or nobody will see it before it is used. Fix a failure at the beat that caused it. For a last pass over the whole piece, use `skill-finish`.

## A series

A recurring format makes each next short faster to make and easy to recognize. Plan the fixed parts before the first one: the shape, the look, the caption style, and recurring characters and places as the same references. Once a run is built, keep it as a template (`manage_timelines` template) and make each next one with `manage_timelines` create with `from_template` and `fill`. Instagram's linked Reels and TikTok's playlists can join the parts on the platform.

## Handover

- A short at 9:16, captioned where it speaks unless the user says otherwise, the strongest moment in the first second, everything inside the safe area.
- It is in the app from its first media, where it plays. The result's app_url is the link the user opens, and `render_timeline` makes the file to post.
- You can also offer: a cover frame (`extract_media` frame, chosen to read in a 3:4 grid), versions for other platforms (`skill-content-repurposing`), a version in another language, a series built from it as a template, music or sound effects.

## An example plan: one short from a long recording

1. transcribe_media {ref: the recording, format: "phrases"}; an item over five minutes returns a job, get_jobs waits on it, and the same call then returns the words
2. manage_timelines {create: {name: "<idea> · short", aspect_ratio: "9:16", clips: [{ref: the recording, source: [start, end]} per beat in order]}}
3. auto_reframe {timeline_id}
4. remove_silence {timeline_id}, unless the user wants the pauses kept; get_timeline for the length after it, a beat trimmed or dropped where it runs long
5. captions {action: "generate", timeline_id, template: "Social"}, unless the user says no captions
6. level_audio {timeline_id}
7. check_timeline {timeline_id}
8. publish_preview {timeline_id} → the link to hand over
