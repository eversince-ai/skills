---
name: eversince-faceless-videos
description: "A channel with nobody on camera: the topic, one format kept episode to episode, voice and pictures, and staying clear of mass-produced."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-faceless-videos"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A faceless video carries its story with a voice, words on screen and pictures, and nobody on
camera. Done well it is a channel: a topic people keep coming back for, and episodes that sound and
look like the same show. A brief, a reference or the user's own channel outranks every default here.

## The plan: a channel's first video

1. The topic: if the user has none, find what is taking off in a niche they care about (`skill-social-trends`), and offer a few channel ideas, each with who it is for, why it can last, and three first episode titles; the user picks one
2. The format, agreed once: Shorts (9:16, under a minute) or long-form (16:9, minutes); the pictures (motion graphics, the user's footage or screen recordings, stock, generated, or a mix); the voice (a generated one from `list_models` type voice, or the user's own recording)
3. The first episode, made as `skill-explainer-videos` makes one: the script in beats, the voice, a picture per beat, captions, music where wanted
4. The format written down: a document in the channel's project (below), so every later episode follows it
5. `publish_preview` for review, then `skill-publishing` when the user posts it

With nobody answering, make the first idea as a Short with motion graphics and stock, its script on screen, and say that default in the handover.

## Choosing the topic

The topic is the user's call; what the numbers say, for them to weigh:

- **What pays on YouTube**: finance, business case studies, real estate and software reviews earn the most per view (often several times more); sleep sounds, ASMR, scary stories and compilations earn the least.
- **What stands out**: in one 2026 study of outlier videos (ten times a channel's usual views), finance channels produced them about five times as often as faceless automation channels, which are crowded with near-identical videos.
- **What lasts**: a topic with endless real material (companies, history, how things work, a field the user knows) carries a series; a fad does not.

## Formats that carry a faceless channel

The formats the biggest faceless channels are built on:

- **Documentary and history**: a person, an event or a place told as a story over archive-style pictures, maps and graphics.
- **Business case studies**: how a company won or failed, a narrated breakdown with its numbers on screen.
- **Explainers**: how a thing works, drawn as it is said (whiteboard or motion graphics), finance and technology above all.
- **Lists and rankings**: "5 things", "every X ranked", one item per beat, the best last.
- **Screen-led**: a tutorial or a walkthrough over the user's own screen recordings (`skill-screen-recording-editing`).
- **Text-led shorts**: a story or a list told in words on screen over music or a simple picture; on TikTok, a photo slideshow with one point and its keywords per slide, since TikTok search reads the text.

Pick the format for the audience and the topic, then hold it: the structure, the voice and the look repeat; the topic changes.

## The script and the pace

- **Write the hook first**, then the script back from it: the first seconds make a clear promise, with no channel intro before it.
- **A structure that holds a long video**: the hook, the context, three escalations each bigger than the last, then the aftermath.
- **Re-hook every 30 to 60 seconds** in long form: a question left open, a shift in the pictures, a thread that pays off later.
- **Change the picture every 3 to 5 seconds**: a new shot, a zoom, a graphic; a still that sits under a whole paragraph is where viewers leave.
- **Match the picture to the line.** Stock shows moods and places, not specific things: a named product, a company, a number or a step needs a screen recording, a motion graphic with the name or the figure, or a generated picture made for it. Generic stock under a specific line (typing hands while the voice names a tool) is the most common faceless tell.
- **A voice from a recording works**: the audio of any video the user made can be the narration, with pictures laid over it; `check_timeline` then notes the recording's own picture is never seen, which is the point.
- **Words on screen back up the voice** (a number, a name, a keyword), never the whole sentence repeated, except in a text-led short where the words are the video.
- **On Reels**, shares and saves count most, so an episode that is useful to keep or send does better.

## What keeps it from looking mass-produced

YouTube does not demonetize faceless channels. Its policy on inauthentic content (renamed from repetitious content in July 2025) refuses monetization to content that is mass-produced or repetitive: near-identical videos from one template with nothing original in them. AI voices and generated pictures are fine where the video adds something of its own. What keeps a channel on the right side, and what viewers stay for:

- Original research, a view, a story or a script that someone actually wrote for this episode.
- Pictures that show what the voice says, not loops behind a reading.
- Variety inside the format: a held structure, but each episode's own opening, examples and pictures.
- An AI persona presented as a human expert giving health, legal, money or political advice is the clearest case that is not monetized; a narrator's voice is not a persona.

## The format document

The first episode settles the format in a document (`save_document`, "<channel> format") in the channel's own project (`save_document` new_project: "<channel>"): the voice (`voice_id`, `stability`, tags), the length, how an episode opens, its beats and how it ends, the look (fonts, colors, the graphic style, the caption template), the intro and outro, the music. Every later episode reads it first, follows it, and lands in the project. A change the user asks for is written back into it. Reusable pieces (a title card, a lower third, a caption template) are kept once: `compose_motion` save_as, `captions` save_template_as.

## The check

- The first two seconds say what the episode is about, with the voice and the picture together.
- Every picture shows what its line says; nothing only decorates.
- The episode reads as the same show as the last one, and still has something of its own.
- Captions on for feeds, where most watch muted.

## Handover

- The episode at its shape, in the app with its app_url; the format document in the channel's project.
- Also possible: the next episodes on a schedule (a routine in the user's agent, each reading the format first), a thumbnail per episode (`skill-making-thumbnails`), a Shorts cut of a long episode (`skill-content-repurposing`), the post text for each platform (`skill-publishing`).
