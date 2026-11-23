---
name: eversince-searching-your-backlog
description: "Past content wherever it lives (YouTube, TikTok, Instagram, Drive, a computer) brought in with the user, then searched."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-searching-your-backlog"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A backlog is the user's past content, wherever it lives: their YouTube channel, their TikTok or Instagram, a Google Drive or Dropbox, folders on their computer, or the workspace's own library. To search it, first bring it in with the user so it can be searched by what is said and shown, then find what they are after. A brief, a reference or a brand the user gives outranks every default here.

## The plan

1. The channel, the range and the purpose agreed with the user; search_library for what is already in
2. pull_posts {source: "youtube", account: the channel, order: "latest"} and its cursor, page by page back to the date set
3. The count and the cost known (the pages and one transcript a video)
4. pull_posts {transcript_of: each video's link}
5. save_document per video {name: its title, new_project: "<channel> backlog", folder: "Transcripts", content: its link, date and timed transcript}; save_document {name: "<channel> backlog", project, content: what was read and up to when}
6. search_library {project, query} for what the user is after; read_document on each match for the moment and its time
7. save_document {name: "<subject> moments", project, content: each moment with its video, date, line and time}
8. The answer: what was found, and what can be made from it

## Bringing it in

Agree with the user where the backlog lives, which part of it (a channel, a date range, a folder, a series) and what it is for. Start with the smallest slice that answers the need (one channel read by its transcripts, one folder, the last year) and search it at once; bring in more when the user asks. Check what is already in the workspace first (`search_library`, `get_workspace_overview` for its projects). When the backlog is the library itself, nothing comes in; the work is making it searchable: `get_items` says which reads each file is missing and `prepare_media` starts them (transcript, search_index), twenty files a call. Leave the user's files in their projects unless they ask for a move. Each other place has its own way in:

- **Files on the computer**, including a Google Drive or Dropbox folder synced there by its own desktop app: `import_media` local with the paths and `cloud_copy: false`, through the Eversince desktop app. The files are read where they sit and stay on the computer: no cloud copy, no cloud storage, and transcribing and indexing run there, free, at any size. A file goes up only when something made from it needs the cloud. Such a backlog is searched by what is shown and said through the desktop app on that computer; an agent in the cloud finds only what has a cloud copy. If the user wants it searchable from other devices or agents, offer a cloud copy (the default `cloud_copy`).
- **TikTok and Instagram**: list the account's posts with `pull_posts` (account, order latest) a page at a time, with `cursor` for the page before, back to the date the user set. Bring in the posts wanted with `keep_posts` (20 files a call, within two days of the pull, a TikTok video within hours); their videos land in the library.
- **YouTube**: list the channel with `pull_posts` (source youtube, the channel as account), a page at a time back to the date set. Its videos cannot be brought in from a link, but their words can: `transcript_of` on each video's link reads its transcript (a page each), enough to search the whole channel by what was said. Videos to cut from come in as files the user downloads from YouTube Studio and adds.
- **A public Google Drive or Dropbox file link**: `import_media` url. A private file or a whole folder comes in through the desktop app or as files the user adds (`import_media` link opens a page they drop files onto, from a phone too).

Know the cost before anything runs: the pages listed, the transcripts read, the hours of video to transcribe and index (cloud processing on the web, free through the desktop app), with `estimate_cost` where it applies. A large backlog can run in parts.

## Keeping it together

Give a backlog one project (`new_project` "<source> backlog") whose description names the source and range. Keep one document in it (`save_document`, "<source> backlog") saying what came in, from where and up to which date, so a later session knows what is there. A video read without its file (a YouTube video) gets its own document in the project with its title, link, date and timed transcript, so its words are searchable in the library (a document holds up to 200k characters, about two hours of speech). When the backlog is brought in again later (every month), add only what is new since that date.

## Searching it

- By what is said and shown inside the files: `search_footage` with the backlog's `project` returns the best matching moments with their spans; a duplicate, or a cleaned copy and its original, fold into one hit with `also_in`. Files not yet indexed are named; `search_footage` with their refs and no query indexes them (in the cloud with a query) and returns job ids for `get_jobs`.
- For an exhaustive ask ("every time X is said"), read instead of searching: `transcribe_media` with `refs` and `format: "lines"` over each file. A search returns the best matches, not all of them.
- By words across the transcripts kept in documents: `search_library` with the project.
- Read each match in its surrounding lines before keeping it (`transcribe_media` with `refs`, `window` over the span); a word is not always a moment about the subject. A spoken match can cover a long stretch (one test returned 26 seconds for one point), so narrow it to the exact lines before cutting.

The user decides what to look for: one moment they remember; every moment on a subject; the strongest moments for highlights (`skill-clipping`); what can be made again for today (`skill-content-repurposing`); one cut of a subject across years (a supercut, below). Keep the moments found in a document in the project, each with its source, date, a line on what is said, and the item linked with its time, or a YouTube link carrying the time (`&t=` seconds) so it opens at the moment. The user can jump to them and a later session can build on the list. Anything made from them shows in the app.

## Choosing what to bring back

When the user wants to reuse rather than find one thing:

- **What people usually want back**: a line they said that makes a clip or a quote card; B-roll found by place, mood or action ("the city at night", "hands typing"), which newsrooms search by meaning rather than by file name; a proof point (a number, a result, a customer's words) for a new piece.

- **Start from what worked**: the posts with the most views, and also those with the most comments, saves and shares; a lively discussion on modest reach often holds the better idea. `pull_posts` returns each post's numbers; an account connected in Eversince reads its own free (`read_social_results` posts).
- **Evergreen first**: tutorials, demos, explanations and interviews age best. The test is whether the point still helps someone who never saw the original.
- **Natural breakpoints**: a long video with several topics, or a clear turn, gives stand-alone moments of 30 to 60 seconds for short-form (`skill-clipping`).
- **Check before reposting**: facts, prices, names and product screens may be out of date; update them or add a line that dates it.

## A supercut

Every moment on one subject, cut together. Find the moments as above, read each in its surrounding lines, then place them on one timeline in the order that builds the point (`manage_timelines` create with `clips`, each {ref, source: [start, end]}). Mark each with its recording and date (addMarker kind note). From the assembly on, edit it as any video (`skill-video-editing`).

## Handover

- The backlog in the workspace as agreed, in its project, with the document saying what came from where and up to when.
- What was searched for, found and listed, and what can be made from it.
- Also possible: the rest of the backlog brought in, the backlog swept again each month for what is new, highlights or a supercut from the moments found, repurposed posts from the strongest.
