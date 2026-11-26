---
name: eversince-translation-and-dubbing
description: "A finished video in another language: a dub in the same voices, or subtitles, fitted to the cut and checked. Lines alone are captions."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-translation-and-dubbing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A language version is a new performance. The words change length, the read changes rhythm, and
everything the cut was built around (the beat a line lands on, the pause before a reveal, the duck
under a sentence) has to hold with the new words in it.

Each language is its own version beside the master, and the master stays untouched. A timeline
version is a tab beside the master; `change_language` and `captions` with `as_new_timeline`
make it that way. In the app, each version plays as its own
tile on the master's preview, after the master: `publish_preview` with a version's id returns that preview
and names the tile. A brief, a reference or a brand from the user outranks every default here.

## The plan

1. Read everything that carries words: captions, speech, text on screen, slides, words inside pictures
2. Settle the terms first: names and brand words that stay as they are, how units, currency and dates are written for the market
3. Subtitles, a dub, or both, as the user asked; if they said nothing, translated captions, with the dub offered at its price
4. Fit the new words to the picture, and the subtitles to the language's reading speed
5. Check each version, then hand it over in the form the platform takes

## Where it goes

- **YouTube** takes a dub as an extra audio track on the same video, so viewers pick their language and the views stay on one video. Render the dubbed version's sound (`render_timeline` format mp3 or wav) for the user to upload as an audio track. YouTube reports that creators using these tracks get over a quarter of their watch time from the added languages.
- **Instagram, TikTok and others** take a separate post per language: the version's own video.
- **Subtitles** are burned in on a feed cut, or delivered as a file (`captions` export, srt or vtt) where the platform takes one.
- Translate the title, description and thumbnail text too. An untranslated title hides a translated video.
- If the dub uses a cloned voice, the user may need to label it as AI-generated (the EU requires this from August 2026, and Meta labels its own AI dubs).

## What carries language

Start by reading the video for everything that carries words. Each kind changes by its own route:

- **Captions and subtitles.** Free when you write the translation. `captions` rewrite with no lines
  returns every line with its id and limits; rewrite with `lines`, `language` and
  `as_new_timeline` writes the translated lines onto a new copy, keeping slots and word timing; on a copy you already made (as the plan below does), leave `as_new_timeline` out, or you get two copies. A
  line past its limits is refused, so write each one to the limits the read returned. `captions`
  translate does the same with a model, for a small token charge in credits. For a platform's
  subtitle file, run `captions` export on the copy.
- **Speech.** `change_language` dubs, in credits: every speech clip with words and a voice is
  translated and re-recorded in the same voice. A person on camera keeps their picture, lips
  unchanged, with the translated take under it, so a talking face reads as dubbed. A voiceover, a
  podcast or narration over pictures has no lips to miss. A sound-only recording (a voiceover, an
  episode) goes on a timeline first and is dubbed there.
- **Text on a timeline.** Text clips, a graphic's text controls and chapter markers: the dub
  translates them. Without a dub, use `edit_timeline` updateClip content per text clip, written by
  you, free.
- **A still or a slide.** A single picture gets a slide per language on the same canvas
  (`edit_canvas` addSlide {copyOf}, then setSlide {slideId, patch: {name: the language}}). A set of
  slides gets a canvas copy per language (`manage_canvases` create with from_canvas, named
  "<name> · <Language>"). Then `edit_canvas` patchLayer on each text layer with the translated
  words, free. Fit a longer language with size or line breaks on the layer, and check it on
  `inspect_canvas`.
- **Words inside a picture.** Packaging, a sign, an interface, words baked into a generated frame:
  remake and replace them (`generate_image` image-to-image on the picture, credits, when the user
  wants it), or leave them in the original language.

Change what the user asked for: subtitles alone, a dub, a slide set, or all of it. Subtitles are a
copy with its caption lines rewritten; the dub is `change_language`, one job. `estimate_cost` prices
either. If the user said nothing, a video with speech gets translated captions, and you offer the
dub with its price.

## The spoken version

`change_language` runs on the timeline the work is on, whether the user's existing edit or one made
from an item (`manage_timelines` create with `clips`), and saves a new timeline named
"<name> · <Language>". It is one price for the whole dub and one job, charged when it starts and
paid back only if it fails before any voice work; `get_jobs` carries the new timeline id when it is
ready, and the result counts what was translated and names what was left.

A take that runs longer opens the timeline at that clip's end: later clips move and the picture
under it is held. `revoiced` gives each clip's seconds and loudness before and after, a warning
names any clip left running over, and timings inside one take do not move.

Get the words first: `transcribe_media` with `timeline_id`. A speaker with words but no voice (an
uploaded recording) gets a voice built from their own recording only when the user has confirmed the
speaker's agreement to the statement the `consent` field carries. Do not pass it on the user's
behalf without that. Without it, those speakers stay as they are and are named in `plan.needsVoiceIds`.

## What the dub does not do

- **Music ducking.** Ducks are volume keyframes timed to the old speech. Run `duck_music` again on
  the new timeline.
- **Words inside the picture**, and a graphic with no text control, as above.
- **A voice chosen per market.** The dub keeps the master's voices. `list_models` type voice lists
  other voices, with American, British and Australian English accents. Every voice speaks every
  language, but none is native to most markets, so pick the voice for its character and let
  `language` set the tongue. Then
  `generate_audio` voiceover per line, and `replaceSource` on the speech clip, which keeps the slot
  and shortens a clip longer than the new take.
- **Terms.** There is no glossary field. Decide the brand, product and feature names, and anything
  legal, before the dub (how the brand is pronounced, whether figures convert, how dates and currency
  are written for the market), and check them after. `captions` rewrite with no lines returns every
  line with its id; with lines and `language` (the captions' own) it writes corrections in place, free, slots kept.

## Writing the script by hand

- Write for the ear. The most common failure is a line that reads correctly and sounds awkward
  spoken.
- Length is the constraint. The same sentence runs longer in some languages and shorter in others,
  so write each line to the slot it has, usually by saying the same thing in fewer words, not more
  words faster.
- Pick one register per version: formal or familiar address, how directly the market expects to be
  sold to, whether the humor survives at all.
- Adapt instead of translating word for word: an idiom, a joke or a local reference becomes its
  local equivalent, and units, currency, dates and number formats follow the market.

## Fitting to the picture

The translated read will not match the original's length. Fixes, in order, each costlier than the
last:

1. Rewrite the line tighter.
2. Put a pace tag before the line on `generate_audio` voiceover (the voice model has no speed
   setting).
3. Use the air around the line, where the cut has a pause before or after it.
4. Change the picture, extending a held frame or retiming a beat. This comes last because it makes
   the version a different edit.

Nothing runs past the picture it belongs to. A line still speaking over the next beat is the fault
viewers notice without being able to name it. `seed` keeps a remade take close to the first, so you
can remake one line later with little drift.

## Subtitles in a longer language

A translated line keeps its original's slot. Slots of two or three words, as a feed look cuts them, cannot hold another language's word order (a verb that moves to the end, an article that joins its noun). So cut the copy's lines again at subtitle length before translating: six or seven words, 32 to 42 characters, about a clause a line. A longer language can still wrap past two rows; that check is in `skill-animated-captions`, Other languages.

Reading speed differs by language: Netflix allows about 20 characters a second for adults in English and French, 22 in Hindi, and 9 in Traditional Chinese. If a line reads faster than its language allows, shorten it; do not leave it on screen longer than the speech. Nothing checks this for you: divide each line's characters by its seconds (the rewrite read gives both). Short labels and graphic text expand most (a short German label can double), so check that translated text still fits its box.

## The check

One `check_timeline` covers every language (`timeline_ids`), speech level included, since a new
take is a new recording. `ok: true` is the pass, and each warning is still read. It does not read text burned into the
picture, so find words left in the original language on the master's own frames. `captions` rewrite
with no lines returns every line; read them as the argument, with the terms as decided.

Draw frames in four cases: a finding names a clip; only the look can tell (a script written right to
left, or with no spaces between words); the user asked for the work to be verified; or nobody will
see the version before it is used. One picture: `inspect_timeline` with `sheet: true`.

Nothing in the loop can judge whether a line reads naturally to a native speaker, so check what can
be checked:

- Read the dub's words back (`transcribe_media` with `timeline_id` on the new version) against the
  translation. A dropped line or a mispronounced name shows there.
- Names, numbers and brand terms are intact in every line.
- Translating a few key lines back into the original catches a changed meaning, though not an
  unnatural phrase. A native speaker's review is the real check, so offer one.

A last pass over the whole video is in `skill-finish`.

## Handover

- Each language: its own version (a tab beside the master timeline, a slide or a canvas copy), speech at delivery level, no line past two rows; in the app, a timeline version as its tile on the master's preview, with the app_url as the link the user opens; a file for use elsewhere.
- With each: the check's result and whether frames were drawn.
- Also possible: more languages from the same master, a subtitle file per platform, the dub with a native voice per market, the thumbnail and slides in each language, burned-in captions for a feed.
- Hand over a version that nobody who speaks the language has reviewed as a draft.

## The plan: subtitles in another language, free once the words exist (transcribing is cloud processing)

1. `manage_timelines` {create: {parent: the timeline the work is on}}: the copy, a tab beside the master, its language recorded with edit_timeline setMeta {note} (from an item, make the master first with {create: {name, clips: [{ref}]}})
2. captions {action: "generate", timeline_id: the copy, style: {maxWords: 7, maxChars: 42}} where it has none or its lines run two or three words, transcribing as it goes; then captions {action: "rewrite", timeline_id: the copy} → every line with its id and limits
3. captions {action: "rewrite", timeline_id: the copy, language: the version's code, lines: [{clip_id, text: the translation}]}
4. check_timeline {timeline_id: the copy}: the one check; a sheet only in the four cases above
5. publish_preview {timeline_id: the copy}: its tile on the master's preview; captions {action: "export", timeline_id: the copy, format: "srt"} where a platform takes a file

## The plan: a spoken version

1. transcribe_media {timeline_id: the timeline the work is on}
2. change_language {timeline_id, target_language, idempotency_key: a new UUID, consent: true only where the user confirmed it}; the response's `plan.needsVoiceIds` names the speakers left as they are
3. get_jobs {ids: [from step 2]} → the new timeline id
4. duck_music {timeline_id: from step 3} where music plays
5. captions {action: "rewrite", timeline_id: from step 3} → every line, terms corrected with the same call with lines and language
6. level_audio {timeline_id: from step 3}, then check_timeline {timeline_id: from step 3}: the one check
7. publish_preview {timeline_id: from step 3}: its tile on the master's preview
