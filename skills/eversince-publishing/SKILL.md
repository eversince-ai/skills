---
name: eversince-publishing
description: "A video or image made ready to post: each platform's file and text, the time, then sent or scheduled to connected accounts or handed over."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-publishing"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A video or image is ready to post when each platform has its item, its post text and a time. This skill gets
it there and out. Accounts the user connected in Eversince (`manage_social_accounts` list) take the
post directly, sent now or scheduled. Where an account is not connected, the post goes out by whatever
route the user has: another scheduling tool, a platform's own API, or the user posting by hand. A brief
or the user's own habits outrank every default here.

## The plan

1. Settle where, when and what shape: only the platforms and accounts the user posts on, the day and time, and each platform's aspect ratio
2. The item for each platform: `render_timeline` {timeline_id, idempotency_key} per video version, `render_canvas` {canvas_id, format} for a still or a carousel (images in order, or format "pdf" for a LinkedIn document post), which returns its items in the result; `get_jobs` returns each rendered video, and a cloud PDF over twelve pages, with its url
3. The post text for each platform, written in the user's voice (below)
4. Where the user plans on the Eversince calendar: place each post on its day (below)
5. `manage_social_posts` check with what create will take answers each platform's rules with nothing made or sent. Show the user each platform's item and text, and get their yes before anything is sent or scheduled
6. Send it: `manage_social_posts` create {caption, media, targets, scheduled_for, card_id} to the connected accounts, a platform's own text as that account's caption; anything else by the route the user has. Where the workspace keeps agents' posts for approval (`manage_social_accounts` list, agents), or with hold: true, the post comes back held and goes out once a person approves it in the app

Where a platform needs a version that does not exist yet (another shape, a shorter cut), make it first with `skill-content-repurposing`.

## The item

- **Video**: the user picks the aspect ratio per platform, confirmed before rendering. Shown full size: 9:16 (1080 by 1920) on Reels, TikTok and Shorts, 4:5 or 3:4 in the Instagram feed, 16:9 on YouTube. Render each from its own version, so nothing carries another app's watermark.
- **Stills and carousels**: one image per slide, in order; Instagram takes up to 20 slides. A LinkedIn carousel is one PDF, sent as the post's only item.
- **A cover**: what a Reel, a Short or a YouTube video shows before it plays: a strong still from the video (`extract_media` mode frame) or a designed one (`skill-making-thumbnails`), sent as the post's `cover`. TikTok takes a moment of the video itself (its cover_at_ms option).

## The post text

Write each platform's text on its own; the same caption pasted everywhere reads wrong on most of them. Use the user's own voice and words, and only claims the content makes.

- **Instagram**: the first line is the hook, since only about 125 characters show before "more". Instagram allows at most five hashtags since December 2025; a few specific ones beat broad ones. Words people search for, said naturally, help the post be found.
- **TikTok**: short, with the words someone would type to find it; TikTok search reads the caption.
- **YouTube Shorts and videos**: a title that says what it is in plain words (under about 100 characters), and a description with the same search words. Chapters are lines of the description, each a time and a title (0:00 Intro), the first at 0:00 and three or more; a timeline's chapter markers (`get_timeline`) give the times. A vertical or square video of up to 3 minutes is a Short, with no setting to choose. Subtitles are SRT or WebVTT items in the library, one a language (`subtitles`).
- **LinkedIn**: longer is fine, often 150 to 300 words in short paragraphs, the point in the first two lines. Most LinkedIn video is watched muted, so the video needs captions.
- **X**: within 280 characters; one clear line usually does better than a packed one.
- **Alt text** for every image: one plain sentence of what it shows, sent with the post as `alt_text`.
- **An AI label** where a platform asks for one (realistic AI people or scenes, a cloned voice): the account's ai_label option sets the platform's own label.
- **A first comment** carries what would crowd the caption: a link on LinkedIn, where posts with links in the text reach fewer people, or extra hashtags on Instagram.
- **A thread** on X or Threads splits a longer point into posts that each stand alone; every post of an X thread counts as an X post.
- **The ask**: one, in the user's words (a comment, a follow, a link), only where they want one. A post that asks for a comment with a word can be answered automatically (`skill-inbox-and-auto-replies`).

## The format

One video on Instagram is a Reel; a story is chosen. A trial reel goes to people who do not follow the account first, a way to test a hook without risking the feed. A Facebook video can go out as a reel; a story on either holds one item and no caption.

## What else a post can carry

`manage_social_posts` choices lists everything one account's post takes, with its limits and what the account itself offers. What is worth knowing exists:

- **People and place**: collaborators and tagged people on Instagram, tagged people on X, a location on Instagram and TikTok.
- **Sound**: a track from Instagram's audio library on a Reel (the search with no word returns what is trending among the tracks open to outside tools, for an account connected through Facebook), or one of TikTok's commercial tracks on a business connection. A sound outside those is added in the app: a TikTok post sent as a draft waits in the TikTok inbox to be finished there.
- **A poll** on X or LinkedIn, **a quote** or a reply on X, **a repost** on LinkedIn, **an article** on X for a Premium+ account.
- **Where it sits**: a YouTube playlist, category and visibility; a Pinterest board and section; the Facebook Page or LinkedIn company page to post as.
- **A link carousel** on Facebook: one card a picture, each with its own link.
- **Who sees it**: countries on Facebook, X and a LinkedIn page; who can reply on X; comments off on Instagram; a paid partnership label where the post is one.

## The time

The best time is the user's own: when their audience is active, which their past posts' results show (`read_social_results` best_time, in UTC, turned into the user's time zone). Without data, the audience's waking hours are a fair start; a steady rhythm matters more than the minute.

## The calendar

Only where the user keeps their plan in Eversince. Each post on it is a card (`manage_cards` create {place: "2026-09-14", title, attachments: the rendered items in order, fields: {"Calendar time": "17:00"}}), each item with its own text where a platform needs one; `manage_calendars` {action: "get"} names the time and channel fields. Days and times are in the user's time zone. A card goes out when it is sent with its card_id; nothing on the calendar posts by itself.

## Sending

The user approves each platform's item and text first. With several accounts on one platform, the user names the one. A connected account takes the post in one call, with what its platform asks for: TikTok who can see it (`manage_social_posts` choices lists the account's own options), a YouTube title, a Pinterest board. check, create and update return `unset`: what was left to each platform (no thumbnail, public, the first frame as cover), to settle before it goes. A scheduled post can be changed or cancelled until shortly before it goes out, a held one until it is approved.

After it goes, `manage_social_posts` get shows each account's result and address: An account that failed says why; retry sends it again to the accounts it missed, and never twice to one that has it. list with status failed finds every post that did not go out, held the ones still waiting. A call that timed out may still have landed, so the post is read before anything is sent again. A published post's text can be changed on Facebook, LinkedIn, Pinterest and YouTube, and the post removed from most platforms but Instagram and TikTok. A published YouTube video's title, tags, playlist and thumbnail can be changed in place; YouTube's own thumbnail test runs only in YouTube Studio, so a thumbnail is tried against another by swapping it and reading the video's results after a few days.

Platforms restrict an account that acts like a machine, so pace matters. Each account posts at most 25 times an hour (a post past that waits and goes out by itself) and, in a day, 100 times on Instagram, 250 on Facebook and Threads, 50 on X, 25 on Pinterest, and 15 videos and 15 photo posts on TikTok. Limits are per connected account. Pinterest also asks for fewer than 10 pins in a row. The same post to the same account within a day is refused; LinkedIn and Instagram also turn away posts that read as copies, so each account gets its own wording. A refusal that names a wait means wait: sending again sooner lengthens it. Where an account is not connected, hand over the item's url and the text by the route the user has, platform by platform.

## The check

- Each platform's item is at the aspect ratio the user chose, rendered clean.
- Each text fits its platform: the hook in the first line, five hashtags at most on Instagram, 280 characters on X.
- Links and handles are real.
- The time is in the user's time zone, and the user said yes.

## Handover

- Per platform: the item, the post text, the time, and where it went: published (with its address), scheduled, held for approval in the app, or ready to post by hand.
- Also possible: the post on the user's Eversince calendar, a cover for each Reel or Short, a version for another platform, a set of posts for the week.
