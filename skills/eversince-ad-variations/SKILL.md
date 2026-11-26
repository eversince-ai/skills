---
name: eversince-ad-variations
description: "New versions of an ad that works, or a set to test: angles first, each variant one stated change a platform sees as different."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-ad-variations"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A performance ad asks someone to act now. Make it as a set: one ad is an opinion, a set is an experiment. Each ad answers a question you can say out loud: which reason to buy moves this audience, which opening earns the rest, which proof lands. This skill decides what each variant is and why. How to make a variant is its format's skill, `skill-ugc-ads` or `skill-image-ads`; an organic short is `skill-short-form-videos`. A brief, a reference or a brand from the user outranks every default here.

## The plan

The winner is a timeline whose first span is the hook. A winner that exists only as an item is placed on a new timeline first (`manage_timelines` create with `clips`: [{ref}]); one running on Meta, TikTok or LinkedIn is found with `pull_posts` from its ad library and kept with `keep_posts`, then placed the same way; the round is N copies with a new opening each, everything after it held constant. Hooks are written first, one per copy, different in kind: a spoken ad varies its first line or first shot, an ad with no speech its first shot and its hook text. Each comes by the routes above, cut to the old hook's length so nothing after it moves.

1. `manage_timelines` {create: {parent: the winner's timeline id}}, once per hook, each a tab beside the winner; each response carries the copy's id, its tab, tracks and clips
2. `edit_timeline` {timeline_id: from step 1, operations: [overwriteRange {trackIds: every track the hook span holds, from: 0, durationInFrames: the span}, addClip {trackId: the hook's track, ref: the new take or shot, from: 0, source: its span}, and for hook text addClip {trackId: the old text's track, clip: {type: "text", preset: "title", content, from: 0, durationInFrames}}]}, per copy; a take shorter than the span leaves a gap that rippleDeleteRange {from: the take's end, durationInFrames: what is left} closes across every track, a longer one is trimmed to it
3. `captions` generate {timeline_id: from step 1, template: the winner's caption template}, per copy with speech, since a caption track is drawn from the speech as it now is
4. `check_timeline` {timeline_ids: every copy from step 1}: the one check for the round; a sheet (`inspect_timeline` with `sheet: true`) only where a finding names a clip, the user asked for the work to be verified or nobody sees the round before it runs
5. `publish_preview` {timeline_id: the winner's}, once: the winner and every copy as tiles of one preview; then `render_timeline` {timeline_id, idempotency_key: minted per copy} per copy when the videos go to the ad platform, and `get_jobs` {ids} returns the rendered items
6. `update_items` {updates: [{ref: each rendered item, name: "<angle> · <hook> · v1"}]}
7. `organize_items` {group: [{refs: the rendered items}]}

A still round runs the same shape on canvases: `manage_canvases` create with from_canvas per variant, `edit_canvas` patchLayer on the headline layer (or the picture, from the library or `generate_image`), `publish_preview` per canvas, `render_canvas` when the files go to the platform, then steps 6 and 7.

## What the job needs

- The thing sold, the offer, who it is for, and the action the ad asks for: a purchase, a signup, an install, a booking. The action decides what counts as proof and how the ad can close.
- The audience's own words, found, not written: reviews, complaints about the problem, support threads, competitors' reviews.
- What already ran: this brand's past ads and the market around it. Offer to read what competitors are running; do not do it unasked. `pull_posts` reads the ad libraries (meta_ads, tiktok_ads, linkedin_ads; credits, a fraction of one a page); `keep_posts` keeps chosen ones within two days of the pull. Do not rerun a variant that already lost, or test a question the market has already answered.
- Two metrics, named before making the set: the one the round is judged on, and one watched beside it, so a variant that wins attention while losing buyers is not called a winner.

## Read the working ad first

Before planning variations of a working ad, write down why it works: its angle, its hook (the first line and the first frame), the proof it shows, its length, its format, who is on screen and its ask. Read a video with `get_transcript` or `transcribe_media` and `inspect_media` overview; a still with `inspect_media`. Its numbers come from the ad account where it is connected in Eversince (`read_ad_results` ad: click rate and cost, with breakdowns on Meta by each picture, video and text of the ad), otherwise from the user where they have them (hook rate, click rate, cost per result). Keep what makes it work and change one thing on purpose.

## What the ad platforms count as different

Meta groups ads that look and feel alike and delivers them as one. Its own example of "the same creative" is a new headline or call to action on the same video, and TikTok's delivery behaves similarly. So a variation that changes only wording teaches nothing there. A variation counts when something a viewer sees in the first second changes: a new opening shot, another person or setting, another angle, another format (a still beside a video). Agencies running Meta in 2026 tend to keep 10 to 15 genuinely different ads live in a campaign and add new ones every two to three weeks. These are working numbers, not Meta's.

## Angles before executions

An angle is the reason to act that the ad argues. Which argument runs matters more than how well any one is made, so settle and test angles first. Six that differ in kind:

- **Pain.** The problem, sharp and specific; the product ends it. For audiences who feel the problem daily and have not named a solution.
- **Outcome.** Life on the other side of the change. For a change that is desirable and visible.
- **Proof.** Someone it worked for, a count of buyers, a result shown happening. For skeptics and considered purchases.
- **Switch.** This against what they use now, on one defensible difference. For buyers already paying a competitor.
- **Mechanism.** Why it works when the others did not. For categories full of broken promises.
- **Deal.** The offer leads. For an offer that is genuinely strong; when it wins it teaches nothing about the product.

Match the angle to what the audience already knows: unaware of a solution, pain and outcome; tried things and let down, mechanism and proof; choosing between options, switch; knows the brand, the deal. Existing material (a shoot, a founder on tape) decides which angles can be argued. Run the ones it can prove, name the ones with nothing to show as ruled out, and mine its language the way you mine reviews.

One ad argues one problem, not two. Where the business has many, pick the ones with the most surprise (a cause nobody connects to the effect: the musty smell that is the roof), the most money at stake, the most people feeling it right now, and a cause one shot can show (a thermal image, a macro of the damage). The kind of problem suggests the form: a hidden cause, a testimonial or a problem-first ad; a cost that grows with time, an expert on the job; an everyday absurdity, comedy; a visible change, a before and after; distrust of the category, a first-person account of the old way against the new. Keep every line in the target business's own world: its places, objects and habits, nothing carried over from another niche's example.

## The anatomy of one ad

Three jobs in order, each earning the next, in any format.

- **The hook** stops the right person. The audience is cold and mid-scroll, so say who this is for and what it is about in the first moment: in video the first seconds and the first spoken line, in a still the headline and image reading as one. A hook that only makes sense after the product appears is too vague; one that could open any product's ad stops nobody. Open on a number only when it carries stakes (hours back, money lost, how much faster), never as inventory. Open on a situation, not on the viewer: "are you still struggling with" reads as an accusation, reaches people the ad was not aimed at, and is the shape platforms refuse most. The moment reaches the same buyer: the third coffee gone cold, the drawer of half-used bottles.
- **The proof** makes one claim believable: something shown or said that a viewer can picture happening, not an adjective. One claim per ad; cut everything that does not serve it. Cuts make claims too: a dull shot followed by a bright one says the product did that, so hold a juxtaposition to the same standard as a sentence.
- **The close** asks plainly for one action and states the offer. Say the offer removes what stops the action (a free trial, no card, free returns) only where it is true. An ad with no ask is a brand ad, not a performance ad.

Invent nothing: no customer's words, no number, no claim beyond what the product does and what real buyers said. A generated person on camera speaks in the first person like anyone in a feed, and stops where a line needs evidence: a measured result, anything about a body, a named comparison. A claim that needs a real user needs a real user. Declare an AI-made ad where the platform requires it.

## The set

Vary one lever at a time, or the result teaches nothing. The exception: when angles are on trial, run each behind the hook that serves it best, since a weak opening says nothing about the argument. Surface identity (the creator, the room, the wording of lines that are not the lever) can vary freely, because viewers tire of repetition faster than a platform reads a result.

The ladder, in order:

1. **Angles.** Different arguments, each behind its best hook. The first round on anything new.
2. **Hooks on a fixed body.** The cheapest round, and where an angle that earned attention gains most. Hooks differ in kind, not wording: a number, the pain named, a question, a situation, the product doing something spectacular. On Meta a new line over the same first shot is delivered as the same ad, so give each hook its own first shot.
3. **Executions.** Format, length, style and ratio of a winner. Last, because it refines a known winner. The placement sets the ratio: 9:16 for stories and short-form, 4:5 for feeds, 1:1 where a placement takes square, 16:9 inside long video. Recompose a video winner into each ratio on a copy beside it (`manage_timelines` create with `from`, `parent` and `aspect_ratio`), then `auto_reframe`. Resize a still on its canvas (`manage_canvases` create with from_canvas and a destination); `reframe_image` carries 16:9, 9:16, 1:1, 4:3, 3:4 and 21:9, not 4:5.

Write the grid before making anything: each variant, the question it answers, what it holds constant. The grid is what the round costs, in generations or in cutting; cut any variant nobody can learn from. The smallest set that answers the first question is one variant per angle on one body. With no size given, a round is 3 to 5 variants.

## Where a variant comes from

An ad is a video or a still, and each has two routes. The user picks the route.

- **A video, from what exists.** Re-cut the ad's own material: another moment as the opening or a different first shot, with the hook's line spoken or set on it as a text clip. A new line over the same opening shot is a wording change, not a new hook. The user's other footage (`search_footage`, `search_library`) and stock (`search_stock`) work the same way. No credits.
- **A video, generated.** One generated part (a hook take, a product shot, a scene) or the whole ad, made the way `skill-ugc-ads` makes its beats. Uses credits.
- **A still, edited.** On a canvas copy (`manage_canvases` create with from_canvas): a new headline, another picture from the library, another layout or color, another shape. No credits; the craft is `skill-image-ads`.
- **A still, generated.** A new picture from `generate_image` set into the canvas, or the whole still generated. Uses credits.

## The next round

The variants run from an ad account connected in Eversince (`manage_social_accounts` list; the user's choice, charged a month), each made into an ad from its library item with `manage_ads`, or from the platform's own tools with the rendered files. Results live where the ads run; a connected ad account reads them here (`read_ad_results` ranked, ad). Reading a running ad and deciding to leave, end or remake it is `skill-run-ads`. Because every variant was a stated question, the grid turns the numbers into which angle won, not which file won. Iterate on a winner: new hooks on the winning angle, the winning proof pushed harder. Kill the losers; state what each taught. A next round that changes three things at once starts the experiment over. Winners fatigue; replace a slowing control with the unrun rows of the grid.

Name each variant's item with its angle, hook and version (`update_items` name) to map a platform's report back to the grid. `organize_items` group makes a round's variants one group in the library, so the next round is told apart from this one. Both are optional records in the workspace; the grid lives wherever the account keeps its plans.

## The check

- Every variant answers a stated question, and the grid says which.
- Each ad makes one claim, and the proof is something a viewer can picture.
- The hook makes who it is for and what it is about unmistakable in the first moment.
- Every ad ends on one concrete ask.
- Nothing in the set repeats a variant that already lost.

## Handover

- Every ad at its placement's shape: 9:16 for Reels, Stories and TikTok, 1:1 or 4:5 in a feed.
- Caption a spoken video where the user wants captions (the usual choice for muted placements), its speech at delivery level on `check_timeline`; no captions where nothing is spoken. Name a stock opening as stock.
- Each variant in the app, where it plays: a copy made with `parent` is a tile on the winner's preview, after the winner, and `publish_preview` with its id returns that preview and names the tile. When the variants go to the ad platform, render with `render_timeline` or `render_canvas`, name each "<angle> · <hook> · v1" and group them with `organize_items` group.
- Also possible: a round of angles before hooks, each winner in every placement's shape, still versions of a video ad, generated parts or fully generated variants, the competitors' current ads read from the ad libraries.
