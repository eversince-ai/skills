---
name: eversince-keeping-content-organized
description: "A workspace structure agreed with the user and held every session: projects, labels, names, a calendar; media in and found."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-keeping-content-organized"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

An organized workspace is one where anyone, the user or any agent in any session, can find a thing
in seconds and knows where a new one goes. That takes a small structure agreed with the user,
written down where every agent reads it, and held to every time. A workspace with no structure works
too: structure is added when the user wants it, and what the user already built is followed, not
replaced.

## The plan

1. Read what is there: `get_workspace_overview` returns the projects, the statuses, the fields, the calendar, and the counts
2. Learn how the user works, in a few questions: what they make, for whom (one brand, several clients), where it goes, and whether they plan ahead
3. Propose a small structure in the chat and agree it: projects and the folders inside them, the statuses and fields that sort across them, the calendar if they plan, and how things are named
4. Build it: `manage_projects` create with a description, update with its folders; `organize_items` {create_statuses, update_statuses, create_fields}; `manage_calendars` where they plan. Each project's description says what goes in it and how it is named, since that is what the next session reads
5. Place what exists, with the user's yes (a request that already says where things go is that yes): `search_library` by type, date or words, then `manage_projects` add, and `update_items` to rename or tag. Footage with no useful name is sorted by what it shows: `search_footage` for a subject across the videos, `inspect_media` to see each one (both free through the desktop app, on the computer)
6. Hold to it: every later session starts from `get_workspace_overview`, passes the agreed project and folder when it brings in or makes something, and names it the agreed way; something that fits nowhere gets a question or a proposed new project, not a guess

## Light for the user

The user should never have to organize. They agree a structure once, in a few answers, and the
agent places, names, tags and dates things quietly as it works, every session. Nothing here is needed
to use Eversince: with no answer, leave new items in no project and carry on. Ask about structure when
the work calls for it (a hundred clips from one shoot, several clients, a month of posts), not before
every job.

## What the app offers

Everything below is optional; use what fits how the user works.

- **Projects**, each one body of work with a description, holding its timelines, canvases, links and items, in nested folders if wanted (`manage_projects`), each in any number.
- **Statuses**, the state of each timeline, canvas, card or item, from one list the user can change (`organize_items`). An item's status is the same in every project that holds it, so on items it suits ones under review. `manage_timelines` list and `get_canvas` filter by status, and `get_workspace_overview` counts each status's items and pieces.
- **Fields**, typed columns: text, number, date, select, multi-select, checkbox, link, each with a description (`organize_items` create_fields).
- **On each item**: a name, a description, tags people search by, field values (`update_items`); **favorites** for the user's picks, **hide** to keep something out of the way without deleting it, **groups** to show related items together (`organize_items` favorite, hide, group).
- **Variations and sizes**: a timeline holds its variations and a canvas its other sizes as tabs (`manage_timelines` or `manage_canvases` create with parent), so cuts of one piece stay together instead of filling a project with copies.
- **Where things came from**: each item records what it was made from (`get_items`), so a render, a cutout or a generation leads back to its source.
- **Reusable pieces**: saved graphics and templates (`manage_templates`, `compose_motion` save_as) and the workspace's own skills, so a look or a method is kept once and reused.
- **Plans and notes**: the calendar and its cards for work ahead, documents for briefs and notes, comments for review.
- **Search over everything**: items by words, tags, transcript or picture; moments inside recordings; duplicates (below).
- **What an item is**: an upload, a generation, a render, stock or a pulled post, recorded on the item whatever project it is in (`search_library` source).

## A structure that holds

- **Shallow**: a project and one or two levels of folders. Deep trees are where things get lost.
- **One project per body of work**: what cuts across projects (a client, a platform) is a field, and where a piece stands is its status, so both can be filtered without moving anything.
- **Divided the way the work is**: a project per client, series, campaign or shoot; inside it, raw footage apart from finished pieces, as media teams commonly keep them.
- **Status, not in a name**: a status changes in one click; one written into an item name goes stale.
- **Names that sort and read**: a date first (2026-11-12), then the project, then what it is, then a version where there are several ("2026-11-12 Spring launch teaser v2"). The same pattern everywhere.
- **Written down**: each project's description and each field's says what it is for, so a new session, another agent or a teammate understands it without asking.
- **Nothing to start with**: an item brought in or made with no project named is in the library alone.

## Bringing media in

- **From the user, any device**: `import_media` link makes an upload page they open on a computer or a phone; `search_library` upload_link lists what came through it.
- **From a web address**: `import_media` url. A social post is not an item: its link goes to `pull_posts` as account, and `keep_posts` keeps it. A YouTube video has no file to keep: `keep_posts` keeps its thumbnail and link, and `pull_posts` transcript_of reads its words.
- **From the user's computer through the desktop app**: `import_media` local with the file's path, nothing uploaded.
- **Into the right place from the start**: pass the agreed project, and folder inside it, on import.
- **A whole archive** (a channel, an account, a drive, years of folders): `skill-searching-your-backlog`.

## Finding things

- **Items** by words, tags, transcript or what a picture shows: `search_library` (filters by type, project and its folder, status, field, date; sort starred for the user's picks).
- **Moments inside recordings** ("where she talks about pricing", "the shot of the bridge"): `search_footage`, over videos read for search (`prepare_media` search_index starts that read).
- **What changed since last time**: `get_workspace_changes` with the last cursor.
- **Duplicates**: `search_library` duplicates_of.
- **An item's details**, where it came from and its comments: `get_items`.

## Planning work

- **The calendar** for work by day, one per workspace: `manage_calendars`; an item goes on a day attached to a card (`manage_cards`).
- **Cards** for work that is not one item (an idea, a brief, a post with its items and text): `manage_cards`.
- **Pins** put a project or a folder first, for everyone (`manage_projects` pinned, pinned_folders); single items are favorited.
- **Comments** for review and questions that outlast the session: `manage_comments`; `get_comments` with waiting_for_me for what is addressed to this agent.
- **A document** for a plan, a brief or notes beside the work: `save_document`, read back with `read_document`.

## What not to do

- Restructure, move or rename what the user arranged without their yes.
- Build a structure the user did not ask for; propose it in a line instead.
- Delete anything other than what this agent made or what the user named (`delete_items`). `manage_projects` delete alone removes only the project; with delete_work, delete_items or delete_links it also deletes what nothing else holds, and the result lists each kept item and what holds it.

## The check

- `get_workspace_overview` read back shows the agreed projects, each with a description that says what goes in it.
- New items from this session sit in their projects and folders, named the agreed way.
- Nothing the user arranged was moved without their yes.

## Handover

- The structure as agreed, in a few lines, and a link to each project in the app.
- Also possible: the library read for search so moments can be found, a calendar for the work ahead, statuses or fields that fit how the user sorts.
