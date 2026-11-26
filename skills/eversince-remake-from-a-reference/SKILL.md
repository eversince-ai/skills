---
name: eversince-remake-from-a-reference
description: "A video like a reference, with the user's footage: the reference written down as numbers, built, compared at the same seconds."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-remake-from-a-reference"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A reference is a set of decisions someone made: how long each shot runs, where the camera goes,
how type enters, how dark the blacks are. Remaking it means writing those decisions down as numbers,
building from the numbers, and comparing the build to the reference at the same seconds until they
agree. Working from memory gets the mood and misses everything countable; written down, most of it
lands on the first build. A brief or a brand from the user outranks every default here.

## The plan: a reference edit remade with the user's footage

1. Get the reference in: `search_library` {query} or `get_items` {refs} if it is already in the workspace; `import_media` {mode: "url", items: [{url}]} for a file; `pull_posts` {source, account: the post's link} then `keep_posts` {posts: [its id]} for a social post (on YouTube and Reddit a post's link does not pull the post; a YouTube video's words come through `pull_posts` `transcript_of`)
2. `inspect_media` {ref, cuts: true}: every cut and each shot's length
3. `inspect_media` {ref, mode: "window", window: {start, end} for each section}: what happens in each
4. `detect_beats` {ref: the reference}, where it is cut to music
5. `inspect_color` {ref, at_seconds}, one call per moment, at two or three moments: the grade
6. Write the breakdown (below), one line per shot
7. `search_footage` {like: {ref: the reference, at_seconds}} or {query} per line: the user's closest moments; a line with nothing is a gap for the user to fill
8. `manage_timelines` {create: {name, aspect_ratio: the reference's, clips: [{ref, source: [start, end]} per line, in order]}}
9. `edit_timeline`: the grade, transitions, type and moves the lines record
10. Compare: `inspect_timeline` {timeline_id, seconds} beside `inspect_media` frames of the reference at the same seconds, and `inspect_color` {timeline_id, at_seconds, reference, reference_at_seconds}
11. Correct one kind of thing per pass, then compare again, until the frames agree
12. `check_timeline`, then `publish_preview` {timeline_id}: the link to hand over

## What to take

The user decides how close to go. Usually the value is in the mechanism: the cut rhythm, the reveal
order, the type style, the way a layout moves. Their own subject, words and footage on top of it is
what makes the remake theirs. If they want it closer, go closer; how close is their call. Another
brand's logo and a real person's face are left out unless they are the user's own.

## Getting the reference in

A reference is read as a library item: one already in the workspace (`search_library` by its words,
or `get_items` by ref), the user's upload, `import_media` for a hosted file, or `pull_posts` with
the post's link as account and then `keep_posts`. A YouTube post comes without its video file: its
words come from `pull_posts` transcript_of and the rest is described. `inspect_media` also reads a
picture or video at a web address without keeping it. A reference that cannot be brought in is
described in words, and the remake is as good as the description.

## Writing it down

Look first, then ask a model only what looking cannot answer.

- **The shape**: `inspect_media` overview of the whole item, then window grids over each section and single frames at the moments that matter.
- **The rhythm**: `inspect_media` with `cuts` gives every cut in seconds and each shot's length. The average and the median are the tempo. Mode shot_log adds a line on what is in each shot. On motion-graphics references and white or color transitions the cut list runs slow and misses changes, so read the shot changes off the overview frames and count them.
- **What a frame cannot answer**: `inspect_media` with a `prompt`, one narrow question at a time (how the type enters and holds, how the camera moves here, how the mix sits). Answers about time come back as seconds in `moments`.
- **The grade**: `inspect_color` on a few moments: black and white points, shadows, mids and highlights, saturation, warm or cool.
- **The music**: `detect_beats` for the tempo and where the downbeats fall.

The breakdown is one line per shot, with a number on everything that takes one: when it starts and
how long it runs; the shot size (wide, medium, close), the camera's height and angle, any camera move with its start, end, speed and where it settles;
when each element enters, for how long, and its easing; what starts while something else is still
moving; what is out of focus; colors as hexes picked from the frame; type as the style of face, its
height as a share of the frame, weight, case and spacing; how it hands over to the next shot; what
can be heard. For example: `Shot 3 (4.2–6.0s): camera locked; the stat card rises 60px into the
lower left over 0.5s on a hard ease-out, the number counting 0 to 87 across 1.2s; the label fades in
0.2s after the count lands; background #0E1116 with one teal glow behind the card; cut at 6.0s with
no transition, on the downbeat.` Every clause is something to check the build against.

What each kind of reference gives:

- **An edit**: the cut list with shot lengths, what is in each shot, where cuts fall against the music and the words, the longest hold and why it works. A build within about a tenth of the average shot length reads as the same tempo.
- **A graphic**: each element's entrance, travel, easing and duration, the stagger between items, how long things hold, what the camera does, what moves across each cut.
- **A look**: black and white points, what is crushed or clipped, the color of the shadows against the highlights, the one hue that pulls the eye, grain and softness.
- **Captions**: size against the frame, case, words per line, placement, how a line arrives, what is emphasized and how.
- **Sound**: the tempo, where the energy drops and lifts, how far the music sits under the voice, what the effects mark.

Stills in a sequence describe motion: a deck, a storyboard or a set of mockups often shows one element at
different moments. A card drawn in four places is one movement to animate, not four images to cut
between.

## When the reference is a moving layout

Many references are not cuts: a grid of tiles scrolling past, a mosaic that builds, a headline
climbing the edge of the frame. A shot log reads them as one long shot, so write down the geometry
and the travel instead:

- **The grid**, off one frame: margins, column width, gutter and corner radius as fractions of the frame, and `inspect_color` on a bare patch for the background color.
- **The travel**, off two frames a known time apart: how far one tile moved in frame heights.
- **The stack**, off the overview: how many tiles and the rhythm of their heights.

Build it as one clip per tile, each on its own track, with a `frame` slot for where it sits and a
`position` keyframe track for the travel (fields in `skill-timeline-operations`). `frame` places
the box, `position` moves the box, `focus` pans the picture inside it. `position` is measured in
the tile's own box, not the frame: for travel T in frame heights and a tile of height h, the end
value is -T / h. Get it wrong and the stack shears instead of scrolling, which looks like a rendering
bug and is not. The background is a `solid` clip on the lowest track; type that types on, rises or
counts is a graphic (`skill-motion-graphics`), fixed in the frame while the layout moves behind it.

## When the reference is a still

An ad, a poster, a slide, a post, a cover: the same method on a canvas. Read off the picture
(`inspect_media` with `grid`): the layout as percentages of the frame (each element's box, the
margins, the gutters), the type (style of face, height as a share of the frame, weight, case,
spacing, the closest catalog face), the colors with `inspect_color` `points`, the photo's grade,
the effects (shadow, blur, glow, duotone). Build at the reference's size (`manage_canvases` create
with its width and height) with `skill-graphic-design`, and compare with `inspect_canvas` against the
reference, one kind of thing per pass.

## Building from the numbers

The breakdown is the brief. Each craft has its skill: the cut `skill-video-editing` or
`skill-short-form-videos`; graphics `skill-motion-graphics`; the look `skill-color-grading`; type
and layout `skill-graphic-design`; captions `skill-animated-captions`; sound `skill-sound-design`.

For shots that look like the reference's, `search_footage` with `like` (a moment of the reference
by `ref` and `at_seconds`, or a picture) returns the closest moments in the workspace's indexed
footage, with their spans. A reference video is indexed first (`search_footage` with its `refs`). A line the footage has nothing for is a gap the user fills: the nearest
footage, a stock shot (`search_stock`, then `import_media`), a generated one (credits; the reference's people not reproduced), or the line
left out. If the user can still film, the breakdown doubles as a shot list: each line says what to
shoot, how framed and for how long. The handover names each gap and what filled it.

## Comparing at the same seconds

Draw frames of the reference (`inspect_media`) and the build (`inspect_timeline`) at five to eight
seconds that carry the video: a cut, a reveal, a hold, the end. `inspect_color` on the build with the
reference frame as `reference` returns the color gap as hints on the grade fields.

- **Pace**: the cut points within a frame or two of the reference's, and the average shot length within about a tenth. An average hides the pattern, so also match where the fast and slow stretches sit and the longest and shortest shots.
- **Sizing and framing**: the subject the same share of the frame, in the same place; type the same height as a share of the frame.
- **Rhythm**: moves and cuts landing on the same beats.
- **Style**: the same colors, grade, type style, easing and transitions.

One kind of thing per pass: timing first, then framing, type, color, texture. A pass that changes
several at once cannot tell which change worked. Give target values, not directions ("the title
starts at 1.2s and takes 0.4s to arrive", not "quicker"), and keep the values already right. A shot
that will not come right is cut back to its subject and its one move, matched there, then rebuilt a
layer at a time until the layer that spoils it shows.

## Where words stop

What can be measured survives being written down: a length of time, a count, a color, a size, a
route. Gradual qualities do not: the softness of a glow, how a surface takes light. Judge those on
frames against the reference, and stop when the difference no longer shows at delivery size. The
settled values (durations, counts, colors, proportions, the camera's route), written at the top of a
graphic's source, are where the next video in the same series starts.

## The check

At every compared second, the build and the reference read as the same decisions: the same cut on
the same frame give or take one or two, the same framing, the same hierarchy. Every number in the
breakdown shows up in the build. Name what could not be matched, and why.

A last pass over the whole video is in `skill-finish`.

## Handover

- The remake at the reference's shape and length; any stock or generated shot named.
- What could not be matched.
- In the app, with the result's app_url as the link; `render_timeline` for a file.
- Also possible: the settled values kept as a template for the next video in the series, the same remake at another shape, the look alone applied to other work (`skill-color-grading`), a shot list for the user to film.
