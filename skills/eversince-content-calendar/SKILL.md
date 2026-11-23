---
name: eversince-content-calendar
description: "Running posts on the workspace calendar, planned ahead or placed when ready: entries, captions, statuses, the hand-off to posting."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-content-calendar"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A content calendar shows the user's posts by day: planned, in the making, or finished and ready to go out. In Eversince it sits next to the footage and the edits, so the user, their agent and anyone they work with see the same plan and the same finished posts. Most of the work is ongoing: moving this week's posts forward, placing finished ones, filling next week. Eversince plans, makes and holds the posts, and sends or schedules them to the accounts the user connected.

Follow how the user already works. The ways below are options; if the user has a routine that works, keep it.

## The plans

**Setting it up, once:**
1. manage_calendars get → the workspace's one calendar, its fields and channels
2. Where the user posts on a platform the calendar has no channel for, offer to add it (update channels). A channel is a label on the calendar: it connects no account and costs nothing. Suggested statuses fit most; add the user's stages (Filming, Editing) with organize_items create_statuses only where they help

**Planning a run of posts:**
1. Find out how far ahead, which platforms, how often, and what the posts are made from
2. manage_cards create per post {place: its day, title, body: what it is, fields: its time and channels, status}
3. Give the plan in the chat, one line per post by day; the posts the user picks are made with their own skills

**An ongoing session:**
1. manage_cards list {from, to} for the days in play, and get_workspace_changes for what others changed since
2. See what moved: posts finished, statuses changed, gaps in the coming days, and posts held for approval or failed (`manage_social_posts` list with status)
3. Do the session's work (make posts, write captions, move entries) and update each card as it changes
4. Report what is ready, what is waiting on whom, and what is open

**Placing a finished post:**
1. Render the post (render_timeline, render_canvas) so it is a file
2. manage_cards create or update {place: its day, attachments: the files in order, body: the caption and what goes with it, fields: its time and channels, status}

## Ways of working

- **Plan ahead:** ideas sit on dates and move through statuses to done. Good for campaigns, launches and teams with review. Most teams plan the next few weeks closely and later ones loosely, since posts fixed months ahead break at the first news day.
- **Ready only:** nothing goes on the calendar until it is finished. Good for creators who make in batches and use the calendar as the record of what is ready.
- **A mix:** the most common. Most days planned, a few slots left open for a trend, news or a reply to the audience.

The pace is the user's call: one post a week, or twenty in a day across platforms. If the user asks what pace helps, Buffer's 2025 data is a fair reference: Instagram and TikTok gain reach per post up to three to five a week and beyond, LinkedIn pays off at two to five, YouTube at about one long video a week. Consistency matters more than volume, and a pace the user can keep beats one they abandon. If posts keep sliding to the next day, the plan is ahead of what is being made.

## What an entry holds

**As a plan:** a working title, the format, the platform, the day, and in the body the idea in a line or two and what it is made from. Its series, pillar or campaign goes in the title or the body if the user groups posts that way.

**As a finished post:** the files in order (a carousel's slides in sequence, each slide's own text in its attachment text), the time, the channels, the status, and in the body everything the person posting needs:

- the caption, one per platform where they differ;
- the first comment, hashtags, mentions, the link and alt text, if the user uses them;
- notes for whoever approves it.

A plan turns into a finished post on the same card: attach the files and write the caption as the work comes in, then move the status on.

## Eversince's calendar

- **The calendar** is one per workspace and holds cards, each on a day with a time and the channels (platforms, each with its own mark) and showing its status. `manage_calendars` get names the fields and the channels, and reads a range of days.
- **A card** (`manage_cards`) is an entry with a title, a markdown body, attached library items in order with their own text, and a status from the workspace's list. It goes on a day with its time and channels, so one card follows a post from idea to done. The body holds the caption and notes; the attachments hold the files. update needs the revision from read, so if another member changed an entry, read it again instead of overwriting it.
- **A single finished file** goes on a day as a card with that file attached and no body.
- **What attaches:** library items: footage, rendered files, images, documents. Name a timeline or canvas still being edited in the card's body until you attach its render.
- **Series, pillars and campaigns** go in a card's title or body.
- **Order within a day:** by time, then as set (`manage_calendars` update order_on_day).
- **Review:** members and their agents see the same calendar. Put a comment where the work is (`manage_comments` on the timeline, canvas or file).
- **Days and times** are in the user's time zone.

## The hand-off

A finished entry holds what posting needs: the files and the caption in the body. Once the user approves it, `manage_social_posts` create with its card_id sends it now or schedules it to the accounts they connected. Where the workspace keeps agents' posts for approval (`manage_social_accounts` list, agents), the post comes back held on its card and goes out when a person approves it there. Where an account is not connected, the files' public addresses (`get_items`) and the caption go out by the route the user has. Once posted, move the status to Published.

A user who plans in another tool or posts straight from the platform may still find a calendar here useful as the place they and their agent work from: posts planned or placed next to the footage, then taken out to wherever they post. You can suggest it; don't assume it.

## Keeping it alive

- **A weekly look:** fill the gaps in the coming days, move what is stuck, see what is ready.
- **An idea bank:** keep undated ideas in a document (`save_document`) so the calendar holds only what has a day.
- **Repurposing:** spread one source video over several days and platforms instead of posting it all at once.
- **What kills calendars:** too many statuses or fields to keep up with, more planned than can be made, nobody owning the review, the same post on every platform, plans fixed too far ahead.

## The answer

In the chat: what you planned, placed or moved, by day; what is ready to post; what is waiting and on whom. The calendar in the app shows the rest.
