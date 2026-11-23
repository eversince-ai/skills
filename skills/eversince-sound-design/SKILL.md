---
name: eversince-sound-design
description: "A video's sound mixed to the cut: voice cleaned, music ducked, effects, silence, levels, cutting to music. New voice or track: voiceovers."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-sound-design"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Sound design is the sound of a finished piece: the voice cleaned, music, recorded dialogue, effects and silence, each at its level, in the mix that holds them. Generating a voiceover or a music track is `skill-voiceovers-and-music`; captions are `skill-animated-captions`; a spoken version in another language is `skill-translation-and-dubbing`. A brief, a reference or a brand from the user outranks every default here.

## The plan

1. Where it plays, and which layer leads at each moment
2. Clean the voice where the recording needs it, before any cut
3. Place the music (the user's, the library, made with `skill-voiceovers-and-music`, or a stock track the user picked) and duck it under the voice
4. Effects on the events that need them, and placed silence
5. Level for the platform, check, hand over

## What the job starts from

- The destination (a feed, a video platform, a podcast, a room) sets the delivery level and what the first second has to do.
- Sound that already exists (a licensed track, a recorded voice, footage with live audio) is placed and mixed like generated sound.
- One layer leads at any second: a voice speaking puts the music under it; music leading means nothing speaks; a placed silence drops everything. Two layers at equal level is the amateur mix. The sets, each with its lead: music only (the score is the narrative) · music and voiceover (voice carries story, music emotion) · music and dialogue (people speak in scene, music under) · all three (voice threads between dialogue moments, never all three at once) · ambient and effects dominant (the design is the score).

## Where it plays

- Speech is delivered at about −14 LUFS with a true peak ceiling near −1 dB. `check_timeline` reads against that target and `level_audio` delivers it; both take `platform` (youtube, instagram, tiktok −14; podcast −16; broadcast −23), and `level_audio` also `target_lufs` for a number of its own. Only Apple publishes a target (−16 for podcasts); YouTube is measured turning loud audio down to about −14 and never turning quiet audio up, and the social platforms publish none, so −14 is the safe choice there. A piece forked per destination is levelled and checked per fork, each with its platform.
- A feed plays muted first: captions are the mute experience, and the first second or two of sound (a snap, a voice, an unexpected texture) is what earns an unmute.
- Phones are mono and roll off deep bass: nothing essential lives only in sub-bass or stereo width.
- A social mix runs tighter and louder than cinematic work; wide dynamic range suits a captive viewer.

## Cleaning the voice

Clean before any cut, since the cleaned file replaces the clip's sound. `clean_audio` has two modes (denoise is cloud processing, isolate_voice is charged in credits; with `timeline_id` and `clip_id` the result is a new audio item that replaces the clip's sound when ready, trim and words kept: an audio clip takes it in its own slot, and a video clip's sound moves to a separate audio clip on its own track):

- **denoise** (the default) keeps the voice and lowers the rest: fans, traffic, hiss, keyboards, distant chatter, music under the speech. Use it on ordinary noise.
- **isolate_voice** rebuilds the speech alone, which also removes reverb. Use it on heavy noise or an echoing room. It also strips the room tone and ambience the scene may want, and pushed on audio that was already clean it sounds hollow or robotic.

Clean only what needs it: on a clean recording, either mode can do more harm than good. A recording that would need heavy de-reverb to work is better recorded again, if the user can. A voice that sounds off is compared inside its own recording before anything is changed: a natural voice has bumps and hollows as large as a fault, and a sentence drops 5 to 6 dB from first word to last; flattening that reads as robotic.

## Music

Tempo is physiology: under about 76 BPM the body settles (gravitas, luxury, reflection); 76 to 90 is warmth; 90 to 120 forward drive; 120 to 140 activates; above 140 fatigues, so it is spent briefly. Major resolves, minor yearns. A muddy mix usually means the music is too busy.

Write what the piece needs in one line before looking at any track (its mood, steady or building, under a voice or not), then take it from:

- **The user's own track,** given or named: used, and the cut fits it.
- **The workspace's library:** `search_library` type audio, where a brand sound or a track used before may be.
- **Made for the piece:** `skill-voiceovers-and-music`, for a track at the exact length, tempo and shape the cut needs, where the user has credits.
- **Stock, where the user picks it:** a track's fit is heard, not read, so a stock track goes in when the user chose it or asked for stock; with no track, the piece runs without music and sound effects carry what happens on screen. `search_stock` kind music is chosen against the line by what each result says it holds (`heard`, `sections`, `character`), never by its title. Under a voice, `under_voice` says whether speech stays clear over the track. `bpm` gives its tempo and `first_downbeat` the second where a cut can open on the beat.

A bed under a voice leaves the voice's range empty: no lead melody in the vocal register, no crescendo of its own. That is what lets `duck_music` run soft rather than deep. A track the user brings from outside is theirs to have the rights to; on a business account, TikTok and Instagram allow only their commercial libraries.

## Effects

Effects make events physical: the click as an element lands, the whoosh matched to the transition's speed (short for a cut, long for a dissolve), a riser into a reveal, a hit on the landing, the product's own sounds as texture, a low ambience for place. Every effect is tied to something that happens on screen. Use them sparingly: the same whoosh on every cut trains the ear to ignore it, and a cut on a drum hit in the music already gives the transition its sound.

`search_stock` kind sfx finds one; `generate_audio` sound_effect makes one (0.5 to 22 seconds, credits), and ambience held longer is generated with `loop` and tiled without seams. An effect is placed by its peak, not its start: a click is its own attack and sits on the frame; a whoosh or riser begins frames earlier and arrives at full on the change, and for a graphic built in code that frame is known exactly. Two effects at once differ in pitch or texture or they smear into one. An effect nearly as loud as the voice competes with it, and `check_timeline` does not flag that, so keep effects clearly under speech. `linkClips` keeps an effect pinned to its picture when the cut moves; a split leaves one half unlinked.

## Silence

Silence is placed, not left over: everything stripped for half a second before the biggest hit, a beat of quiet after a peak, a half second before the end card. Near silence that still feels inhabited is a low room-tone bed (a looped effect near 0.05). Dead air used often reads as an error.

## The mix on the timeline

One audio track per layer (music, voice, effects), because per-layer control is what makes the rides tractable: `edit_timeline` addTrack kind "audio" per layer, addClip by ref, updateClip for volume, fadeInFrames and fadeOutFrames, and category ("voiceover", "music" or "sound-effect") on a clip whose kind was guessed wrong. A crossfade between two cues sits on two tracks; the check reads layers, not track counts.

Volume is linear, 0 to 2, 1 as recorded; `check_timeline` speaks in dB: 0.5 is about −6 dB, 0.25 about −12, 0.1 is −20, 2 about +6, and moving a clip by a measured amount multiplies its volume by 10^(dB/20). Starting points: the lead layer at 1, music at its own level where it leads, an ambient bed at 0.05 to 0.1, effects matched to the music's presence, fades of 10 to 15 frames where music or an effect starts or stops, none where one piece of a song meets the next. Speech starts at full level; its only fade is the two or three frames at a join that stop a click (`skill-video-editing`). What the listener gets is `played_lufs` and `played_peak_db` per clip in `check_timeline`, not what the volume fields suggest.

`duck_music` with `ref` lays a track under the whole cut in one call: its `end_at` second (a final hit from its sections, or the file's end) on the cut's last frame, faded out, the speech brought to delivery level first, the track set by measured loudness to sit 13 dB under it and stay below it where nobody speaks, and ducked; a search_stock id goes straight in as `ref`. Where a track's loudness is not measured, `duck_music` keeps its level. So check it: `check_timeline` reports the speech and the music levels, and when the gap is not about 10 to 15 dB, set the music clip's volume so it is. Without `ref` it runs wherever music already sits under speech: every music clip dips just before the first word, holds through pauses under a second, and comes back after the last. Depth is a fraction of the music's level: soft to a half (the default with `ref`), medium to a quarter (the default without), deep to a tenth. Dense narration, unfamiliar terms and a number that has to land take deep; ordinary speech medium; sparse or emotional speech soft; a piece heard on a phone in a feed goes a step deeper than the same mix in a room. It writes the music's volume keyframes and replaces what that track held, so a deliberate ride (`setKeyframes` on volume) comes after it, and anything that moves the speech (a cut, a new take, a language) means running it again. A voice still buried under a ducked track is a level gap, not a shallow duck: `duck_music` with level bed lifts the speech to delivery level and seats the music against it.

Footage with its own audio is kept and mixed around, ducked under a voice, or silenced on the clip's own volume. Its speech counts as dialogue for the ducks once the clip carries words (`transcribe_media` with timeline_id); footage sound that is not speech is levelled by hand and triggers no dip. `remove_silence` reads a source stream (voiceover or dialogue), so the categories in `get_timeline` are read before a source-scoped call; `duck_music` dips under voiceover and dialogue alike.

## Cutting to music

`detect_beats` returns every beat and downbeat as source seconds (before a clip's trim and placement), the tempo, and whether the pulse is steady, variable or unclear. A tempo named in a prompt is a request; the measurement is what the track has. A beat lands on the timeline at its source second minus the clip's trim plus its place; addMarker on each beat the cut uses keeps them visible.

The kind of music decides the pacing. A beat tracker returns a grid whatever it is given, and on gentle music (pads, ambient, solo piano, anything without a steady drum) the grid is guesswork with more beats than anyone hears. Two ways to pace, chosen per section: pulse-led, where drums or a hard pulse push the music and cuts and reveals sit on the beats; breath-led, everything else, where changes wait for the end of a phrase or a shift in energy, shots hold long, and nothing sits on every detected beat. The brief or the prompt that made the track says which; for a brought track where the pulse is unclear, one narrow `inspect_media` prompt (is there a steady percussive pulse, where does it start and stop) settles it.

Where things land:

- A hit is a cut, not a move: a cut, a swap or a color flip lands on the beat with no easing, on its frame or a frame or two ahead, since a change placed exactly on the measured beat reads late.
- A move that must arrive on a beat finishes on it (a wipe filling the frame, a number coming to rest, a title settling), so it starts as many frames before as it takes.
- Big changes on strong beats, small on weak: cuts, camera moves and new sections on downbeats and phrase starts; a swapped word or a flicker between them. The camera moves once a phrase at most.
- A build pays off on a downbeat: layers pile up through a roll or a swell and the next picture arrives where it stops.
- A drop is bigger than a downbeat: on a downbeat the picture cuts and the section keeps its look; at a drop the look changes.
- Count the music's changes, not its beats: on pulse-led music no shot is shorter than about a bar, there are as many treatments as the track has real changes, and a section past about six seconds usually takes a second treatment partway.
- Text that has to be read stays up: one beat for a single word, three to eight for a headline, four to ten for a sentence.
- The end lands: the last word or mark comes in on the track's final accent and holds while the sound rings out.

A brought track rarely starts where the piece should; its opening is often a quiet build. The clip starts on a downbeat of its strongest clean section (addClip with source), fades in short (about 0.4 seconds) and out long (about 1.5), and covers the whole piece with no silent tail. Any retime means checking both ends again.

Fitting a song to a cut whose timing is set: the song is edited, the picture stays, and nothing in the song is reordered or repeated. Cuts take out whole sections, downbeat to downbeat from `detect_beats`, so every jump is a whole number of beats (bars where it can be), and never split a sung line: the words' own times (`transcribe_media` on the track) show where a line ends. Where the picture needs time the song does not have, the gap takes the same song without vocals (its `separate_audio` instrumental, or bars nobody sings in), chosen to run unbroken into what follows, so a single join is heard and it sits on a beat, under a sound effect that hits there. An instrumental is silent wherever the band stops under a sung line, so a bridge is chosen from bars that sound on their own (their loudness read, not assumed), and where the vocal carries the music the song simply plays on through it. A bridge runs a whole number of beats, so both its joins land on the grid; where the beats on the two sides cannot meet, the picture moves a few frames, never the music off its beat. A musical join is a hard cut on the beat; a fade there reads as a dropout. The words then land where the picture expects them: a sung word a frame or two after its visual, not before.

## The order across tools

Every step alters what the next works on: `clean_audio` before any cut where the user says the recording is noisy, then `remove_silence` and `remove_words`, then `transcribe_media` with timeline_id for speech that has no words, then the music placed and cut to, then `duck_music`, then `check_timeline`, then `level_audio`, then `check_timeline` again. A cut or a new take after the ducks means `duck_music` again before the levels; `level_audio` moves the whole mix together, which is why it comes last. `estimate_cost` quotes a batch of generations before it runs.

## The check

The mix is read back, not heard. `get_timeline`: every clip on its layer, levels stepped where the plan says lead and support, the placed silences present. `check_timeline`: each clip's played loudness and peak, the clips that will distort, the speech against the delivery target, the loudest stretch of music included, where the dips have to reach the speech. Wrong looks like: two leads in one second, music meeting speech undipped, a fade obscuring speech, a cut clipping a word, a breath or a reverb tail, an opening louder than what follows, an ending trimmed mid-phrase or stopped at the file's edge. `inspect_media` with a `prompt` answers what the read cannot: whether a track arrived instrumental or at the energy asked for, or the render before it goes out.

## Handover

- Speech at the destination's level: `level_audio` with its platform, then `check_timeline` clean.
- Music, where there is any, heard where nobody speaks and ducked under every word.
- In the app from its first media, where it plays, the result's app_url the link the user opens; `render_timeline` makes a file for use elsewhere.
- You can also: put effects on the key moments, place a silence before the biggest hit, retime the cut to the music's beats, level the mix for another platform, make a voiceover or a track where none fits (`skill-voiceovers-and-music`).

## An example plan: the sound of a cut

1. clean_audio {ref, mode: "denoise", timeline_id, clip_id} where the recording is noisy, before any cut
2. The track: the user's, search_library {type: "audio"}, one made per `skill-voiceovers-and-music` where the user has credits, or a stock track the user picked (search_stock {kind: "music", query: the one line})
3. get_timeline {timeline_id} → tracks and clip categories
4. transcribe_media {timeline_id} where uploaded speech has no words; a generated voice carries its own
5. edit_timeline {timeline_id, operations: [addTrack {track: {kind: "audio", index: 0, id: "voice"}}, addClip {trackId: "voice", ref: the voice}]} where the voice is not on the timeline yet
6. duck_music {timeline_id, ref: the track, end_at: its final hit}: the speech at delivery level, the track placed, seated under it, ducked
7. detect_beats {ref: the track's item, the ref step 6 placed}; edit_timeline {moveClip and trimClip so the cut's changes land on the beats it uses}, where the cut follows the music, then step 6 again without ref if a move shifted the speech
8. check_timeline {timeline_id}: the music about 10 to 15 dB under the speech, else updateClip volume on the music clip
9. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over
