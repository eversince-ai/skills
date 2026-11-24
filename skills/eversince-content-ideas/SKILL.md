---
name: eversince-content-ideas
description: "Ideas for the next posts, each traced to evidence (an outlier, a search, a question), with hook and format. A field's trends: social trends."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-content-ideas"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Content ideas finds what the user should make next: five to ten ideas, ranked, each traced to evidence. An idea is a topic, an angle and the post itself, specific enough to make as written. Ideas generated from nothing come out the same for every user, so start every idea from a real source: something the user has, a post that outperformed, a question people actually ask. The ideas go to the user in the chat. The skill is for when the user asks what to make; during other work it is an offer to them, not a step.

## The plan

1. Learn who the user is and what they make: the kind of account (a software product, a business, a creator), its audience, its platforms, and what is new with them. You likely know most of it already; ask for what is missing in one question.
2. Mine the sources that fit that kind of user (below) for twenty to thirty raw ideas.
3. Build each raw find into an idea: the topic, the user's own angle, the post.
4. Run the filters and drop anything that fails one.
5. Send five to ten in the chat, ranked, each in the shape below.

## Where ideas come from

**A software or AI product**
- What shipped: every release, feature or fix is a candidate. Show it working; don't describe it.
- What customers ask and object to: support questions, sales objections, the exact words buyers use.
- The decisions behind the product: why something was built, cut, priced or changed, with the numbers and the mistakes.
- The category's news: a model release, a competitor's launch, a platform change. The window is hours to a day; after that it is coverage, not news.

**A business**
- What happened this week: an order, a delivery, a new product, a customer, a problem solved. What the business already does is the content.
- What customers ask, in messages, reviews and in person: each question is a post, and so is each myth it can correct.
- The owner and the making: why it started, how the thing is made, the day behind the counter.
- The season: holidays, local events, weather, restocks, the calendar of its customers.

**A creator or personal brand**
- Their own breakouts: remake a past post that outperformed with a new subject, or turn it into a series. An account connected in Eversince lists them free (`read_social_results` posts, sort engagement), a post made here with the library item it came from.
- Their comments: the questions people ask and the lines they repeat back are the next posts (`manage_social_inbox` commented_posts and comments on a connected account).
- What happened to them: the work, the trip, the experiment, the result.

**Every kind of user**
- **Outliers.** A post at three to four times its account's usual views or more proved its idea and format, not its account's reach. Look in the user's niche and in neighboring ones, where a proven format carries over ("$1 against $100", "one day against one year"). `pull_posts` reads an account's recent posts with their counts, so you can compare a post with the account's own median; by query it shows what a niche is watching. Each page costs a fraction of a credit. With `media: true` the same pull gives each post's video address: `inspect_media` on its first two seconds shows how it opens, and `transcript_of` on a TikTok or YouTube link gives what it says (a post from another platform is kept with `keep_posts` and read with `transcribe_media`).
- **What people search.** Social platforms are search engines now. What people type is an idea with its title already written. Eversince does not read search data. If you can browse the web, some of it is public: Google and YouTube search suggestions (a phrase that autocompletes is one people type), Google Trends, TikTok's Creative Center. YouTube Studio's content gaps and TikTok's creator search insights (searches with few good videos) sit inside the user's own accounts, so you get them only if the user opens and shares them. Without either, the questions in comments and on Reddit stand in for search.
- **What is taking off this week (optional).** A format, sound or topic rising in the niche can start a timely idea: run `pull_posts` by query with a short window and the source's own top ordering, and look for what repeats. For a full read of a field, use `skill-social-trends`.
- **The audience's own words.** Reddit threads, forum questions and the comments under the niche's top posts (`comments_of`): the recurring questions, complaints and phrases, used as people said them.

Use only the sources that fit. A creator with a thousand comments needs no Reddit; a software company that shipped this week starts with what shipped.

## Building an idea

An idea has three parts, and a raw find is usually only the first:

- **The topic:** what it is about ("editing a podcast with AI"). Many accounts share a topic.
- **The angle:** the take only this user can give, which is also what the viewer takes away ("a 40-minute episode cut into twelve clips before the coffee was ready"). It comes from what the user has: their access, numbers, product, footage, opinion. One topic carries many angles (a how-to, a result, a mistake, a comparison, a contrarian view, then against now).
- **The post:** what it actually is: the format (a demo, a before and after, a list, a story, a reaction, a carousel, a single image), what happens in it, the platform and the length.

A proven post is a precedent to learn from. Copied as it is, it gives the same take with a different face and a weaker result, however much it is trending. Keep its topic and format, and give it the user's own angle.

## The filters

Offer an idea only if it passes all of these:

- **It fits in one line and one opening.** If you can't say the angle in a sentence and show it in the first second or on the cover, it is still a topic.
- **People want the result.** The viewer gets something: an answer, an outcome, a feeling, a reason to share. A mechanism or a feature name alone is not a result.
- **Something proves demand.** An outlier, a search, a repeated question.
- **The user can say it.** They have the access, experience or material to be believed.
- **Someone would send it.** To a specific person ("you need to see this"): sends weigh heavily in how platforms show posts to people who don't follow the account.
- **It fits the audience.** Most ideas serve the people already following; about one in five can be an experiment that reaches past them.

## The shape of an idea

Each idea, in the chat:

- **The topic.**
- **The angle:** one sentence, the thing the viewer takes away.
- **The post:** the format, what happens in it, the platform and the length.
- **The opening:** what is seen first and what is said or written there: a video's first second, a carousel's cover slide, a single image and its headline.
- **The evidence:** the source by link: the outlier and its multiple, the search, the question.
- **Timely or lasting:** for a timely one, how long its window is.
- **What it needs:** footage the user has, something to film, stock or generation. If the user's footage is in the workspace, `search_footage` finds the moment an idea would use; include it as a link.

Rank by strength of evidence against effort, strongest first.

## Handover

- The ideas in the chat, ranked, each traced to its source.
- You can also keep the posts behind the strongest ideas for reference (`keep_posts`), save the ideas as a document (`save_document`: a table of the idea, why, and a recommendation from 1 to 10) or as cards on the calendar (`manage_cards`) when the user wants them kept, and write the chosen one as a hook and script (`skill-hooks-and-scripts`).
