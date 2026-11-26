---
name: eversince-run-ads
description: "Ads launched, read and improved: what each creative did and why, then left, ended or remade from its source. Making a set is ad variations."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-run-ads"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Running ads is three jobs that repeat: put distinct creatives in front of buyers, read which ones worked and why, and make the next ones from what the read says. The third is where most accounts stall, because the ad's numbers sit in the ad platform and its footage somewhere else. Here they sit together: an ad is made from a library item, its results read back beside it, and the next version is cut from the same source. The loop is the same on every connected ad platform (Meta, TikTok, Google, LinkedIn, Pinterest, X, Reddit, ChatGPT); the numbers and habits are each platform's own, Meta's first below since most of today's practice was worked out there, the others after. The spend is the user's, on their own ad account. Connecting an ad account here is the user's choice, charged a month; with none connected the method is the same, the ads made in the platform's own tools from the rendered files and the numbers read from there or from what the user shares. A brief, a budget or a structure the user already runs outranks everything here: where the user has a way of running ads, the work is done their way, and what follows is what is known to work where they have none.

## The plan

1. `manage_social_accounts` list → the ad account connections, and under agents whether an agent's ads go out or are made paused for a person to switch on in the app
2. `manage_ads` ad_accounts {account_id}, then health {account_id, ad_account_id} before anything is created; `manage_ad_tracking` pixels and match_quality where the goal is a sale or a lead counted on a site
3. `read_ad_results` tree {account_id, ad_account_id, fields: {from_date, to_date}} → what already runs and what it did; ranked {fields: {sort_by}} for the ads in order, timeline for a campaign day by day. An account with history is read before anything new is made
4. With the user: how they run ads today and what stays as it is (their campaigns, audiences, budgets). Then the round (below): the goal, the budget, how many distinct creatives, and where they go: a new ad set, or an ad set that already delivers
5. The creatives: each a rendered library item (`render_timeline`, `render_canvas`), named so a report maps back to it ("<angle> · <hook> · v1")
6. `manage_ads` fields {of: "create"} → what create takes on this platform; fields with field reads inside one (targeting, placements)
7. `manage_ads` create {account_id, ad_account_id, fields: {name, goal, budget_amount, budget_type, budget_level, creatives: [one per item, each with its own headline, body, link_url and call_to_action], targeting or advantage_audience, validate_only: true}} → the check with nothing made; then the same call without validate_only. A creative added to an ad set that already delivers is create with ad_set_id; a post already out is promoted with boost (boostable_posts lists them); ad_preview {id} shows an ad as its placements draw it, and review its state in the platform's review
8. Wait for the read (below), then `read_ad_results` ad {id, fields: {breakdowns}} per ad, and insights for what the tree does not carry
9. Act on each ad: leave it, pause it (`manage_ads` ad_status with status paused) or end it for good (end), copy a winner into another ad set (duplicate_ad, made paused), or remake it from its source and add the new version with step 7
10. Report: what ran, what each creative taught, what was changed, what runs next

Each tool's fields action lists what an action takes on this platform. Ad writes go one at a time. Where ads are made paused for approval, the report names the ads waiting for the user's switch.

## On Meta: what decides the result

Meta's delivery picks, for each person, a few thousand candidates out of tens of millions of ads before any ranking happens (its Andromeda retrieval system, described by Meta's engineers in December 2024). The practical effect, as buyers describe it: the creative does the targeting. An ad about a new parent's 3 a.m. problem finds new parents; an ad set's audience settings matter less than what the ads show and say.

Two things follow.

- **Distinct concepts, not variants.** Ads that look and sound alike are treated as one ad and compete for the same people. Five versions of one video with different headlines are one creative. A concept is distinct when it changes who it speaks to or what it argues: another stage of awareness (does not know the problem, knows the problem, comparing options, skeptical), another reason to buy, another format (a person talking, a demo, a still, a product on its own), another person or setting on screen. A fair test: if one sentence describes every ad in the ad set, they are one ad.
- **Fewer ad sets, more creatives in each.** Splitting a budget over many small ad sets starves each of the results it needs. One practitioner test reported one ad set with 25 distinct creatives beating five ad sets of five (17% more conversions at 16% lower cost, Common Thread Collective). Meta allows 50 ads in an ad set and itself suggests about six; working accounts sit between, by how many truly different ads they can make. A small account with three good, different ads is better served by those three than by twelve near-copies.
- **Advantage+ creative enhancements.** Some are on by default and change an ad after it is built (a crop, added text, a reworked image). An ad meant to run as composed has them switched off in the ad account; since March 2026 the choice is kept for later campaigns.

How many new creatives a month is a function of spend. One agency's working numbers: 8 to 12 under $10,000 a month, 15 to 25 up to $50,000, 25 to 40 above that. Creatives tire faster than they used to, often in two to four weeks. These are practitioners' figures, not Meta's.

## The round

The numbers in this section are Meta's; another platform's are in its own lines further down.

- **The goal** is the action the business is paid by: a purchase, a lead, an install. Optimizing for clicks or views buys clicks or views. Meta's delivery wants about 50 of the chosen event per ad set in a week to settle; an account that cannot reach that on purchases can optimize for the nearest earlier event that it can (add to cart, a lead form opened), named as such in the report.
- **Audience.** The user's own audiences stay where they have them (`manage_ad_audiences` list; search and reach size a new one; create makes one from their customers or a lookalike of it). With none, broad with Meta's Advantage+ audience is where most accounts now start: place, a minimum age and exclusions are hard limits, and interests, lookalikes and age ranges are treated as suggestions. advantage_audience is set explicitly either way. A narrow audience is right when the offer is truly limited (a city, a profession, existing customers) and the ads cannot make that clear themselves.
- **Budget level.** Two structures are in use, and accounts argue for both. Testing in its own ad set with the budget set on the ad set (budget_level adset), so new creatives get spend without the proven ones taking it all, then moving winners into the main campaign where the budget sits on the campaign and Meta spreads it. Or one campaign for everything, new creatives added beside the old, trusting delivery to try them. The first costs more management and gives cleaner reads; the second suits small budgets, where a separate test ad set never gets enough results to mean anything. Practitioners who split put 15 to 30 percent of spend on testing.
- **Consolidate before adding.** More than three or four campaigns with none reaching about 20 conversions a week is a structure problem; merging them does more than any new creative.
- **Attribution.** Meta's default counts a conversion within 7 days of a click or 1 day of a view or an engagement. It suits purchases. For a free signup or a lead that takes no thought, 1-day click is the honest window, and for ads shown to people who already know the brand, view-through credit mostly counts sales that would have happened anyway. The setting is attribution_spec; whatever it is, say it with every number.
- **Tracking, where the result happens on a site.** A campaign optimizing for purchases that Meta cannot see optimizes for nothing. Before spend: a pixel that fires the event, the server sending the same event with the same event id, and a match quality worth having. Many businesses have no such event: the sale happens in a shop, on a call, in a message or on a marketplace. There the goal is the nearest thing the platform can count (a message started, a lead form sent, a call, a visit to the page, a video watched), the ads are still read on attention and clicks, and the verdict on what they earned comes from the business's own count over the same days, which the user gives.

## Reading a creative

Wait before reading. A creative that has spent less than about one target cost per result has said nothing; most buyers judge at two to three times that, or about a week, whichever comes first. Reading daily and acting daily is the commonest way to kill ads that would have worked.

Read in this order, each against the account's own last 90 days, never a published benchmark:

- **Did it get delivery?** In a shared ad set Meta gives most spend to a few ads. One that got almost none was not rejected by buyers; it was never shown. It can be tried again in a test ad set before it is called a loser.
- **Hook rate** (Meta): 3-second video plays divided by impressions. It says whether the opening stopped the right people. Meta does not report it; it is built from those two counts, where the read returns them (`read_ad_results` insights names the metrics the platform gives), else from the user's ads manager. Low with everything else fine means the first second is the problem.
- **Hold rate** (Meta): ThruPlays (plays to the end, or 15 seconds) divided by 3-second plays. Some teams use 15-second plays instead; the two are not comparable, so name the one used. A good hook with a weak hold means the body loses them: the proof comes late, or the ad is longer than what it carries.
- **Click rate and cost per click** on the link: people watched and did not want the offer, or the ask was unclear.
- **Cost per result and return** on the event the campaign optimizes for, over the attribution window. This is the verdict; the others say where to look.
- **Frequency and the trend.** Cost rising week on week while frequency climbs is a creative tiring, not failing.

On Meta, breakdowns by video_asset, image_asset, body_asset and title_asset split an ad's result by each picture, video and text it carries, which is how a hook is told apart from the copy around it.

Platform numbers overstate what ads caused. Where the user has it, the business's own total (revenue against all ad spend) is the check on any single ad's reported return; an ad with a poor reported return can be the one introducing buyers that another ad closes.

## Leave, end or remake

- **Leave** an ad that is at or under the target cost and still getting spend. Editing a working ad set (its audience, its optimization event, a large budget jump) restarts its learning. On Meta, raise a budget in steps of about a fifth, a few days apart; a strongly performing ad set takes more. These are habits buyers hold, not rules Meta publishes, and Meta's own treatment of them has loosened: on Meta, adding a new ad to a healthy ad set often does not restart anything.
- **End** an ad that spent two to three times the target cost with nothing to show, or whose cost stayed well over target through a full week. End it and write down what it tested, so the same idea is not made again.
- **Remake** an ad that half works. This is the loop:
  - Good hold, weak hook: keep the body, change the first two seconds. Another opening shot from the same footage, another first line. The opening has to look different, not only read different.
  - Good hook, weak hold: keep the opening, re-cut the body. Bring the proof forward, cut the length.
  - Good attention, weak clicks: the offer or the ask. Change the close and the headline.
  - A winner that is tiring: the same argument with another person, setting or format, before its cost has doubled.
- **Scale** a winner by budget, and by making more of what it proved: its angle in other formats, its format on other angles.

The remake is made from the ad's source in the library: the timeline or canvas it was rendered from is copied as a variation, the one part changed, the new version rendered and added to the same ad set as a new ad. The old ad keeps running until the new one has its own read. The new ad's name carries what changed, so the next read says whether the change did it.

## The other platforms

What differs from Meta, from each platform's own guidance. Where a platform's number differs from one above, the platform's holds there.

- **TikTok.** TikTok's help pages put the end of learning at about 50 conversions, over about 7 days; an ad group short of 20 in its first 10 days is unlikely to leave it. Outside its automated setup (Smart+, one ad holding up to 50 assets) TikTok asks for 3 to 5 different creatives in an ad group and 3 to 5 ad groups in a campaign, and advises against duplicating ad groups to scale: fix the one that is slipping. 9:16, the point made in the first 3 seconds. A post already on the account can run as an ad (a Spark Ad). There is no 3-second count or ThruPlay: the opening is read from 2-second or 6-second video views over impressions, the rest from views at 25, 50, 75 and 100 percent and average play time. Default window 7-day click, 1-day view.
- **Google** (Demand Gen, Performance Max). The unit is the assets inside an ad: several pictures and a video in each shape (vertical, square, horizontal). Learning lasts up to 7 days and a change is judged over two weeks or two conversion cycles. Demand Gen wants about 50 conversions and a daily budget of at least ten times the target cost; bids move 5 to 10 percent a week; changing the conversion goal is the largest reset. New assets are added before old ones are removed, and given 14 days. A view is 30 seconds or the whole ad, so hook and hold do not carry over; view rate does. Default window 30-day click. `manage_ad_tracking` conversion_actions and conversion_goals set what counts.
- **LinkedIn.** No learning phase is published; a campaign runs at least seven days before it is read. The audience is the lever here (job, seniority, company; 300 members at least, `manage_ad_audiences` reach and forecast size it), with four or more creatives in a campaign. A person's own post can be sponsored as a Thought Leader Ad. Video is read from views (2 seconds), view rate, completions and dwell time. Its window can run to 90 days for clicks and views, so its conversions are not comparable with Meta's.
- **Pinterest.** 50 to 200 conversions a week, at most two rounds of changes a week, three to five days to settle after one. While an automated campaign shows Learning, no creative is added and no budget changed. Pins are 2:3; people save them, so saves and outbound clicks are read beside conversions. Default window 30-day click, 1-day view.
- **Reddit.** About 20 results to leave learning. It restarts on a bid or budget change above 20 percent, a change of targeting, placement or creative, and a pause of a week. Default window 28-day click, 1-day view.
- **X.** No learning phase is published. 3 to 10 posts in a campaign, weak ones swapped for fresh ones. Video is read from 2-second and 6-second views.
- **ChatGPT.** An ad sits under a response: a headline, a line of text, an image and a link, no video. There is no audience to set: each ad group is matched to conversations by the context it is given. Objectives are views, clicks or conversions; the read is impressions, clicks, spend and conversions, with spend arriving hours late.

## Rules that watch

A rule can end an ad that spends past a ceiling with no result, or tell the user when cost crosses a line (`manage_ad_rules` templates, then create with enabled false and preview to see what it would have done; runs and changes list what a rule did, and undo takes a change back). Rules suit protection. Decisions about creative stay with a read: a rule sees a number, not why.

You can also keep a number of the workspace's own on the ads (`manage_ads` save_measure: a name and a formula, such as purchase_value * 0.4 - spend for profit at a 40% margin) and put an alert on it (alert_when, alert_value). The user sees it on the Campaigns page beside the platform's numbers, and `read_ad_results` measures returns it with its value and whether it is alerting, so the figure the round is judged on is one both of you read.

## What gets accounts restricted

Payment failures, budgets raised many times over in a day on a new account, bursts of edits, and ads that make claims the platform bans (before and after bodies, income promises, "are you" lines about a personal trait). A new ad account is warmed up: modest budget, few changes, a week of clean delivery before scaling. An ad in review is waited for, not duplicated.

## The check

- The goal is the event the business is paid by where the platform can see it, else the nearest one it can count, named as that.
- Every creative in the ad set is a different concept, and each maps to a named library item.
- Nothing was judged before it had the spend or the days to mean something on that platform.
- What the user already runs their way was left their way.
- Each change is one change, named on the ad, so the next read can attribute it.
- Every number is given with its window and its attribution setting.

## Handover

- What runs: the campaign, the ad sets, each ad with its item, the budget and the goal; which ads wait paused for the user's switch.
- What the last read said: per creative, delivery, hook, hold, click, cost per result, and the verdict (left, ended, remade), with what each taught.
- What comes next and when it is read.
- Also possible: a rule that ends runaway spend, a lead form, a custom or lookalike audience from the user's customers (`manage_ad_audiences`), the same winners on another ad platform, a lead's details read back (`manage_ad_tracking` leads), competitors' running ads read from the public ad libraries of Meta and LinkedIn (`read_ad_results` ad_library).
