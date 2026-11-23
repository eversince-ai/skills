---
name: eversince-hooks-and-scripts
description: "The words of a video before it is made: one idea, the hook as picture, line and text, beats and length, the script in two columns."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-hooks-and-scripts"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Use this to write what a video says before it is made: the hook and the script, beat by beat, with what is on screen at each line. You deliver the script in the chat, ready to read aloud or cut to. To make the video from it, use `skill-short-form-videos` or the format skill for the piece.

## The plan

1. The one thing the video says, in one sentence, plus who it is for, where it runs and how long it is
2. Five to ten hooks, each written as picture, spoken line and on-screen text together; pick one
3. The body, beat by beat, each beat joined to the last by "but" or "therefore"
4. Check the length against the word budget
5. Read the words for the ear and cut generic phrasing
6. Send the script in the chat in two columns, the line beside what is on screen

## The one thing

A video says one thing. Write it as one sentence first: what the viewer knows, believes or can do afterwards. A second thing is a second video. If the strongest hook does not fit the idea, change the idea, not the hook (Jenny Hoyos writes her Shorts hook first, ending second, middle last).

If the script builds on material that already exists, read the material first, because the moments that exist decide the beats. Use `transcribe_media` for what was said in the user's recording and `search_footage` for moments across their library. Read a post the user wants to learn from the same way: `pull_posts` `transcript_of` on a TikTok or YouTube link for its words (another platform's post: `pull_posts` with its link as account, `keep_posts`, then `transcribe_media`), `inspect_media` on its first two seconds for its opening.

## The hook

A hook is three layers landing together in the first one to three seconds: what is seen, what is said, and the text on screen. All three say the same thing. A picture saying one thing under a line saying another loses the viewer to confusion. TikTok's own guidance is to state the proposition in the first three seconds and finish the hook by six; research it commissioned on ads found most of what a viewer remembers comes from the first six seconds, and about half from the first two.

- **The picture:** choose it; don't settle for the footage's first frame. A person, an action already underway, the result itself, something unlike what the field usually shows. A person on screen at the open holds attention better than none.
- **The line:** start on the point (an action, a number, a claim), never a greeting, a name or "this is". One subject, one question.
- **The text:** the line or its core in a few words, inside the platform's safe area. It has to carry the hook alone when the sound is off. TikTok is mostly watched with sound; Reels, LinkedIn and X are watched muted far more, so there the picture and text are the hook.

**Hook types that work.** Pick the one that shows this video's payoff fastest:

- The result first: "This took 4 minutes."
- A claim against what people believe.
- A gap the viewer now needs closed.
- "I tried X": the experiment and its stakes.
- A count: "3 things…", which promises an end.
- A warning: the mistake and its cost.
- "This is for you if…": the viewer named.
- A pattern-breaking picture, with no line needed.

A question is the weakest hook when the viewer can answer it in their head.

**Write five to ten, pick one.** Ad teams test five to ten hooks on one body, and the same script performs very differently depending on the hook. The one you pick can be pictured in its first frame, names one subject, and is paid off in full by the body. Give the rest as alternatives. If the user wants to compare them, Instagram's Trial Reels show a reel only to people who don't follow the account (public accounts over 1,000 followers).

**Why hooks fail:** a greeting or "hey guys"; a logo or intro; context before the point; telling where an action would show; a promise the body doesn't pay. That last one is the worst, because a viewer who feels tricked doesn't come back.

## The body

- **Join beats with "but" or "therefore,"** never "and then" (Trey Parker and Matt Stone's rule for South Park, and Hoyos's main tool for shorts). Each beat turns on or follows from the one before, so the middle doesn't sag into a list.
- **Foreshadow the ending in the hook.** Where the subject allows, show visible progress (money counting down, steps ticking off, a before becoming an after).
- **Rehook halfway** in anything over twenty seconds: a turn, a reveal, a new question.
- **Pay off what the hook promised,** in full, and keep the ending as short as possible. A tip, claim or result can lead with its payoff and spend the body on how, since few viewers stay to see a payoff saved for last.
- **Close every loop you open.**

## The length

Spoken short form runs about 140 to 165 words a minute, faster for high energy:

- 15 seconds: about 35 to 45 words, one beat, hook and payoff.
- 30 seconds: about 75 to 85 words, three beats.
- 60 seconds: about 150 to 160 words, three to five beats and one rehook.
- One to three minutes (the limit on Shorts and Reels): chained sections, each payoff opening the next hook.

The idea sets the length, not the platform's cap: an idea done in eleven seconds is eleven seconds. Hoyos found Shorts under thirty seconds needed close to full retention to travel, and writes to about thirty-four.

Other shapes:

- **A YouTube video:** the first thirty seconds confirm what the title and thumbnail promised, show what the video is, and open a new question.
- **A carousel:** the first slide is the hook, and the second needs one too, since Instagram can show a carousel again starting on slide two.
- **A LinkedIn post:** finish the hook within about the first 140 characters, before "see more" on a phone.

## Writing for the ear

- Words people say aloud, simple enough for a ten-year-old; one idea per sentence; short sentences.
- A number or an object instead of an adjective ("4 minutes", not "fast").
- No filler ("basically", "it's important to", "here's the thing").
- Nothing that reads as generated: "it's not X, it's Y", tidy lists of three, "game-changer", "a testament to", "let's dive in", "in conclusion", dashes for drama. If the script sounds like nobody, cut until it sounds like the user.
- Read it aloud at the end and rewrite any line that trips the tongue.

## On screen

Write the script in two columns: the spoken line, and beside it what is seen (the shot, the screen, the graphic) plus the on-screen text. On-screen text is a short label per beat, not the full transcript, and never sits over a face. With no voice, the text column is the script.

## The call to action

Only if the user wants one, and never more than one:

- **"Comment [word] and I'll send you the link":** Instagram allows it when the link really arrives by message; asking for a comment just for reach ("comment YES") gets demoted as bait.
- **"Send this to someone who…":** what Instagram weighs most for reaching people who don't follow the account.
- **"Follow for part 2":** only when part 2 exists.

## What you send

In the chat:

- The one thing, in a sentence.
- The chosen hook: the picture, the line, the on-screen text.
- The script in two columns, with the time or word count per beat and the total.
- The other hooks, as alternatives.

If the user wants, you can also save the script as a document (`save_document`), record the voice from it (`skill-voiceovers-and-music`), or make the video from it (`skill-short-form-videos`).
