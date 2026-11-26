---
name: eversince-inbox-and-auto-replies
description: "Comments and messages answered in the account's voice, and the comment-a-word reply that sends a link to everyone who asks."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-inbox-and-auto-replies"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

This skill covers two jobs on an account's comments and messages: answering what people wrote, and the "comment a word, get the link" reply that sends something to everyone who asks for it. Both speak in the account's name to people outside, so the account's own voice and the user's own rules come before anything here. X has no comments, messages or automatic replies here.

## The plans

**Answering what came in:**
1. `manage_social_accounts` list → the accounts, and under agents whether replies, messages and removals are allowed for an agent in this workspace. They are off unless the owner allowed them in the app: then step 5 is drafts handed to the user, each with where it goes
2. `manage_social_inbox` commented_posts → the posts with new comments (unread first); conversations → the message threads
3. `manage_social_inbox` comments {account_id, post_id} per post, messages {account_id, conversation_id} per thread: read all of it before answering any of it
4. Sort (below): answer in public, answer in private, leave, hide, hand to the user
5. reply {account_id, post_id, comment_id, text} for a public answer; private_reply for one that belongs in a message (Instagram, Facebook); send in a thread that is open. A reply or a message can carry one library item (attachment); the same words to one place within ten minutes go once
6. Report: what was answered, what was hidden and why, what waits for the user

**Comment a word, get the link:**
1. Settle with what the post promises: the word, what is sent, and the post it runs on
2. Only where the user wants what is sent to be a page made here: `manage_public_links` create {items, title} → link_url, live at once
3. Where the post goes out from here: `manage_social_posts` create {caption, media, targets, scheduled_for} → its id
4. `manage_auto_replies` list → what already runs on the account, so two do not answer the same comment, and how many automatic replies the month has left. A post that draws many comments can use them up, and replies pause until the next month; more are bought with credits (`manage_social_accounts` buy_block) by the owner
5. `manage_auto_replies` create {account_id, name, post_id, keywords, dm_text, reply_text, options: {buttons: [{title, url}]}, active: false} → made paused, bound to the post before it is out (platform_post_id for a post already on the platform)
6. Read it back as a commenter would meet it: the comment, the public reply, the message, the page. Then update {automation_id, active: true}, before the post publishes
7. Once it runs: get {automation_id} for its log, `manage_public_links` get and activity for the page's opens, people and downloads; the replies people send back are answered as in the first plan

## Answering

**Speed.** About three in four people expect an answer within a day (Sprout Social Index, 2025). An account answered once a day at a set time is better served than one answered in bursts with silent days between.

**What gets a public answer.** A question others will have too, a compliment, a correction, a complaint that is fair. The answer is for everyone who reads the thread after, so it carries the actual information, not "check your DMs". A complaint is acknowledged in public in one line, then taken to a message for anything that involves an order, an account or a personal detail: the acknowledgment shows it was handled, the details stay private.

**What goes private.** Anything with personal information, an order, a price negotiated for one person, or a long back-and-forth. On Instagram and Facebook a commenter is messaged once per comment, within seven days of it; after that the thread opens only if they write back, and each message from them opens 24 hours in which the account can answer. A thread past its 24 hours waits for the person. So the one private reply has to carry what they need, and a question in it is how the thread stays open.

**What is left alone.** Tags of friends, emoji, a conversation between two commenters. A like is enough where the platform has one.

**What is hidden or deleted.** Spam, slurs, scams, someone's private information. Hiding is quieter than deleting where the platform has it: the commenter still sees their comment, others do not, and nothing escalates. Criticism is not hidden: an account that removes fair complaints is noticed and loses more than the comment cost. Which actions each platform has is in `manage_social_inbox`'s own text.

**What goes to the user.** Anything legal, a refund or a promise only they can make, press, a threat, a customer in real distress, and anything where the right answer is not known. These are listed in the report with the comment and a drafted answer, not answered.

**The voice.** Read the account's own past replies first (the comments read in step 3 include them) and answer in that register: its length, its warmth, whether it uses names and emoji. Answer the person, not the category: a reply that could sit under any comment reads as a machine, and platforms slow accounts whose replies are the same text many times over. Vary the wording, and answer over the session, not in one burst.

## Comment a word, get the link

A post says "comment GUIDE and I'll send it", and everyone who does gets a message with the thing. It works because asking costs the viewer one word, and the comments carry the post further.

**The promise.** The post says exactly what the person gets and that it arrives as a message. What arrives is that thing, at once, with no second ask in front of it. The message exists on Instagram and Facebook. On TikTok, Threads, LinkedIn and YouTube the answer is a public reply under the comment, so a post there promises a reply, and the link sits in reply_text where everyone reads it.

**The word.** One distinctive word that nobody types by accident: GUIDE, MENU, the product's name. Common words (link, yes, info, price) fire on ordinary comments. Matching looks inside longer words by default, so a short word also matches where it sits inside another; options.match word answers the whole word only, with typo_tolerance for near misses, and exclude_keywords for comments that should be left to a person ("refund", "scam").

**One post, not every post.** A reply tied to the post that made the promise (post_id for a post made here, scheduled or published; platform_post_id for any other) converts far better than one listening on the whole account, and cannot surprise someone commenting elsewhere.

**The message.** Two sentences or fewer, and one button (options.buttons). It says what this is and gives it. Longer openers and several buttons lose most people. It can say it is automatic and that a person reads replies: Meta recommends saying so, and it sets up the answer when someone writes back. On Instagram and Facebook this one message is the private reply the platform allows for that comment, so it has to be complete.

**What the button opens.** Whatever the user wants to send, at any address: their site, a store page, a file host. A public link (`manage_public_links`) is one option: a page anyone opens without an account, built from library items, so the guide, the items or the video the post promised sit on one page, with a text block for a prompt to copy. On a paid plan the page can carry a passcode, or ask for a name and email before it opens (permissions require_identity), and it reports back: get returns how often it was opened and by how many people; activity lists each person who left a name and email and each download, naming who downloaded which item where the gate is on. Read on a schedule, these make the link a step in an automation: a follow-up to the people who came, a list for the user. A name and email is a second ask, so it fits where the post said so.

**The public reply.** A short line the account posts under the comment ("Sent, check your messages") tells the commenter where to look and everyone else that it works. The same line under two hundred comments reads as a bot to people and to the platform: the account's reply can have several wordings (options.reply_variations, and dm_variations for the message). It is posted after the message went out (options.public_reply, the default), so nobody is told to look for a message that did not arrive.

**Delay.** None. People act within about a minute of commenting; a staged pause loses them.

**A follow gate** (options.audience follower_status, options.follow_gate for its words) holds the link until the person follows. It grows the account and costs goodwill: some people leave at the gate, and a post that promised the thing for a comment and then asks for a follow has changed the deal. It fits when the post said "follow and comment" up front. tap_to_unlock, a button the person taps first, is the lighter version.

**The post itself.** Instagram has said it does not recommend posts whose point is to collect comments ("comment with a specific word, number or emoji"), and later clarified that answering people to deliver something they asked for is not that. The line is whether the comment gets the person something real. A post worth watching that offers a guide is fine; a post that exists to farm the word is the one that loses reach. Keep the ask to one line at the end.

**After it runs.** Replies to the automatic message are from people who raised their hand; they are answered by the first plan within the day. The log shows what was sent and what was not. Each account has a monthly number of automatic replies and of messages (`manage_auto_replies` list returns the allowance); a post expected to draw more than that is said before it goes out, not found out when replies stop.

**Other triggers** (Instagram): a reply to a story, a mention in a story, a comment on a live broadcast (options.trigger). A live comment can only be answered while the broadcast runs.

## What gets an account restricted

- The same words sent many times in a short span, in public or private.
- Messages to people who did not ask: only someone who commented or wrote first is messaged, and only in the windows above.
- Automation that does not go through the platform's own interfaces. Everything here does.
- Hiding or deleting at volume, or replying to every comment within seconds around the clock.
- Promising something for a comment and sending something else.

## The check

- Every answer reads as written to that person, in the account's voice.
- Nothing personal was answered in public, and nothing fair was hidden.
- The automatic reply was read end to end as a commenter meets it before it was switched on.
- Its word cannot fire by accident, and it listens on the post that made the promise.
- What only the user can answer is with the user.

## Handover

- Answered: how many comments and messages, on which accounts; hidden or deleted, each with its reason.
- Waiting for the user: each item with a drafted answer.
- Automatic replies: which run, on which post, with which word, what the log shows, and how much of the month's allowance is used.
- Also possible: the questions that repeat, as ideas for the next posts; the same automatic reply on the next post; a reply to story mentions.
