---
name: eversince-image-ads
description: "An image ad built on one angle, chosen by what the buyer already knows: headline, product, layout, sized for each placement."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-image-ads"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A static ad gets about a second, at thumb size, next to something more interesting. Everything here serves being understood that fast. It is one still with a headline and the product, sized for a placement, made one of two ways: composed on a canvas from real pictures and typed words, or generated whole from a prompt. A product photograph without copy is `skill-product-photography`; a thumbnail or poster is `skill-making-thumbnails`; a batch built to test arguments is `skill-ad-variations`, which holds the angles and the anatomy of one ad. A brief, a reference or a brand the user gives outranks every default here.

## The plan

0. `search_library` {query: the product} for the photo's ref; `manage_brand_kit` for the colors (each with its role: primary, accent, background, text) and fonts where the workspace keeps a kit; `get_templates` kind font lists every family the renderer draws
1. A cutout (`remove_background`, credits) only where the photo's ground does not suit the ad and the user agrees
2. `manage_canvases` {create: {destination: "instagram-post", name: "<product> · <angle>"}}, or {create: {from_canvas: a saved ad template, name}}; the response carries the canvas id, size and slide id
3. `edit_canvas` {canvas_id: from step 2, operations: [setCanvas background, addLayer shape {shape: "rounded", fill: {color}, box} as the panel, addLayer image {ref: the photo or the cutout} in its box, addLayer text {text: the headline, preset: "heading", font, color}, addLayer text {text: the subhead, preset: "subheading"}, addLayer image the mark]}
4. `inspect_canvas` {canvas_id: from step 2}
5. `publish_preview` {canvas_id: from step 2} for review; `render_canvas` {canvas_id, format: "png", title: "<product> · <angle> · 4:5"} only when it goes to the ad platform
6. `manage_canvases` {create: {from_canvas: from step 2, destination: "instagram-story"}}, then steps 4 and 5 on the copy, for each further placement

## What the job needs

- The thing sold, its price, who buys it and the words they use (reviews are the best source).
- The best real images: a cutout or packshot, a screenshot of the interface, a photo of the place, the person doing the work.
- The brand's colors as hex and its fonts by name, where it has them; a workspace that keeps them has them in its brand kit (`manage_brand_kit`).
- The placement. Decide the ratio before the layout: 4:5 for a feed, 1:1 for square placements, 9:16 for stories.

One ad targets one person and says one thing. When the money and the use come apart (the one who signs off, the one who lives with it, the one it is bought for), pick one and answer that person's objection. Written to all three, it reads as the corporate average.

Invent nothing on the image: no claim, number or customer quote beyond what the product does and what buyers said. The picture claims as loudly as the copy: a product shown larger than it is, an interface in a state the software never reaches, a before and after with a gap the product cannot produce.

## Angle

Start with the angle: the one reason this ad gives, sharpened into a claim. "Made without palm oil" is a fact; "the one a dermatologist won't flinch at" is an angle.

**Pick it by what the viewer already knows** (Eugene Schwartz's five stages of awareness, the frame direct-response copywriters still use):

- **Unaware** of the problem: show the problem or a story they recognize, the product small or late.
- **Problem-aware**: name the pain in their words and make it sharp, then point to the kind of fix.
- **Solution-aware**, comparing options: the mechanism, why this works where the others did not, or a comparison.
- **Product-aware**, not yet sold: answer the objection that stops them, with proof (a review, a result, a number, a demonstration).
- **Most aware**: the offer itself, the product and the deal.

Ad platforms now match each ad to the people it suits, so a few ads at different stages tend to beat one aimed at everyone.

**The six kinds of angle**: pain (the problem, specific, ended), outcome (life after), proof (someone it worked for, a result), switch (this against what they use now, on one difference), mechanism (why it works) and deal (the offer leads).

**The layouts that carry them**, the static formats ad accounts keep running: us against them (a two-column comparison, for switch), a customer's words as the headline (for proof), before and after (for outcome, only as far as the product goes), the product with three callouts on its features (for mechanism), a number with stakes (for proof or outcome), and the offer big with the product (for deal). A set of ads testing several angles is `skill-ad-variations`.

## Types of image ad

Ads that look like a post, a note or a screenshot get read; ads that look like ads get scrolled past. The types ad accounts keep running in 2026, each built on a canvas:

- **Native-looking**: a phone notes page, a text-message thread, a post or a DM as a screenshot, a sticky note or a handwritten card. Plain and low-fi on purpose; the words carry it. Use real words from the user or their customers.
- **Review**: a real customer quote or a review card with its stars, as the headline or the whole ad.
- **Us against them**: two columns, the product against the usual alternative, ticks and crosses on a few real differences.
- **Bold claim** or **question**: one line, very large, the product small.
- **Features and benefits**: the product in the middle, three or four callouts pointing at what each part does.
- **Before and after**: the change in one frame, only as far as the product goes.
- **Reasons why** or a **list**: "3 reasons" with short lines, the product beside them.
- **Offer**: the deal big, the product, an end date if there is one.
- **Clean product**: the product alone, beautifully lit, one short line.

Choose the angle first and the format to carry it. Accounts usually run still ads beside video.

## Remaking an ad that works

To build on ads already running in the category, pull them from the ad libraries: `pull_posts` {source: "meta_ads" (or tiktok_ads, linkedin_ads), query or account: the competitor or the category, media: true} (credits per page), and `keep_posts` the ones worth using. Ads that have run for a long time are usually the ones that work. Read one with `inspect_media` and `grid` to write its layout down as percentages: where the headline sits and how big, the product's place and size, the colors (`inspect_color` `points`), the type style. Then rebuild it on a canvas with the user's product, words and brand. How close to go is the user's call. The full method of writing a reference down and comparing the build is `skill-remake-from-a-reference`.

## Format

**The base is one of three.**

- **Graphic.** The product cut out on a built background, with control of color, layout and every element's place. For a verbal message, a layout of several elements, or a graphic brand look.
- **Photographic.** The product in a real place and real light, built on an actual photograph (a scene described from nothing comes back as stock). For when the context is the message, or the product should look owned rather than sold.
- **Screen-led.** A real screenshot as the evidence, in a device or framed as the object. For anything sold on a screen, where there is no package to shoot.

**The elements.** The floor is a headline and the product, and an ad can stop there. Add only what sharpens the message: a subhead, callouts, a price or offer, a badge, a comparison split, a list, a diagram, a hand, a person, a chart. Use two to six elements; the floor counts as two and a group counts once. A price, a badge or a callout is a text layer with the body preset and its own size and color, or a text layer on a shape. A chart carries real numbers, or is drawn so nobody could read it as a measurement: a shape that means growth is a device; the same shape with an axis is a claim.

Write the layout out by area before building it, positions as percentages, every text slot named: headline top 12%, product centered at 55% height, three callouts down the right edge, price bottom left. A centered product under a line of text is what you get when nobody decided anything. The ads that work show the argument: a comparison made literal, a before and after, a diagram of how it works, a text message or a search bar, a chart with one line that matters.

## Copy

Say three things, fast: what this is, who it is for, why they should care. The headline carries the angle, leads with the result, and names or signals the plain category, since a cold audience has never heard the brand's word for its own thing. One headline, at most one subhead; callouts and labels can be several, all short. Don't repeat what is legible on the packaging. Cover the image: the headline alone should say what is sold and roughly to whom. If it leaves a "so what", something else on the image answers it.

## What ad copy gets wrong

The left column is what comes out by default; the right is what a person would say. The pattern under every row: say what would be true in a photograph, and replace the feeling with the event that caused it. If a competitor could say it, it is not copy yet.

| The default | Why it fails | Instead |
|---|---|---|
| "Elevate your everyday" | A mood, not a promise; could sell anything | "Fits a laptop and a helmet" |
| "Revolutionary skincare technology" | Hype with nothing checkable | "The one that doesn't sting" |
| "Sustainably crafted for the modern home" | The brand's values, not the buyer's day | "Ships flat. Together in four minutes." |
| "Best-in-class battery life" | A superlative with no comparison behind it | "Two days. Not two days if you're careful." |
| "Say goodbye to back pain" | An outcome the product cannot guarantee | "Holds your lower back where a chair doesn't" |
| "I've been using this for two weeks and honestly, I'm obsessed" | Spoken enthusiasm with nothing under it | "Three weeks in and I still haven't had to reapply" |
| "This completely changed my routine" | A claim with no observable event | "I stopped keeping the old one in my bag" |
| "Perfect for busy people who want to feel confident" | A segment, not a person | "I get out the door before my kid finds his shoes" |
| "Hey guys, so today I want to talk about" | A greeting and a wind-up, scrolled past | "So this is the third one I've tried" |
| "Link in bio, you won't regret it" | Sells at the end, where the illusion breaks | "Anyway. That's it. That's the whole thing" |

## Design

Type, color, composition, depth and the defaults that read as generated are `skill-graphic-design`; its delivery checklist is the check on a finished ad. Specific to ads: the eye lands on the headline, then the product, then everything else, and size, weight and position set that order. In a long headline, lift the two or three words that carry it with an accent color, a heavier weight or a marker behind them. Put text over a photograph on the calmest part of the frame, with a real treatment behind it when it needs one (a panel, a lozenge, a scrim); don't fix contrast by recoloring the line. A set or a multipack shows the whole set somewhere in the ad.

On Meta, Advantage+ creative enhancements (some on by default) can crop, add text to or rework a still ad after it is built; an ad that must run as composed has them switched off in the ad account.

## Composed on a canvas

The exact route: real pictures placed unchanged, the brand's own fonts, every word editable afterwards, one layout resized to every placement.

To reuse an ad the workspace saved as a template, copy it with `manage_canvases` create `from_canvas` and replace its words and pictures. Use the photograph itself where its ground suits the ad; where it does not, offer a cutout (`remove_background`, credits). `manage_canvases` create takes a destination (instagram-post is 1080 by 1350, the 4:5 feed size; instagram-square, instagram-story; `get_canvas` destinations lists them) or a width and height. In `edit_canvas`, place the picture as an image layer by ref, the headline and subhead as text layers with a preset (heading, subheading, body) in the brand's font and color, a panel or lozenge as a shape layer under the words, and a mark as an image layer; boxes are slide pixels from the layout's percentages. A font the renderer lacks is refused with a list of the ones it has. `inspect_canvas` draws the slide as an export would and flags words cut off, overlapping or too small. `render_canvas` exports PNG, or JPEG for a photograph where size matters. For another placement, resize the same canvas: `manage_canvases` create with from_canvas and the new destination lays the layout out again, and the response names any layer that fell off the slide.

## Generated whole

Use this for a photographic or graphic base no real picture supplies, or a look no layout reproduces. Generation costs credits; price it first (`estimate_cost`). Read the chosen model's guidance in `list_models` before writing the prompt: it says how the model takes copy, references and ratios. What holds across models that render text at all:

- Put the exact copy in double quotes and describe the typography separately; paraphrased copy comes back paraphrased or misspelled. A model whose guidance does not say it renders text gets no text-heavy ad.
- Pass the subject as a reference (source_refs or reference_refs, as the model takes them) and don't describe it: describing its shape, size or packaging text makes the model redraw and distort it. With several references, give each its role. Where no usable image exists, make the shot first (`skill-product-photography`); fix a weak source rather than designing around it.
- Brand fonts go in as a picture: image models copy letterforms they are shown and load no font files. Pass a rendered specimen of the fonts as a reference marked as type only, and name the font in the prompt too. It moves the type toward the brand without reproducing it, so use a canvas when the type has to be exact.
- Pass the aspect ratio explicitly; otherwise some models take the shape of the first reference.
- Describe the finished image, never a correction: a fresh generation starts from nothing. An edit is different: the source is in front of the model (image-to-image, edit_points for a spot), so name the change and what must not move.
- Give positions as percentages, colors as hex and the case of every line: ambiguity is where the generic version comes from.
- Settle composition on a fast model; generate the final on one that holds detail and type.

For another ratio of a generated ad, use `reframe_image`, in credits (16:9, 9:16, 1:1, 4:3, 3:4, 21:9; 4:5 is a canvas resize). It extends the scene rather than cropping it.

## Sharing for review

Review each ad in the app (`publish_preview`), with comments on it. For people outside the workspace, `manage_public_links` create with the rendered items in order makes a review page anyone can open without an account, live at once. Comments on the page arrive as threads on the link (`get_comments` with `link_id`); reply in the thread to answer them. To bring in a catalog of products for a set of ads, use `import_store_products` (`skill-product-photography`).

## The check

On the rendered image (`inspect_canvas` for a canvas, `inspect_media` for a generated item):

- **Thumbnail.** At the size it runs at, the headline still reads and the product is still identifiable.
- **One second.** Glance away and back: the angle should come through. If not, the hierarchy is wrong.
- **Cover the image.** The copy alone says what it is, who it is for and why care.

Trace a failure back to the decision that caused it. A prompt written as a correction to the last attempt describes an image the model has never seen.

## Handover

- A composed ad: the placement's ratio (4:5 feed, 1:1 square, 9:16 story), the check passed (`inspect_canvas`), the canvas in the app with `publish_preview` for review, and a PNG from `render_canvas` only when it goes to the ad platform.
- A generated ad: every word on the image spelled as the quoted copy, and the product's shape, label and color as its reference shows them.
- Several placements: one canvas per size, each checked and in the app.
- Also possible: variations on the headline or the angle (`skill-ad-variations`), the other placements' sizes, a video version (`skill-ugc-ads`), a version in another language, a review page for people outside the workspace.
