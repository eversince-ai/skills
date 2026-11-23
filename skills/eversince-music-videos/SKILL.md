---
name: eversince-music-videos
description: "A performance, footage, photos or type cut to a song: takes synced to the track, cuts on the beat, lyrics on screen."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-music-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

The song is the spine. Its sections decide where the video changes, its beats decide when the
picture cuts, and the pictures are laid onto that grid. A brief, a reference or a brand from the user
outranks every default here.

There are four common shapes, and one video can mix them:

- **Performance:** the artist singing or playing to the song, filmed over several takes. Most real music videos are built on this.
- **Montage:** photos or clips cut to a song.
- **Lyric video:** the words are what people watch.
- **Type only:** moving words and graphics, with no photos or footage.

## The plan: a performance video

1. Find the song's beats and sections: `detect_beats` {ref: the song}
2. `manage_timelines` {create: {name, aspect_ratio: the destination's}} → the timeline
3. Line up the takes to the song: `sync_recordings` {timeline_id, refs: [the song, the takes]}
4. Cut the performance across the whole song: `switch_angle` {timeline_id, switches: [{from_seconds, to_seconds, angle}]}, cuts on downbeats
5. Add story and detail shots where the video cuts away: `edit_timeline` addClip on a track above the takes, on their beats
6. `auto_reframe` {timeline_id} for a vertical version
7. `level_audio` {timeline_id}, then `check_timeline` {timeline_id} and `inspect_timeline` {timeline_id, sheet: true}
8. `publish_preview` {timeline_id}: the link to hand over

## The plan: a montage, lyric or type video

1. The user's song, or a song made for the piece (`skill-voiceovers-and-music`; a song with lyrics comes from the music generation itself)
2. `detect_beats` {ref: the song}
3. `manage_timelines` {create: {name, aspect_ratio, clips: [{ref: the song, source: [the section's first downbeat, its end], snap: false}]}} → the timeline and the song's clip id
4. `edit_timeline`: a fade on the song (fadeInFrames 12, fadeOutFrames 45) and addMarker on the downbeats the cut will use
5. `edit_timeline`: addClip for each picture on its beat; moves and transitions only where the user wants them
6. `auto_zoom` {timeline_id, beats_of: the song's clip} only where the user wants punch-ins
7. `captions` {action: "generate", timeline_id, source: "music"} for a lyric video
8. `level_audio`, then `check_timeline` and `inspect_timeline` {sheet: true}
9. `publish_preview` {timeline_id}

## What the job needs

- **The song:** the user's track as it is, the soundtrack of a video they bring, or a song made for the piece. On a long track, pick the section by how it sounds ("where the drums come in", "the last chorus") and start it on a downbeat, not on a second picked by eye. One short `inspect_media` question finds where the energy lifts.
- **The pictures:** performance takes, the user's own photos and footage, shots generated for the song (`skill-ai-filmmaking`, `skill-ai-animation`), or a mix. Use every picture the user brings unless they leave one out, and hold each long enough to take in.
- **The destination:** 9:16 for Reels, TikTok and Shorts, 4:5 or 1:1 in a feed, 16:9 for YouTube.

## The track

- `detect_beats` returns every beat, every downbeat and the tempo, in seconds of the file. On the timeline a beat sits at its file second, minus the clip's trim, plus where the clip starts.
- Divide the song where it really changes before placing any picture: an intro, a verse, a chorus, a drop, a stop, a build. A short piece has one to six sections. A section longer than about six seconds usually takes a second idea partway through.
- Where a steady drum drives the music, cut on the beat. On gentle music without one, cut on the phrase and let shots breathe; cuts forced onto a soft grid look robotic. `skill-sound-design` has the detail on where a cut lands against the beat.
- Cut on the main beats, not on every drum hit, so the edit follows the song and not the hi-hat.

## Performance

The artist mimes or sings along to the song playing on set, so each take's camera sound carries the song. That is what lines it up.

- `sync_recordings` takes the clean song and the takes (twelve files at most) and places each take at its moment in the song. The song plays and the takes' own sound goes quiet under it. A take whose camera did not pick up the playback comes back unplaced and named: transcribe it (`transcribe_media`), find a sung line and place it by hand where that line falls in the song.
- Build the performance cut over the whole song first, as if it were the finished video: the best take for each line, cuts on downbeats or phrase ends. `switch_angle` with `switches` makes the cut and keeps every take in sync.
- Wider shots for the verses and closer ones for the chorus and big notes is a common default. Hold a shot longer where the performance needs room: a held note, a look into the lens.
- Then decide where to cut away to story, detail or B-roll shots. They go on a track above the takes, on the song's beats, and the performance shows again when they end. Use only the strongest shots, and don't cut away so often that the artist disappears.
- Lip sync has to hold at every cut. A synced take is never sped up, slowed down or slid by hand; that breaks sync. The one exception: a sync can land a frame or two off, so check one frame where a word is sung, and if the mouth leads or trails the sound, nudge that take by a frame with `moveClip`. A slow-motion shot of the artist only works where it was filmed for it, with the song played faster on set.

## How often to cut

- One picture a bar in a verse, two a bar in a chorus, one a phrase in a calm stretch. Nothing shorter than about a bar on music with a steady beat. A face or the main shot gets two bars.
- Put the strongest pictures on the drop and the big downbeats, the quiet ones in the calm sections, and the best of all on the last hit. Order by energy, not by date or file name.
- A hard cut on the beat is the default. Use transitions only where the user wants them, and put them on phrase changes and at the drop. Use one transition style throughout, with one or two accents.
- On gentle stretches: long holds and slow dissolves at the phrase edge, no cuts on every beat.
- Punch-ins on the downbeats, where the user wants them: `auto_zoom` with `beats_of` naming the song's clip on footage; on a still, a scale keyframe that steps in and settles.
- A speed ramp is built in pieces: splitClip where the pace turns, setSpeed on each piece (0.5x into 3x, say), a blur transitionIn of 4 to 6 frames on the fast piece, the drop landing where the fast piece starts, and the clip's own sound muted.

## Pictures

- A still can hold as it is, cut on its beat. If the user wants movement, either settle it (start at about 1.06 scale, ease to 1 over about a second, then hold) or push it across the whole shot with two linear keyframes so it is already moving at both cuts. Pushes go the same direction through the piece.
- Trim footage to its best moment and cut on action where there is any. Turn its own sound off unless a line is meant to be heard, then `duck_music` dips the song under it.
- Words over footage need the picture dimmed by a third to a half (an adjustment clip or the clip's grade), and change on the same beats as the cuts.
- `auto_reframe` keeps a person in a vertical crop. Frame a still with its transform.

## A lyric video

- Get the words with their timing: `transcribe_media` with `timeline_id` and the song's clip in `clip_ids` (music is otherwise skipped), then `get_transcript` with source music. Speech recognition mishears singing, so check every line against the real lyrics: the ones the user gives, or the ones the song was made with.
- Each line lands on a beat. As captions: `captions` generate with source music, short lines (`skill-animated-captions`). Where the type is the design: text clips, or a `compose_motion` graphic with the line times written in, counted from the graphic's first frame. The hook's words hit on their downbeats.
- Type carries the frame: large, one font family, hierarchy by size and weight (`skill-graphic-design`).

## Type only

With no pictures, the video is typography and graphics: one idea per section, words changing on the beats, one standout move per section, beat times written into each graphic. The background is a solid clip on the lowest track (`edit_timeline` addClip with clip: {id, type: "solid", color}) or a full-frame `compose_motion` graphic. The words are `compose_motion` graphics or text clips. Motion craft is in `skill-motion-graphics`.

## The ending

End where the music ends: on the track's own ending, or on a final downbeat with a fade of about a second and a half. A closing word or logo arrives on the last hit and holds through the tail. A song cut off mid-phrase is the mistake everyone hears.

## The check

- The right part of the song, starting on a downbeat and ending where the music lands.
- The biggest visual changes on the biggest changes in the music, and no hard cuts on a calm stretch.
- In a performance, the lips match the words after every cut.
- One `check_timeline` for gaps, missing media, shots too short to see, lyrics off the frame or too small, and the sound level. `level_audio` sets the level.
- A music video is judged by its look, so draw one picture of it: `inspect_timeline` with `sheet: true`. Single frames only where a finding names a clip or the user asked for the work to be checked.
- Lyrics on screen match the vocal word for word.

A last pass over the whole piece is in `skill-finish`.

## Handover

- The music video at its destination's shape, the song at delivery level, lyrics word for word where they are on screen.
- In the app, with the result's app_url as the link the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: a vertical cut, a lyric version, a teaser of the strongest section, a cover frame, punch-ins on the downbeats, moves on the stills.
