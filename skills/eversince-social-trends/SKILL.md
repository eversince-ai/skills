---
name: eversince-social-trends
description: "What is taking off in a field now, from its top posts: each trend's stage, fit and the user's angles on it. One account is account audit."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-social-trends"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill finds what is taking off in the user's field right now and what the user can do with it. You pull the field's top recent posts from the platforms with `pull_posts`; whatever repeats across them is a trend. You answer in the chat with every trend you found, one or many, each with its stage, the posts that show it and the angles the user can take on it. To read a single account on its own, use `skill-account-audit`.

## The plan

1. Know the field and the user: what they make, for whom, on which platforms; ask for what is missing in one question
2. Pull: two to four queries per platform the user posts on, recent and ordered by traction (below). About five to fifteen pages in all
3. Read what repeats across the top posts, group it into trends and give each its kind
4. Judge each trend: its stage, its fit for the user, whether there is time to make it
5. Turn the ones that fit into angles for the user, with how to use them
6. Answer in the chat: each trend, its stage, the posts that show it by link, and the angles

## Pulling the field

- **Queries.** Use atomic queries, one to three words naming a thing ("standing desk"), or specific ones, five to fifteen words naming one event or launch. The vague middle returns the average of everything. Anchor a word with another meaning: "AI agent" alone also matches police and estate agents, so put "AI agents" beside the tools' own names. Drop off-topic posts before you count anything. If a query finds nothing, replace it; don't widen it until it matches anything.
- **Per platform.** tiktok by query: order most-liked, posted this-week, `within_days` for the window; `content_type` all adds photo carousels. instagram: a query finds Reels, a #hashtag finds posts and Reels; posted last-week with `within_days`. youtube: `content_type` shorts for Shorts, which takes no order, so use `min_views` instead; posted this_week.
- **What "top" means per platform.** tiktok orders by likes itself. instagram search and youtube Shorts have no popularity order (Shorts sorted by popularity come back nearly empty), so their pages arrive in the platform's relevance order and you do the ranking yourself, as below.
- **The window.** Three to seven days for "this week". A filter one source honors is silently dropped by another (`pull_posts` lists which honors which), so don't read a result as windowed when the source ignored the window.
- **Traction.** Read the first page of each platform whole to learn the field's spread. Later queries take `min_views` or `min_likes` at about its top quarter, never a number from another field. `views_per_day` ranks what is rising above what is old and large; shares and saves, where TikTok gives them, weigh most.
- **An empty or thin page means the filters, not the field.** Pull it again with the order alone before you say anything about the field. Never answer "nothing is trending" because your filters emptied a page.
- **Other platforms, where the field lives there.** linkedin and x by query for a business or professional field. Both are ranked by a search engine and reach months back, so they show the background of a subject more than this week's movement, and `within_days` on them often leaves nothing. threads by query. reddit by query or a subreddit (r/name, rising for what is gaining) for what buyers and fans say in their own words.
- **Geography.** No organic source filters by country. Use `language` and `creator_country` on TikTok posts, and the language of the text elsewhere, as the proxy, and say that it is one.

## Reading the posts

Read the whole page before opening any post. What repeats across unrelated accounts is a trend; the same thing from one account is just that account.

- **Views against the account.** Where followers are given (tiktok, instagram), a post at many times its creator's following was carried by its idea or format, and that is what travels. A large account's large number says more about the account.
- **The few that matter** (five to ten). The same pull with `media: true` gives their video addresses. Run `inspect_media` on the first two seconds of each to see how it opens, and `transcript_of` on a TikTok or YouTube link for what it says; a post from another platform is kept (`keep_posts`) and read with `transcribe_media` on its ref. An Instagram reel's top comments come on its page; `comments_of` reads another post's comments, a page each, when what viewers say is the question. If a post asks viewers to comment a word for a prize, its comments count the bait, so look at its saves instead. A post marked `paid` is an ad; read it separately.
- **Each trend's kind**, since each is used differently:
  - **A sound:** the track carries it and the meaning is swapped in. The fastest to fade.
  - **A format:** a structure, transition or edit pattern reused with new subjects. Lasts longer, and works without the sound.
  - **A topic or news:** use it with a point of view or information, never by imitation.
  - **A meme or challenge:** use it like a format, with more risk in where the joke came from and who it mocks.
  - **A look:** an aesthetic carried over the user's own subject.

## Judging each trend

- **Its stage.** About half of TikTok trends fade within five days and about a quarter last past two weeks (Publicis Groupe's study of TikTok trends).
  - *Rising:* the top posts are days old, many unrelated accounts use it, small accounts earn views far above their following.
  - *Peaking:* the top posts are over a week or two old, large accounts and brands are using it, comments say "this again".
  - *Over:* name it as over, so nothing gets made from it too late.
  - A format seen across several weeks, many accounts and different sounds is lasting, not a trend, and can be made any week.
- **Its fit for the user.** Does it match their tone, apply to their own situation, and leave room for them to add something instead of copying? A trend that fits none of these reads as forced; report it as not theirs.
- **Its timing.** If making it takes longer than the trend has left, skip it, or keep the format and drop the sound.
- **The sound's rights.** Many trending songs are not cleared for business accounts on TikTok and Instagram, which offer commercial libraries instead; tell a business which songs it can use. Eversince's stock music does not include platforms' trending audio, so a trending sound is added in the platform's own app when posting.

## Angles

A trend copied as it is gives the user the same post with a different face. Give each trend that fits one to three angles of the user's own:

- **Same structure, the user's subject:** keep the format, put their product, customers or day in it.
- **The payoff twisted:** turn the expected ending. This is where brands that ride trends well get their reach.
- **Joined to what they already run:** a series, a recurring character, a product they show often.
- **Their own past hit:** a post of theirs that did well, remade in the trend's format.

For each angle, say what the post is, its opening (what is seen first and what is said or written there), the platform, and how fast it needs to go out.

## The answer

In the chat, strongest first:

- **The trend:** what it is and its kind, in a line.
- **Its stage,** and how long it likely has.
- **The evidence:** two or three posts by link, with views against their account.
- **The angles** for the user, as above, and for a sound, whether a business account can use it.
- **What does not fit,** a line each, so the user knows you saw it.

State the scale of the read: how many posts, from which platforms, over what window.

If the user wants them, you can also keep the posts (`keep_posts`, within two days of the pull, since a pull's video addresses expire, a TikTok video's within about six hours), save the read as a document (`save_document`, each kept post linked where a sentence names it as [Title](item:REF)), repeat the same read on a schedule you run, or write an angle into a script (`skill-hooks-and-scripts`).

## An example: a field this week on TikTok, Instagram and Shorts

1. pull_posts {source: "tiktok", query, order: "most-liked", posted: "this-week", within_days: 7} → read the first page whole for the field's spread
2. pull_posts {source: "tiktok", the next query, the same filters, min_views: about the first page's top quarter}, for each further query
3. pull_posts {source: "instagram", query or "#hashtag", posted: "last-week", within_days: 7} per query
4. pull_posts {source: "youtube", query, content_type: "shorts", posted: "this_week", within_days: 7, min_views} per query
5. Set aside off-topic and paid posts; rank the rest by views_per_day, shares and saves, and views against followers; group what repeats into trends
6. The same pulls with media: true for the five to ten that matter → inspect_media {urls, window 0 to 2 on each video} for the openings
7. pull_posts {transcript_of: permalink} on the TikTok and YouTube ones, one page each; an Instagram one: keep_posts {posts: [its id]}, then transcribe_media {ref}
8. Judge each trend and give it angles; answer in the chat

## What brands pay to run in the field

Use this when a business wants to know what its category advertises: the ad libraries by query (meta_ads, tiktok_ads, linkedin_ads). What a brand runs is what it spends on, not proof it works. On meta_ads, the strongest signals are days running (an ad still active months on is paying for itself) and `variants` (several creatives under one ad means it is being scaled). Read the arguments made (pain, price, proof, identity): what is crowded is table stakes, and what is absent is either an opening or a grave, which you cannot tell from outside.
