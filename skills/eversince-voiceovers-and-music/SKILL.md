---
name: eversince-voiceovers-and-music
description: "A script read in a chosen voice and a track made to the length and mood of the video, every take kept. The mix itself is sound design."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-voiceovers-and-music"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill makes a voiceover from a script and gets the video its music. Generation is `generate_audio`, in credits. Every take is kept beside the timeline so the user can choose. The mix, effects and cleaning a recording are `skill-sound-design`. What the user asks for outranks every default here.

## The plan

1. Pick a voice
2. Write the script for the ear and direct it with tags
3. Generate two or three takes; fix a bad sentence on its own
4. Music: the user's track imported, or one generated
5. Keep every take beside the timeline, place the chosen ones, put the music under the voice

## The voice

`list_models` with type voice lists the voices with gender, age, accent and style; naming one returns its description and a `sample_url` to hear it. With no `voice_id`, `generate_audio` uses the brand kit's voice if it has one, else the default. Every voice speaks every language `language` takes. Pick for the video and its audience (age, energy, accent), and keep the same voice and settings across a series.

## The script and the tags

`generate_audio` reads the text exactly as written, so the script is where you direct the read.

- **Spell it as said:** numbers, dates, prices, abbreviations and symbols written out ("two thousand twenty-six", "forty percent", "Doctor"). A name it mispronounces can be written in IPA between slashes ("/ˌbaɪoʊˈkemɪstri/"), or respelled.
- **Rhythm:** commas and full stops set the pace, an ellipsis adds a pause and weight, a word in capitals takes the emphasis.
- **Tags** direct the delivery and are not spoken: lowercase in brackets, before the words they change. Emotion ([excited], [sad], [curious]), reactions ([laughs], [sighs], [gasps]), delivery ([whispers], [shouts], [sarcastic]), or a description ("[warm, unhurried]"). Use one or two at a time. A tag only works within what the voice can do, so a soft voice will not shout well; pick the voice for the delivery you need. Short pauses can be cued ([short pause], [long pause]), one cue per clause; a pile of tags or cues makes the pace drift and adds artifacts, so a long or exact pause goes in on the timeline.
- **The register:** conversational (to one person, relaxed), direct (confident, second person), minimal (few words, real pauses), raw (imperfect on purpose), hype (short phrases, each harder). Avoid the announcer read.
- **Length:** about 130 to 160 words a minute for narration, slower for dense material. Very short inputs come out less stable, so generate a paragraph rather than single lines where you can.

## Takes

- `stability` (default 0.5): higher holds one delivery across takes, for narration and a series; lower is more expressive and varies more. `similarity_boost` (default 0.75) is how closely it tracks the voice. `seed` gives a close repeat. A read that comes out over-dramatic takes plainer tags and a little more stability; a stiff one, a little less (ElevenLabs' v4 guidance).
- Make two or three takes and pick on emphasis, pronunciation and clean sound. Fix a stumble or a wrong stress in the script or the tags and regenerate only that sentence.

## Music

Unless the user has something else in mind, music is one of two things:

- **The user's track,** brought in with `import_media` (or already in the library, `search_library` type audio). The user is responsible for its rights.
- **Generated,** with `generate_audio` kind music, where the user has credits; with none, it is an option to offer beside the user's own track, not a condition. The tool's description says what each of the three takes (a timed arrangement, a named key and tempo, an exact duration, lyrics), and the user can pick one. Only elevenlabs takes an exact `duration`. google and minimax set their own length, so for a short video the track is trimmed on the timeline.

**The prompt reads like session notes:** genre, tempo, key, two precise mood words (yearning, defiant, not happy or sad), the instruments, the structure, and what to leave out ("no slow intro, no fade out"). Instrumental is the default; vocals when the lyrics are the point.

**Under a voice,** ask for no lead melody in the vocal range and no crescendo of its own, so the voice stays clear. For a short video ask for full energy from the start; a track still building when the video ends never arrives. Two to four versions, changing one thing at a time, give the user a choice.

## Keeping and placing

Attach every take to the timeline (`timeline_media` attach) so the user can hear and swap them in the app; rename where the names do not tell them apart (`update_items`). Place the chosen voice on its own track (`edit_timeline` addTrack kind "audio", addClip by ref). `duck_music` with `ref` lays the chosen track under the cut and dips it under every word, then `level_audio` for the platform. To land cuts on the music's beats, see `skill-sound-design`.

## Handover

- The voice and the music placed, the music under the voice, every other take beside the timeline.
- You can also make: takes in another voice or language, the track at another length, a version with vocals, sound effects (`skill-sound-design`).

## An example plan

1. list_models {type: "voice"}; a candidate's sample with {type: "voice", model}
2. generate_audio {kind: "voiceover", text: the script with its tags, voice_id, stability} two or three times
3. import_media the user's track, or generate_audio {kind: "music", prompt} two to four times; get_jobs for each
4. timeline_media {timeline_id, attach: every take}
5. edit_timeline {timeline_id, operations: [addTrack {track: {kind: "audio", id: "voice"}}, addClip {trackId: "voice", ref: the chosen take}]}
6. duck_music {timeline_id, ref: the chosen track}; level_audio {timeline_id, platform}
7. check_timeline {timeline_id}; publish_preview {timeline_id} → the link
