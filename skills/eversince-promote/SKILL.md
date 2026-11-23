---
name: eversince-promote
description: "Videos and images that sell a product, release, event or service: what to know first, which kind fits, and the skill for each."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-promote"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill covers videos and images meant to sell: a product, a release or update, an event, a service, a course, a recording going out. It lists what helps to know, which kind of video or image fits which goal, the skill for each, and how a set for one launch is planned.

Every step is optional. A user who names what they want ("a 15-second ad for the new serum") gets exactly that; for a user who names only the goal ("help me launch this"), what to make can be proposed first. What the agent already knows about the user, their product and their buyers comes before any question.

## What helps to know

Used where the agent has it or the work needs it; none of it is a precondition for starting.

- **The product**: what it is, what is new or better about it, and the proof (a demo, a number, a review, the product itself). Read from what the user gave or pointed to: their page, release notes or a brief (`read_document`), the product's files in the library (`search_library`, `search_footage`).
- **The buyer**: who it is for and what they want, in their own words. Where it matters and the user wants it, `skill-social-trends` reads what the field runs and what buyers say, `skill-account-audit` one competitor's account and, when asked, its ads (ad libraries and posts through `pull_posts`, in AI credits).
- **Where it runs**: the platforms and placements (a feed, a story, a store page, a landing page, an ad account), which set the sizes and lengths. An ad account the user connected takes the finished file directly (`manage_ads`: create from the file, or boost a post once it is out), spending from their own ad account; where the workspace keeps agents' ads for approval (`manage_social_accounts` list, agents), the ad is made paused and a person switches it on in the app. What each ad did reads back with `read_ad_results`. With no ad account connected, the file goes to the platform's own tools.
- **The material**: the user's own footage and photos first; the brand kit (`manage_brand_kit`) for colors, fonts and logo; an online shop's products through `import_store_products`; stock (`search_stock`) and AI generation (credits) where the material runs short, with anything generated said so in the proposal.

## Which video or image for which goal

- **A narrated case for a product, release or update**, with real screens as proof: `skill-launch-videos`.
- **The product as the star**, the picture carrying it, few words: `skill-product-videos`.
- **Making people want a release without explaining it**: `skill-teasers-and-trailers`.
- **A video ad that looks like a real person filming**: `skill-ugc-ads`.
- **An image ad**, headline and product on a layout: `skill-image-ads`.
- **Several ad versions to test against each other**: `skill-ad-variations`.
- **Ads launched, read and improved once they are made**: `skill-run-ads`.
- **Photos of the product itself** (packshot, lifestyle, detail): `skill-product-photography`.
- **A store listing** (main image, gallery, video): `skill-product-photography`, A store listing; posts across days: `skill-content-calendar`.
- **A short social video that informs rather than sells**: `skill-short-form-videos`.

Anything that fits none of these is made with the tools directly: a timeline (`manage_timelines`, `edit_timeline`), a canvas (`manage_canvases`, `edit_canvas`), graphics (`compose_motion`), AI generation (`generate_image`, `generate_video`, `generate_audio`).

## A set for one launch

When the user asks for the set (a launch, an episode going out, an update shipped), it is proposed before it is made:

- One line per video or image: what it is and its shape, what it is made from (the user's own material first, stock where it is thin, generation marked as generated), and what it costs: free, cloud processing, or credits with an estimate (`estimate_cost`). Then the total for the set.
- One cut from another comes after it: the launch video before its clips and stills.
- The user picks, changes or adds; the list as agreed is the plan, and only those are made, each by its own skill.
- A set the user wants kept together gets one project (`new_project` on the calls that make it) holding the rendered files and one document (`save_document`) with the plan, each one as it is finished and the post text.

## Along the way

- What the user asked for is made; a bigger set is offered, not made.
- Where several videos and images come from one launch, the long video is made before the shorter versions and images cut from it.
- Nothing is claimed that the material does not support: no invented numbers, reviews or features.
- Each result shows in the app (`publish_preview`), where the user reviews it; changes are made to the same timeline or canvas. Files are rendered (`render_timeline`, `render_canvas`) at each placement's size when they leave Eversince.

## Handover

- Each video or image in the app.
- Also possible: shorter versions and other sizes, versions on other angles, a thumbnail or cover, the post text, captions, the same set for the next release.
