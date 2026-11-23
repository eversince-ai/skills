---
name: eversince-animated-captions
description: "Word-timed captions styled to fit the video: the look chosen from it, lines, timing, emphasis, readable over any footage."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-animated-captions"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Captions are how most feed viewers meet a video with the sound off: every word the speaker says, in lines a reader would have chosen, readable over whatever the footage does, and styled so they belong to this video. The style is chosen from the video, not from a favorite template. A brief, a reference or a brand from the user outranks every default here.

## The plan

1. Read the video: its energy, its footage, the brand, where it plays
2. Choose the look: a built-in template, adjusted with `style`; a styled caption as a motion graphic only when asked for (below)
3. Check the words: misheard names, invented lines, fillers
4. Generate with the line length and timing the talk needs
5. Make it readable over the footage, clear of faces and the platform's buttons
6. Check, and hand over

## Choosing the look

Read the video before choosing: how fast the person talks and the cuts come, how bright or busy the footage is, whether a brand kit sets a font and a color (`manage_brand_kit`), and where it plays.

- **Energy:** fast talk and fast cuts can take words built or lit as they are said; calm delivery wants whole lines that hold still and fade.
- **Footage:** bright or busy footage, or a screen recording, needs a box; heavy motion under the words wants simpler caption motion, not more.
- **Brand:** a brand or premium piece uses the brand's font and its one accent color. Loud styles read as off-brand for business, legal, finance or luxury.
- **Faceless or over B-roll:** the captions carry the meaning, so they can be larger and nearer the middle. Over a talking head they stay smaller and off the face.
- **Platform:** LinkedIn and long-form YouTube lean plain; TikTok carries more energy.

The built-in looks, `template` on `captions` generate (`get_templates` kind caption shows each):

| The video | `template` |
|---|---|
| A talking head on a feed: still lines, the key words in one accent | Social |
| Podcasts, interviews, motivational talk, lyrics: each word lights up as it is said | Karaoke |
| High-energy business or educational shorts, the look people call "Hormozi style": heavy type built word by word | Kinetic |
| One large word at a time, for a fast montage or a beat-driven piece | One word |
| Interviews, documentary, anything long or landscape | Broadcast |
| Shorts and talking heads: bold outlined lines, the word being said on a color block | Highlight |
| Busy footage or screen recordings with word timing: lines on a dark box, the spoken word in an accent | Lit box |
| Brand films, explainers, calm talking heads: a clean sans whose key words turn to a larger serif italic | Editorial |
| Punchy shorts: bold lines whose key words step up in heavy capitals and an accent | Statement |
| Essays, brand films, documentary: designed, a sans set low on the left, each phrase's key word on its own line in a large serif italic | Lockup |
| Talking heads, explainers, launches: designed, words built in the middle, the key word below in heavy capitals | Center |
| Trailers, sport, hooks, hype: designed, small capitals around one key word in tall condensed capitals across the frame | Headline |
| Statements, quotes, bold brand shorts: designed, each phrase a centered block of heavy capitals, rows set to one width | Block |
| A speaker framed to one side, fashion, sport: designed, one word to a row down the left | Column |
| Busy footage, interviews, business talk: designed, each phrase on a slim white box | Box line |
| Essays, podcasts, calm talking heads: designed, a large left-set sentence, the words already said dimming | Sentence |
| Architecture, fashion, calm films: designed, small tracked capitals with one large light word | Minimal |
| Montages, music-led edits, brand films: designed, only the word each phrase turns on, a sans beside a serif italic, in the open space | Accents |
| Launches, fashion, confident shorts: heavy capitals filled with a champagne-to-white sweep, each word springing in | Gradient |
| Creator shorts and talking heads: the word being said on a white rounded pill | Pill |
| Explainers, tutorials, business talk: a bar drawing under the word being said | Underline |
| Hype edits and hooks: two or three condensed capitals landing hard in the middle | Slam |
| Calm, cinematic, wellness: each word sharpening out of a blur | Focus |
| Product, launch and tech shorts: bold words rising out of a mask | Rise |
| Lifestyle, food, travel, personal vlogs: large handwriting built word by word | Handwritten |
| Brand films, poetry, fashion, slow storytelling: large warm serif lines | Serif |
| Code, AI and developer content: mono type typed out on a dark box | Terminal |
| News-style, reaction and loud entertainment: dark type on a yellow box | Tag |
| Plain lines under a soft shadow, the default with no template | Subtitles |

`style` and `animation` beside a template change only what they name, so a template is a starting point you can push toward the user's brand. `save_template_as` keeps the look under a name, so the next video wears it.

Where styles are heading in 2026: the loud stack (several colors, emoji on every line, a pop on every word) now reads as template content to many viewers. The cleaner version keeps the word timing and drops the noise: a clean sans, white with a soft shadow, one accent word, a fade. Use the loud version when the user asks for it or the content is pure entertainment.

## Two kinds of captions

Caption lines from `captions` are cut from the transcript and keep its word timing: after a trim, a cut or a retake, `captions` generate runs again and the lines follow. A caption built as a motion graphic carries its own word times, written in from `get_transcript`; it is placed where it sits and does not follow a later edit, so it is placed again after the cut changes. Mixed styles inside the lines (a key word in another face, larger or in capitals) are the first kind: Editorial and Statement do it, and `emphasis.style` adds it to any look. The motion graphic is for type the lines cannot be: words stacked, scattered or placed around the frame.

The designed templates (Lockup, Center, Headline, Block, Column, Box line, Sentence, Minimal, Accents) are editor-made systems: one placement, two sizes and one entrance held for the whole video, the word each phrase turns on stepping up the same way every time, and each phrase held until the next starts. The key word is picked from the words; `emphasis` words, when named, are taken instead. `color` sets the running words and `emphasis.color` the key word; the faces belong to the template.

## Styled captions, as a motion graphic

The templates style a whole video one way. A styled caption, the kind editors design by hand (one word huge, another in italic serif or handwriting, words stacked and placed around the frame), is built as a motion graphic. Use the templates by default; build a styled caption when the user asks for that look, for a short where the type is part of the look, and work from a reference: one the user gives, or one you find. Rebuild the reference's lockups (how each phrase is broken, sized and placed) rather than inventing a style.

How it works here:

- `get_transcript` with `granularity: "words"` gives each word's `from_frame` on the timeline. Write the phrases and their word times into the graphic as data, and place it with `compose_motion` at frame 0 for the timeline's length, so the graphic's frames are the timeline's (`skill-motion-graphics` holds the code contract).
- Start each phrase fading in a few frames before it takes over, or the frame between two phrases is empty.
- Check the source for captions already burned in: `inspect_media` overview has no flag for them, so look at its frames. Choose the position from a real frame, off the face.
- Draw frames at a few phrase changes (`inspect_timeline` with `seconds`): its notes flag text that is cut off, over a face or too faint.
- Keep a look with `compose_motion` `save_as`; for the next video, place it with `template` and swap only the words with `source_edits`.
- A dub or translation does not rewrite words inside a graphic, so a video headed for other languages uses the captions tool.

## The words

Read before generating:

- `inspect_media` overview of the source: no tool flags burned-in text, so look at the overview's frames. Burned-in subtitles or heavy on-screen text often start partway in, and a second caption system over the first reads worse than none.
- `get_transcript` sentences: speech under loud music, a strong accent or near silence can come back as fluent nonsense, and a line of invented words is worse than no line. An empty result usually means untranscribed, not silent.
- Names, products and terms: the workspace keeps spellings under `manage_brand_kit` words, with `heard_as` for what a transcript returned instead.

`captions` generate transcribes uploaded clips that have no words. A subtitle file the user already has is the transcript to trust: `captions` import keeps its cues as written.

Fillers: the feed looks (Social, Kinetic, Karaoke, One word) leave "um" and "uh" out of the lines while they stay in the sound; `skipFillers` in `style` sets it either way on any look. A word leaves the video itself only through `remove_words`, and only when the user asked for the cut to change. Small words ("a", "it", "so") stay; tidying them breaks how the person talks.

## Lines

The generator breaks lines at sentence ends, cuts between clips, pauses over 0.7 s, and phrase and clause boundaries, within `maxWords` and `maxChars`. What it cannot know is energy, which is the `maxWords` ceiling:

| The talk | `maxWords` | `maxChars` |
|---|---|---|
| A hook, a punch line, high-energy social | 2 to 3 | the template's |
| Conversation, a talking head for a feed | 3 to 5 | the template's |
| Calm narration, documentary, an explainer | 4 to 6 | the template's |
| Subtitles on long-form or landscape | 6 to 7 | 32 to 42 |

A template brings its own `maxChars` (fitted to the frame's width) and `closeGapMs`, and they apply unless `style` sets them: Social applied `maxChars` 30 and `closeGapMs` 400 in one run. `maxChars` 0 removes the cap, and is the default only with no template.

Lines chunked by phrase are easier to follow than single words flashing one at a time: in eye-tracking studies, word-by-word reveal is the format that pulls the eye off the picture most. A name or fixed phrase reads as one unit. Where two voices share a line (`get_transcript` with `speakers: true` shows the turns), or a line breaks against the sense, `captions` rewrite with `regroup` moves the breaks with each word's time kept; run it after the last generate. Avoid a single word alone between two pauses; a higher `maxWords` merges it with a neighbor. The generator can still make lines of one or two words and split a name across two lines, so after generating, read the lines (`captions` rewrite with no lines) and fix them with `regroup` or a rewrite before handing over. For subtitles, 42 characters a line and two lines at most is the Netflix measure, read at up to about 20 characters a second.

## Timing

A line comes in just ahead of its first word. `closeGapMs` holds it through a pause to the next line: about 500 for subtitles and conversation, so text does not blink off between sentences; 0 ends every line on its last word, where a silence should read. Lines hand over to each other, so they fade in and not out: `fadeInFrames` about 0.15 s at the timeline's rate, `fadeOutFrames` 0. Start the speech 0.1 to 0.3 s in, and have the last line gone before the final frame. In Karaoke and the built looks, a highlight early or late is very visible, so a clip whose words drift is read again (`transcribe_media` with `timeline_id` and `clip_ids`).

## Readable over the footage

Never grade, darken or blur the footage for a caption; treat the text. `inspect_color` at a moment where a line sits gives `mean_luma` (0 to 1) for the frame; the same frame on `inspect_timeline` shows what the average hides, like a bright window behind the words.

| Behind the line | Treatment |
|---|---|
| Dark, `mean_luma` under about 0.25 | light text, `shadowEnabled` |
| Middle, about 0.25 to 0.7 | `strokeEnabled` at `strokeWidth` 2 in near-black, plus the shadow |
| Bright and even, over about 0.7 | near-black text, `color` near #111, no box |
| Bright and busy, over about 0.7 | a box: `backgroundColor` near-black at `backgroundOpacity` 0.45 to 0.6 |

The box is one rounded shape hugging each line, not a bar across the frame. Try a stroke and shadow before a box. A template carries its own treatment; this table is for a hand-set style.

## Type

- One family, one or two weights; hierarchy by weight and size, not a second face. Any family in the catalogue (`get_templates` kind font); bold sans like Montserrat at 700 to 900 is the common social choice, a condensed display face like Anton for short uppercase chunks, the brand's font where there is one. Subtitles at 500 to 600.
- `fontSize` in the timeline's pixels: running lines at 3.5 to 5% of the frame's height (40 to 55 on 1080 tall, 65 to 95 on 1920 tall). Seven to nine percent is hook size, for two or three words only.
- Uppercase suits chunks of one to three words; longer lines read faster in sentence case.
- One accent per piece, `activeColor`, from the brand or the footage.
- Italic only as a change of face: a serif italic key word inside a sans line, at least 1.2 times its size (Editorial). The line's own face slanted reads as slanted over moving footage, not stressed.

## Emphasis and animation

Every word is plain, emphasized or a peak. About 70% plain, 20% a slight lift, 8% emphasized, 2% peak: when everything is emphasized, nothing is.

- **Emphasis:** the one or two words a phrase turns on, in at most a third of the lines and one word in ten. `emphasis` in `style` (on `captions` generate; its own field on `cut_clips` and `edit_clips`) is `{words, color, emoji, style}`: a named word stands in that color every time it is spoken, with or without animation. `style` sets those words apart further, `{fontFamily, fontWeight, italic, scale (1 to 1.6), uppercase, ownLine}`; `ownLine` stands the word on a line of its own between the plain words, a lockup, at a scale up to 2.4, with one key word to a phrase (`font_family` and `font_weight` on `cut_clips` and `edit_clips`): one other face for them at most, from the catalogue, and a scale of 1.2 to 1.35 so the line keeps its rows. Editorial and Statement carry a style already, so only the words are given. Coloring by meaning (green for good, red for bad) belongs to the loud entertainment look only.
- **Emoji:** only when the user asks. `emoji` writes one after a named word, `{"launch": "🚀"}`; one or two in a clip, on a word it pictures.
- **Word animation** (`wordAnim`): "highlight" lights each word in `activeColor` as it is said, "fill" dims the line and fills each word in, "reveal" builds the line word by word, "pop" jumps each word up from 60% of its size; "blur" sharpens each word in, "slam" lands it from larger, "wipe" raises it out of a mask, "glow" lights the spoken word in the accent over a dimmed line, "underline" draws a bar under it, "pill" sets it on a rounded pill. `textTransform` uppercase and `letterSpacing` (em) in `style` set capitals and tracking on any look. Highlight and fill are a reading aid for fast talk, not emphasis, and sit wrong on documentary and interview work; reveal suits a hook.
- **Line animation** (`lineAnim`): "typewriter" suits code and interfaces; "pop" overshoots on a spring. A caption that bounces on every line reads as a template, so keep bounce for high-energy pieces.

## Where the line sits

Lower third, centered, one line at a time, over no face and no text the footage already carries. On 16:9 and 1:1, `position` `bottom-center`. On 9:16, `bottom-center` already sits clear of the bottom quarter, and generate lifts the lines further for the platforms the timeline names (setMeta `platforms`) and off a face once `auto_reframe` has followed the subject. Over a talking head, look at one frame (`inspect_timeline` with `seconds`) and move `position` if a line covers the face or mouth: `check_timeline` reads text over a face only on clips whose faces were read, and says so ("read on 0 of N framed clips"). A close-up with the chin low takes `top-center`; `center` is for the two or three word hook looks and faceless pieces. Black bars from a landscape source on a vertical frame are caption space. Keep other small text (a URL, a label, a legal line) out of the caption's band; text burned into the footage is not read by any tool, so choose placement from the source's frames: `position` takes top-, center- or bottom- with left, center or right, or `center`.

## The peak, behind the person

A peak word (what the beat builds to, two to four words) can sit behind the person: a text clip or a `compose_motion` graphic between the shot and a cut-out copy of the person (`skill-video-effects`). At least twice the line's size, held at least a second once it lands, arriving on an ease-out rather than a bounce, clear of the line's band. One per beat, never two visible together.

## Fixing a word

`captions` rewrite with no lines returns every line with its id and the captions' `language`; with lines and the corrected text it writes them back into their slots, timing kept. A rewrite with lines also needs `language`, the language the lines are written in (for a fix in the same language, the captions' own language); without it the call is refused. A word the speaker did not say is cut with `remove_words` instead, which changes the cut. A restyle that keeps lines placed by hand: `edit_timeline` updateClip with the caption track as `trackId`; `captions` generate replaces them all.

## Other languages

A language version is a tab beside the timeline (`captions` rewrite or translate with `as_new_timeline`). A longer language can outgrow its slot: frames of the longest lines show any running past two rows, and the fix is `captions` generate again at a lower `maxChars` before translating. Check right-to-left languages and languages without spaces on frames first. A spoken version is `skill-translation-and-dubbing`.

## The check

One `check_timeline` reads every line's placement and size: no findings means it passed. It does not read what a line says: `captions` rewrite with no lines returns every line, read for names, terms and numbers. Draw frames when a finding names a clip, the look alone is the judgement (a hand-set style over bright footage, a peak behind the person, a source with its own text), the user asked for it to be verified, or nobody sees it before it is used: `inspect_timeline` with `sheet: true`. Every line should read over the brightest thing behind it.

## Handover

- The video with captions in the app (`publish_preview`), with the check's result.
- Where captions leave as a file (a YouTube subtitle upload, a player that takes one), `captions` export gives SRT or VTT.
- You can also: keep the look for the next video (`save_template_as`), add captions in other languages (`captions` translate, in credits), put a peak word behind the person.

## An example plan: a talking head for a feed

1. The timeline the video is on, or manage_timelines {create: {name, clips: [{ref}]}}
2. inspect_media {ref, mode: "overview"}, its frames looked at for burned-in text; get_transcript {timeline_id} to check the words
3. captions {action: "generate", timeline_id, template: chosen from the video, style: {maxWords, emphasis: {words, color} where the user wants highlighted words, the brand's fontFamily and activeColor where there is one}}
4. captions {action: "rewrite", timeline_id} to read the lines; then {action: "rewrite", timeline_id, regroup} for bad breaks, or {action: "rewrite", timeline_id, lines, language: the captions' language} for a wrong word
5. inspect_color {timeline_id, at_seconds} only for a hand-set style; inspect_timeline {timeline_id, seconds} on one frame to keep the lines off the face
6. check_timeline {timeline_id}
7. publish_preview {timeline_id} → the link; render_timeline {timeline_id, idempotency_key} for a file
