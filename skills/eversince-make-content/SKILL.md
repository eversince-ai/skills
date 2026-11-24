---
name: eversince-make-content
description: "Running content with or for a user: their goals, research into what works, a strategy, making it, review, and publishing."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-make-content"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill covers making content with or for a user over time: understanding their business and goals, researching what works, building a strategy, making the content, involving the user where they want to be involved, and getting it published. Everything here is optional and adapts to the user: what the agent already knows about them, and how they want to work, comes first.

## Understand the user and their goal

A creator, a business, a person promoting a product, a course or themselves: who they are and what they want from content (reach, sales, signups, a community, a portfolio) decide what kind of content fits. Start from what the agent already knows; what is missing comes from the user. Their documents (`read_document`), brand kit (`manage_brand_kit`) and existing work in the workspace (`get_workspace_overview`) add to it.

## Research

Research grounds the strategy, so it usually comes first and is repeated as the strategy needs, sometimes before every batch.

- **The user's own content**, where they already post: an account connected in Eversince is read with `read_social_results` (their posts with the platform's own numbers, those posted outside Eversince included, free); one not connected, or on X, with `pull_posts` by platform and account (public counts: views, likes, comments, shares). Either way the agent learns their voice, what has worked for them, and what has not.
- **Content with traction in their field**: `pull_posts` by keywords or hashtag, or by named accounts and competitors, finds what is getting attention now. Trending matters, and so does any post with strong engagement relative to its account: both show what is working.
- `inspect_media` reads a post's frames, `pull_posts` transcript_of its words, and `keep_posts` keeps a reference in the library. `skill-social-trends` holds the method for reading a field's posts and ad libraries into angles and briefs, `skill-account-audit` for reading one account against its own baseline.
- `pull_posts` is charged in AI credits, a fraction of a credit per page of posts.

## Strategy

A content strategy is usually a mix:

- **Formats**: videos, images, and carousels of images or videos, in the shapes the user's platforms take.
- **Angles**: the recurring ideas and series the user's content returns to, each one repeatable.
- **What is gaining traction this week** around the user's field, and how the user can join it in a way that is authentic to them. This is often the most important pillar for reach, and it is refreshed with each round of research. A trending sound is usually added in the platform's own app when posting: stock music in Eversince does not include platforms' trending audio.

Where the user wants the strategy kept, it goes in a document in the library (`save_document`) or in the agent's own system, so later sessions continue from it.

## Making the content

The content is made with whatever the work needs, which can be a plain edit: cut a recording, add captions, reframe for vertical, put a few clips together. The skills cover common formats and are starting points; some content needs none of them. The tools for editing video (`manage_timelines`, `edit_timeline`, `captions`, `auto_reframe`, `remove_silence`), images (`manage_canvases`, `edit_canvas`), graphics (`compose_motion`), stock (`search_stock`) and AI generation (`generate_image`, `generate_video`, `generate_audio`) are all available. `skill-make-videos`, `skill-make-images` and `skill-promote` map them by kind of work. Around the making: what to post (`skill-content-ideas`), the hook and the script (`skill-hooks-and-scripts`), the plan by day (`skill-content-calendar`), the same video or image for other platforms (`skill-content-repurposing`), the item and text for posting (`skill-publishing`), the workspace kept in order (`skill-keeping-content-organized`), and past content brought in and searched (`skill-searching-your-backlog`). A generated video is prompted per `skill-ai-filmmaking` (a recurring person `skill-ai-influencer`); the user's own take changed by a model (another angle, light, place, outfit) is `skill-transform-video`.

## Involving the user

How involved the user is depends on them. Some want to see every edit: each one shows in the app (`publish_preview`), and the agent waits for their reply or their comments (`get_comments`), making changes on the same timeline or canvas. Others want the content to run with little of their time. Where that fits, the user may be involved in a few places, as they choose:

- **Media**: new footage, photos and recordings on a regular basis, through the upload page (`import_media` link), the app, or the desktop app.
- **Context**: what is happening in their life or business, which often becomes the content. The agent keeps it organized, in a document in the library or in its own system.
- **Review**: checkpoints on edits, or on a batch of posts before they go out, as often as the user wants.

## Calendar and publishing

The calendar (`manage_calendars`, one per workspace) is optional to use. It is where the agent and the user manage content together, in a view the user opens in the app. One card (`manage_cards`) can carry a post from idea to done: its working title, the caption or post text in its body, the finished items attached in order, each with text of its own (a carousel's slides, the parts of a thread), a day with its time and channels, and a status, all on the same card. Nothing on the calendar posts by itself; users who keep a content calendar elsewhere can keep using it.

Eversince makes, edits and holds the content and the conversation about it, and posts it to the accounts the user connects (Instagram, TikTok, YouTube, X, LinkedIn, Facebook, Threads, Pinterest): `manage_social_accounts` lists them and connects more, `manage_social_posts` sends or schedules a card's items and text with its card_id and reads each post's result. `manage_social_accounts` list also says whether the workspace keeps agents' posts for approval: there a post comes back held and goes out when a person approves it in the app. Connecting an account is the user's choice, charged per account a month. Where the user posts somewhere that is not connected, a rendered item's public url (`get_items`) and the card's text go out by that route. Comments and messages on those accounts, and a reply sent to everyone who comments a word, are `skill-inbox-and-auto-replies`; ads on a connected ad account are `skill-run-ads`; a page of work anyone can open is `skill-public-links`. `read_social_results` reads what the posts did for the next round of research and strategy, each post made here tied to the library item it came from; `pull_posts` reads the public counts of an account that is not connected.

## Running it as a routine

A content routine repeats: new research, new material, the next batch. The agent can run it on its own scheduler (a task, goal or routine in its host). `get_workspace_changes` returns what changed since the last run.
