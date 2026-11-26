---
name: eversince-brand-style
description: "A brand's colors, fonts, logos and look, read from what the user has, agreed, and saved in the brand kit for everything made later."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-brand-style"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A brand style is a few fixed decisions: the colors and what each one does, the fonts and what each
one sets, the logos, the look in a sentence, and a short list of do's and don'ts. Saved in the brand
kit, they are what any agent in the workspace reads before it makes something for the brand, so every
video and image looks like it came from the same place. A brief or guidelines from the user outrank every default here.

## The plan

1. Read what exists: `manage_brand_kit` {} returns the active kit and lists every kit
2. Gather what the user has: logo files, brand guidelines (`read_document` on a PDF in the library), past posts (`pull_posts` on their account, or their library), their website (read with your own web tools, outside Eversince)
3. Read the colors and fonts off that material (below)
4. Propose the style in the chat: the palette with roles, two fonts, the look in a sentence, a few do's and don'ts; the user agrees or changes it
5. Save it: `manage_brand_kit` {colors, fonts, add_items: the logos with role "logo" and a name each, add_context: the look and the rules}
6. Pin only what the user wants to reuse: a caption template, a text style such as a lower third, a saved graphic component, a voice (`manage_brand_kit` pin, one call each, with a note on when it applies)
7. Where the user wants to see it working: one sample image built with it (`skill-graphic-design`)

Save what the user agreed to keep, nothing more. A section with nothing agreed stays empty, and a one-off choice for a single video or image stays with it.

## Reading a brand from what exists

- **Guidelines or a website come first.** Hex codes and font names written there are exact. A website's own styles carry its real colors and fonts; read them with your web tools.
- **Colors from a picture are estimates.** `inspect_color` with `points` on the logo or on a post gives a hex at each point; say they were read from a picture.
- **Past posts show the habit**: the ground they use, the accent that repeats, how the words sit. The look is what repeats across them, not one post.
- **Fonts.** The kit takes families from the renderer's catalog; an unknown family is refused with the list. Where the brand's font is not there, pick the closest in the catalog (same style: geometric or humanist sans, serif, slab, condensed) and say it is a stand-in.
- Nothing is invented: a section with no source stays empty or is proposed as a suggestion, named as one.

## The decisions

- **Colors, with roles.** Usually a primary (often the logo's), one or two secondaries, one accent for highlights and calls to action, a background and a text color. Fewer colors used with discipline read stronger than many. Check text against its background: at least 4.5:1 for body text and 3:1 for large type (WCAG).
- **Fonts, with roles.** A headline face and a body face, or one family at two weights. Two faces that look alike give no hierarchy; pair faces that differ in more than one way (serif and sans, condensed and wide).
- **Logos.** Each version the brand uses (full, icon, on dark, on light), with a name that says which is which.
- **The look, in a sentence**, anchored to something a designer would recognize ("editorial, warm paper, one red accent", "bold condensed type on black"). With no brand yet, `skill-design-looks` has ten named looks to start from, and `skill-graphic-design` has palettes by subject.
- **Do's and don'ts**: three to six, the ones that keep the work consistent (the logo never on a busy photo, captions always in the accent, no gradients).
- **The words**: names the brand spells its own way go in `words`, so transcripts come back right.

## Using it in all later work

The active kit is the one read when no kit is named. Later work in the workspace reads it first: the
fonts, colors and logos, the context block with the look and the rules, and the pins with their
notes. How strictly a video or image follows the kit is the user's call: a brief for one can step
outside it. A workspace with several
brands keeps a kit per brand (`create_kit`, `activate_kit`).

## The check

- Every color has a role, and text and background pairs pass the contrast numbers above.
- Every font is in the catalog, or named as a stand-in.
- Logos are in the kit with names that tell them apart.
- `manage_brand_kit` {} read back shows what was agreed, nothing more.

## Handover

- The style as saved, in a few lines: the palette with roles, the fonts, the logos, the look, the rules.
- What was estimated (a color read from a picture, a stand-in font) and what the user should confirm.
- Also possible: a sample post or thumbnail in the style, a caption template and a lower third pinned, a kit for a second brand.
