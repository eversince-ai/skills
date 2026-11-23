---
name: eversince-public-links
description: "Media, text or both on a page anyone opens without an account, for clients, an audience or teammates: what it holds and what a plan adds."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-public-links"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A public link is a page anyone opens without an account, on a phone or a computer. It shares media, written text, or both, with clients, an audience or teammates. What goes on it, who gets it and what happens after it is sent are the user's. This skill says what a page can be.

## The plan

1. Where the user has not said: what the page is for and who opens it. That decides what leads and how much goes on
2. A timeline or a canvas goes on as its rendered file: `render_timeline`, then `get_jobs` for the item; `render_canvas` returns its items in the result
3. `manage_public_links` create {items, title, project} → link_url, live at once, with the items in a default layout
4. To lay it out: get {link} → the blocks and a revision; update {link, blocks, revision, heading, description, style}; publish {link} puts a layout change live (settings reach the live page at once, a layout change waits for publish)
5. Hand over link_url

A page for one file is steps 3 and 5.

## What a page holds

A page is a column of blocks, in the order given:

- **Items** from the library, one block for any kind of file: one picture or video stands alone, several become a gallery, audio becomes a player with its tracklist, a CSV becomes a table, anything else a row to download. Each item can carry a line under it (note), a size in its row, a download button (off unless asked for) and its short id shown, so a comment can name which one it is about.
- **Headings and text**, the text in markdown: links, lists, quotes, code fences. A prompt to copy, instructions, credits, a story around the work.
- **An embed**: a YouTube, Vimeo, Loom, Figma or Spotify address.
- **A button** to any https or mailto address.
- **A divider.**

The page's look is a few choices (style): a cover picture across the top, light or dark or the reader's own setting, the font, the heading's size and place, and margins: normal for a page read down a column, small for a page that is one frame of work edge to edge. heading is what the page says at its top and what a pasted link previews with.

## Open, or with more

A page is open by default and complete that way: anyone with the link sees it, and it costs nothing to make or to open. A paid plan adds, each a choice:

- **Comments from visitors** (allow_comments), read back with get.
- **A passcode**, **an end date**, or **a name and email before the page opens** (permissions).
- **Numbers**: get returns how often the page was opened and by how many people; activity lists each person who left a name and email and each download, naming who downloaded which file where name and email are asked for.

A gate costs reach. Reports from marketers put the share of people who leave at a form at half or more, so a name and email fits something the visitor asked for and cannot get elsewhere, and where the user wants to know who came. An open page travels further and is shared on.

## A link over time

- The link stays the same while the page changes: update, then publish. People who have it see the new page.
- duplicate makes a second page from the first, for another audience or another version of the offer.
- regenerate_link issues a new address and ends the old one, for a link that reached the wrong people. unpublish takes the page down and keeps it; delete removes the link and its activity, never the media.
- A link belongs in the project of the work it shows (project on create); list finds it again.
- A file that is only on a computer is uploaded when the call comes through the desktop app on that computer, and listed in uploaded; otherwise it is left off and listed in local_only.

## Examples

None is the default; the same tools make all of them.

- **One thing to look at**: a video or an image on its own, small margins, the header off.
- **A collection**: several videos or images in a gallery, the strongest first, a line under each where it helps.
- **Text with media**: a written page with the videos, images or files it refers to set into it.
- **Files to take**: media and documents with their download buttons on, and text that is easy to copy.
- **Something a post promised**, sent by an automatic reply (`skill-inbox-and-auto-replies`).

## The check

- Everything meant to be on the page is on it: nothing in local_only, each timeline or canvas there as its render.
- The first thing a visitor sees is the thing the page is for.
- Downloads are on only where the user wants files taken.
- A layout change was published.

## Handover

- link_url, what is on the page and in what order, and whether it is open or what it asks of a visitor.
- Also possible: a cover and a look to match the brand kit, a second page for another audience, comments or a name and email on a paid plan, the page's numbers read later.
