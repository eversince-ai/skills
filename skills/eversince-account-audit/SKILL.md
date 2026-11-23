---
name: eversince-account-audit
description: "One account's posts studied against its own median: what breaks out, what holds back, what to change, its style. A field is social trends."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-account-audit"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

An account audit reads one account's recent posts against that account's own baseline and says what works, what doesn't, and what to change. The same read serves any study of an account: what it posts, how it opens, its formats and voice. The user's own editing style is `skill-edit-in-my-style`; colors, fonts and logos are `skill-brand-style`. The account can be the user's own, a competitor's, or one the user points to. Every conclusion traces to posts the user can open, and the answer goes in the chat. For a field read across many accounts, use `skill-social-trends`.

## The plan

1. The account: use the link or handle the user gives. If they name a competitor without one, find it by name and name the handle you read in the answer, since a wrong account is read and paid for all the same.
2. Any public account, connected here or not: pull_posts {source, account, order: "latest" where the source takes it} → the last twenty to thirty posts (pull a further page with `cursor` if the first falls short). Each page costs a fraction of a credit. The user's own account, where it is connected in Eversince (`manage_social_accounts` list), is read free instead: read_social_results {view: "posts", account_id, from, to}, with the numbers the platform gives the account itself. X accounts have no results there.
3. The baseline: the median of each count, per format, with the counts of posts under two to three days old set apart.
4. Breakouts and weakest: score each post as a multiple of its format's median and set the top and bottom quarters side by side.
5. Run the same pull with media: true, then inspect_media on the first two seconds of each breakout and each weak post. Read their words (`transcript_of` on TikTok and YouTube) and captions, and the comments on the breakouts (`comments_of`).
6. Work out what the breakouts share that the weakest lack, and what else could explain the gap.
7. Answer in the chat: the sample, what works, what holds the rest back, and three to six changes as keep, stop and start, each with the posts it rests on and how the next posts test it.

## The numbers

Platforms show public counts only: views, likes, comments, followers, and on TikTok shares and saves. From a public read you never get reach, retention, watch time, swipes, sends or follower growth, so every conclusion has to hold within those counts. A connected account's own read adds what its platform reports to the account, such as reach, impressions, shares and saves (`read_social_results` catalog lists them per platform).

- **Use the median** of the last twenty to thirty posts, not the average, which one viral post inflates. Each format gets its own (a Reel against a carousel, a Short against a long video). A format with fewer than eight posts has no baseline of its own: describe it, don't score it.
- **Score each post as a multiple of its median.** About one and a half times is above typical; three times or more is a breakout; the bottom quarter is the weakest.
- **Young posts are still counting.** A TikTok or Reel that goes on to do well can have under a quarter of its month's views at two days (viral.app, 430,000 posts, 2026). Read posts under two to three days old like any other, since the newest are the most relevant, but keep their counts out of the baseline, name them as still counting, and compare posts only at similar ages. A connected account shows its own curve: metrics on posts grouped by postAge.
- **Read engagement against views, never followers.** On recommendation feeds a post's reach has little to do with the following, so likes, comments, shares and saves are measured per view. Shares and saves against views, where TikTok gives them, are the closest public sign that a post was worth passing on or keeping. Compare each to the account's own median, never a published benchmark. Where the account is connected, a ratio worth watching can be kept as a number of the workspace's own (`manage_social_accounts` save_measure, such as saves / views * 1000), with an alert past a value: the user sees it in Analytics, and `read_social_results` measures returns it later with whether it is alerting.
- **Cadence:** posts per week and the gaps, and whether recent posts trend down against older ones.
- **YouTube Shorts counts changed.** Since 31 March 2025 a Short's public views count every start and replay, so don't compare views on Shorts from before and after that date.

## Breakouts against the weakest

Compare the breakouts with the weakest posts on the traits below. Call a trait a cause only when the breakouts share it and the weakest lack it, across three or more posts. One post is an anecdote, and a trait the weakest share too explains neither.

- **Topic and angle:** what it's about, and whether it's what this audience came for.
- **Opening:** what is seen, said or written in the first two seconds; whether it starts on the point or warms up first (a greeting, a logo, context).
- **Format:** the shape (a list, a demo, a before and after, a story, a reaction), its length against what it carries, its pace.
- **Cover or thumbnail,** where the platform shows one.
- **Sound:** original, voice, or a trending track.
- **Caption** and its first line; whether the post is part of a series.
- **What viewers say:** the questions asked and lines repeated back in the breakouts' comments. These are often the next posts.
- **When it went out** and the gap before it. Rarely explains a gap of three times.

## What else could explain it

Before calling a trait the cause, check the other explanations and name any that apply: a post boosted with paid ads, a collaboration with another account, a trending sound, a news moment, a giveaway or "comment a word" post (its comments count the bait). On Instagram, an account that reposts others' content is held out of recommendations, so weak reach across the board can be that rule and not the posts.

## What it can and cannot say

A public audit shows which topics, openings and formats beat the account's own median and by how much, how consistent the account is, whether shares and saves follow views, what viewers ask for, and how often it posts. It can't show where viewers dropped off, who saw a post, or what it did for followers or the business. So a weak post with a slow opening suggests the opening, and you say "suggests". The user's own account connected in Eversince answers those: `read_social_results` metrics reads a video's retention, the audience by age, gender and country and when followers are online (insights), and followers by day (accounts), and a post made here comes back with the library file it was made from. Where it is not connected, the same answers come from the account's analytics through a connection of your own or from what the user exports, matched to posts by link or date; connecting it here (`manage_social_accounts` connect) is another way.

## The changes

Three to six, as keep, stop and start, one line each. Order them by the size of the gap they close and the strength of the evidence, weighed against the effort they take. Each line gives:

- what to keep, stop or start, with the posts it rests on by link;
- how the next posts test it: change one thing across the next five to ten posts and compare at the same age against the old median.

Put what to keep first: dropping a breakout's trait by accident costs more than keeping a weak one.

## A competitor, or several

Compare the same number of recent posts over the same window, scoring each account against its own median. This shows the topics and formats a competitor wins with that the user doesn't make, and the reverse.

**Their ads**, when asked: meta_ads, tiktok_ads and linkedin_ads by advertiser, google_ads by domain. On meta_ads the signals are days running (an ad still live months on is likely paying for itself) and `variants` (several creatives under one ad means it's being scaled). Fifteen to twenty ads grouped by hook and offer show the pattern. For a weekly watch, keep the ad ids from the first read and report each time what's new, what stopped and what changed. Spend and results aren't in the libraries.

## The answer

In the chat:

- **The sample:** which account, how many posts over what dates, the medians per format, and the posts left out as still counting.
- **What works:** the breakouts, by link and multiple, and what they share.
- **What holds the rest back:** by link, with the other explanations checked.
- **The changes,** as above.

If the user wants them, you can also keep the posts (`keep_posts`, within two days of the pull, since a pull's video addresses expire), save the audit as a document (`save_document`, each kept post linked where a sentence names it as [Title](item:REF)), write the account's style (subjects, formats, openings, voice) into the brand kit for later work (`manage_brand_kit`), repeat the audit in a month against the same medians, or build the changes into the next posts.
