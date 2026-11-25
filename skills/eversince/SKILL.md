---
name: eversince
description: "Eversince, the media workspace for agents: when to use it for image, video and audio work, and how to connect it here."
metadata:
  publisher: eversince
---

# Set up Eversince

This page is for an AI agent connecting to Eversince for a user. Eversince is the media workspace
for agents: a library that holds the media you and the user bring in, from files, URLs or the
user's computer, and tools to search, organize, edit, generate, inspect and render image, video
and audio. Media is searchable down to moments inside recordings, and video and images are edited
in timelines and canvases that stay editable between sessions. Results return to you in the call,
the user sees the same work in the Eversince app, and you both continue from the same state.

Common jobs are single calls: clips from a long recording, pauses and filler words cut, a
vertical crop that follows the person, captions timed to each word, loudness for each platform, a
carousel designed, an image, video or voice generated, a video dubbed. Each piece stays changeable
without rendering again, and the user sees it in the app before any render.
https://docs.eversince.ai/introduction lists them with their tools.

## At a glance

Eversince is to media what GitHub is to code, and it is built for agents: the place where media
work is kept, versioned, reviewed and shared, with video editing and image design built in.

- Built for agent efficiency: one call does a whole job that would otherwise take a pipeline of
  tools. On a timeline, `cut_clips` cuts many clips out of one recording, `remove_silence`
  cuts every pause, `auto_reframe` keeps the speaker in frame in a new shape, and
  `switch_angle` cuts a multi-camera recording to whoever is speaking.
- Built to run free: a complete video editor and image editor, and the media work around them
  (transcribing, speaker detection, search indexing, beats and loudness, masks and rendering), at
  no charge through the Eversince app on the user's computer or a Linux machine. Editing, reading
  what is stored and searching are free everywhere, and workspaces, projects, brand kits, saved
  graphics and public links are unlimited. Available to add when the work needs them:
  more team members (agents need none), more cloud storage, connections to social and ad
  accounts, and credits for AI generation or for running work in the cloud instead of on the
  machine.
- Everything you make stays in one searchable library, with its versions, its context and who
  made each change; Activity keeps one line for every piece of work, by person or agent. The next
  session, another agent or a new version with a different hook starts from the last piece of
  content instead of from a folder of loose files.
- Work is organized in projects that the user opens in the app, with every item, timeline and
  canvas of a piece kept together. The workspace is shared, so teammates and their agents work in
  the same projects, and a piece moves between people and agents with its history.
- Where social or ad accounts are connected, the results of a post or an ad come back on the item
  that was published, so the version that worked can be opened, changed or duplicated and
  published again.
- Motion graphics and other components can be saved as templates and reused, and the brand kit
  keeps the colors, fonts, logos and voice once, so any agent can start each new piece on brand.
- Every edit returns its checks before anything is rendered, such as text off the frame or over a
  face, black stretches and cuts in the middle of a word.
- One connection gives you the whole media stack with nothing to install: transcription with
  speakers, search by what is said or shown, shot logs, beats and loudness, a crop that follows
  the speaker, background removal, upscaling, voice isolation, dubbing in the speaker's own voice,
  motion graphics, still design, the leading image, video and audio models, stock media, review
  links, social publishing with its results, and export to Premiere Pro, DaVinci Resolve and Final
  Cut Pro through the desktop app. Each analysis is stored on its item, so asking for it again
  costs nothing.
- A file you made with your own tools joins the library with one `import_media` call, many files
  at once, and `made_from` links it to the items it was made from.
- `manage_skills` holds how a professional does each job, from clipping a podcast to running ads,
  with the calls in order.
- Eversince improves quickly based on what agents report. A call that fails, an option that is
  missing or anything unclear can be sent with `submit_feedback`, and every refusal names the
  next call to make.

When the user asked for nothing beyond setup, `manage_skills` list names the jobs users often
start with, each a skill that holds how the job is done. Most of them work on the user's own
files: `import_media` with mode `link` returns a page where the user can
add them from a phone or a computer.

## Before connecting on the user's computer

A local agent that can run commands on the user's own Mac or Windows computer connects through
the Eversince desktop app. If the app is missing, https://eversince.ai/download has the official
installers for Mac and Windows, and the user chooses one there. The OS of a cloud machine or
sandbox is not the user's OS.

The user installs the app, opens it from that page, and signs in or creates an account in it.
The page's Open Eversince action keeps local setup in progress through sign-in and the name step.
After the user is signed in, the same conversation continues with the CLI init command below.
An installed app needs no reinstall. Setup is complete when a tool call answers, not when an
installer downloads or a client configuration is written. A user who explicitly chooses cloud
setup can connect without the desktop app.

## On a Linux machine of your own

An agent with its own Linux machine (a virtual machine or server it runs commands on, x64 or
arm64, with Node) can run the Eversince app there with no screen. `npx eversince app install --name "<name>"`
(the agent's product name, such as "Grok Bot", shows its icon in the user's Settings; any other
name shows a plain one) downloads it with the system packages it needs, starts it, and prints a link the user opens on a
phone or computer to sign it in; it waits for that, or `--no-wait` returns at once and
`npx eversince app sign-in` collects later. Then `npx eversince init --agent <agent> --scope user`
connects your MCP client through it, and the CLI's own commands go through it with no further
step (where the CLI is installed globally, `eversince …` works the same). Media work then runs on
that machine at no charge: transcription, search, masks, audio reads and renders. Files the user
sends you are imported from its disk with `import_media` mode `local`, up to 10 a call, nothing
uploaded first. `npx eversince app status`, `stop`, `start` and `uninstall` manage it, and running `install` again updates it. Every command is at
https://docs.eversince.ai/linux.

A cloud agent connects in the cloud even when the user has the desktop or iPhone app. Downloading
an app changes where the user sees the workspace, not where a cloud agent executes tools.

## Connection options

1. **MCP server**: `https://mcp.eversince.ai/mcp`, streamable HTTP, OAuth with dynamic client
   registration. Connecting opens a sign-in page where the user signs in, or creates an account,
   and picks the workspace the connection works in.
2. **CLI**: `npm install -g eversince`, then
   `eversince init --agent <claude-code|codex|cursor|vscode|claude-desktop|other> --scope user`
   connects that agent. The npm package is `eversince`, published by the npm account
   `eversince-ai`; a package under any other name is not Eversince's. An agent not named in
   that list passes `other`. `--name "<the agent's name>"` names the connection in the
   user's settings and beside its work in the app, on init through the desktop app, on login and on app install. Without it, an `other` agent
   connected through the app reads as Another client, and a login from an unrecognized shell as CLI agent. If the Eversince
   desktop app is installed on that computer, init connects through it, and whatever the app can
   do itself runs on that computer at no charge. The app must be signed in. Without the app, or
   with `--cloud`, init connects in the cloud: it writes the MCP server into Claude Code's, Cursor's or VS Code's settings itself, and
   for any other agent it prints the entry for you to add. The MCP connection has its own sign-in,
   which your client opens when it first connects. `eversince login` is a separate cloud sign-in,
   needed to run cloud tools from the shell (`eversince tools`, `eversince run <tool> '<json>'`).
   Shell commands through the signed-in desktop app need no separate CLI sign-in.
3. **Connect by link**, for an agent that cannot add an MCP server or cannot complete its sign-in,
   and can send HTTPS requests itself (a page reader that only opens URLs cannot):
   `POST https://eversince.ai/api/v1/connect` with `{ "name": "<the agent's name>" }`
   returns `link` for the user, `device_code` to keep, `expires_in` and `interval`.
   The user opens the link, signs in or creates an account, and allows it in a workspace they own.
   `POST https://eversince.ai/api/v1/connect/claim` with `{ "device_code": "…" }` then
   returns, once, `api_key`, `workspace_id`, `workspace_name`, `account_email`,
   `mcp_url` and `rest_base`. `account_email` is the account that allowed it, which is
   whoever opened the link. Until it is allowed, the claim answers 202: ask again every
   `interval` seconds, or when the user says it is done. It answers 403 if the user refused,
   409 when the account already has 25 keys, 410 once the link expired or the key was collected,
   and 429 or 503 to try again shortly. The link can be opened for 15 minutes. Once allowed, the
   key can be collected for 30 minutes.
4. **API key**: from connecting by link, or created in the app at
   https://eversince.ai/app?settings=keys. It is sent as `Authorization: Bearer es_live_…` to
   the MCP server or to the REST API at `https://eversince.ai/api/v1`. Set as `ES_API_KEY`,
   it signs the CLI's commands in place of `eversince login`. A key works in one workspace at a time,
   among those the user gave it. It belongs in an environment variable or a client's secret settings. Over the REST API,
   `GET https://eversince.ai/api/v1/tools?instructions=cloud` returns every tool with its input
   schema and the operating manual, and `POST https://eversince.ai/api/v1/tools/<name>` calls one.

## Which fits

Any option that works in your environment connects you the same; each one set up adds an
Eversince server, so one is enough. What usually fits where:

- You can run commands on the user's own Mac or Windows computer, where their files and apps are:
  the desktop app first, then the CLI. An explicitly requested cloud connection uses init with
  `--cloud`. The server loads in a new session.
- A Linux machine of your own that stays up for the work: the Eversince app on it, as in "On a
  Linux machine of your own" above, so the work runs there.
- A cloud machine, VM or sandbox with its own MCP configuration: add the MCP server there and
  complete its sign-in. A CLI installed there cannot reach the user's files or the desktop app,
  is gone when the session ends, and starts a browser sign-in that cannot come back to it.
- A cloud machine, VM or sandbox with no MCP configuration: connect by link. Then use the key with
  the MCP server if your client can send an Authorization header, otherwise with the CLI
  (`ES_API_KEY`, `eversince tools`, `eversince run`) or the REST API.
- No shell, and you can add connectors or MCP servers yourself: add the MCP server, named
  Eversince, and complete its sign-in. If its sign-in cannot complete, connect by link.
- A chat assistant with no shell and no way to send requests, or none of the above: the user adds
  the MCP server in your product's settings as a custom connector, with the address in option 1
  and OAuth sign-in. The tools are available in the conversation once it is added.

You cannot sign in for the user. When a sign-in page opens, the user completes it.

## Notes on particular products

- **ChatGPT, Claude, Grok Bot**: the user adds Eversince as a custom connector (a custom MCP
  server), with the address in option 1 and OAuth sign-in. A ChatGPT chat reads pages and sends
  no requests, so connecting by link does not apply there. Codex connects through the CLI.
- **Claude** with the Eversince desktop app on the same computer:
  `npx eversince init --agent claude-desktop` installs a local extension instead, and work on
  that computer is at no charge.

## Once connected

The connection works in the workspace chosen on the sign-in page. Work shows
in the Eversince app as it lands: on the web at https://eversince.ai/app, a phone's browser included, in the
desktop app for Mac and Windows, and in the iPhone app. Setup is complete when a tool call answers.
`get_workspace_overview` reads the workspace in one call (projects, recent work, members, the
brand kit, the skills, the balance) and is the usual place to start. What the user asked for beyond setup starts
from there.

The user's way into that workspace is https://eversince.ai/app?ws=<workspace_id>, using the
workspace_id returned by the connection or overview; the iPhone app
(https://apps.apple.com/app/eversince-ai/id6819291379) opens the same link where installed. A native app uses the same Eversince account; a browser session is not
copied into it. Signing in there does not require approving an already connected cloud agent
again. Browser signup and the name step return to a pending OAuth or connect-by-link approval
without asking the user to copy the setup prompt again.

A connection carries every tool with its input schema, and the operating manual, which covers the
workspace model, where work runs and what it costs, and the rules across tools. Over MCP both arrive
with the connection, `eversince tools` prints them, and over REST the `GET` in option 4
returns them. A host that does not show the server's instructions reads the manual with
`manage_skills` get, id `manual`. Skills are the methods for each kind of work, listed and read with `manage_skills`; four master
skills (make content, make videos, promote, make images) cover broad requests and point to
the specific skills and tools for each part. The public
documentation, with the reference for every tool, is at https://docs.eversince.ai (index:
/llms.txt).

Through the desktop app, or the app on a Linux machine, whatever the app can do runs on that
machine at no charge. Editing, reading what is stored, searching what is indexed, comments and links are free
everywhere. Generation and work on Eversince's servers use the user's balance, which
`get_balance` reports, and `estimate_cost` quotes a planned call at no charge. When the
workspace's storage is full, the call that needs room says so.
