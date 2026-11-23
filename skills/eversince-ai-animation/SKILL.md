---
name: eversince-ai-animation
description: "AI-generated animation: drawn, painted or rendered worlds, style families, motion, one style held across shots. Type is motion graphics."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-ai-animation"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Animation is video whose frames are drawn, painted or rendered rather than photographed: flat vector, stylized 3D, watercolor, clay, anime, paper, line, isometric, pixel, mixed media. Choose the style first and hold it across every shot. Shot sizes, movement and the cut are `skill-ai-filmmaking`; animation adds the surface and its own physics. Keeping a character identical across generations is `skill-ai-influencer`; type, charts and titles built in code are `skill-motion-graphics`. A brief, reference or brand from the user outranks every default here.

## The plan

1. list_models {} → the image and video models with prices, then {model: each one chosen} → its limits and guidance; estimate_cost for the shot list's price
2. generate_image {type: "text-to-image", prompt: the style block, the world, the palette} → job; get_jobs → the style key
3. generate_image {type: "image-to-image", source_refs: [from step 2], prompt: the character turnaround as one sheet} → job; get_jobs → the sheet (per character)
4. generate_image {type: "ingredients-to-image", reference_refs: [from step 2, from step 3], prompt: the scene's first frame in the style block} → job per scene; get_jobs → the scene frames
5. generate_video {type: "image-to-video", source_refs: [the scene frame from step 4], reference_refs: [from step 2, from step 3] on a model whose list_models entry takes references beside a start frame, otherwise the frame alone, prompt: the style block, one action, the world's life, settings: {duration: one of the model's}, the shot count set by those lengths against the runtime} → job, one per shot; get_jobs {ids} → the shots
6. manage_timelines {create: {name, aspect_ratio: the destination's, clips: [{ref: each shot} in order]}} → timeline_id
7. edit_timeline {timeline_id: from step 6, operations: trims and transitions where a cut needs them}
8. sound per skill-sound-design
9. inspect_timeline {timeline_id, seconds: [one moment from each scene]} → the frames, read against each other
10. check_timeline {timeline_id}
11. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over

## What the job starts from

Animation wins on invisible and abstract products (a database query cannot be photographed), on transformation and compressed time, on the impossible, and on a world no stock footage can imitate. It loses where the real thing's texture is the sell (an appetite shot, a watch's weight on a wrist), unless charm is the strategy, which is claymation's ground. Start from what exists. A brand's illustrated characters or established style is the style key, and the job is to extend it. Finished animation the user brings, rendered shots or hand-drawn frames, sets the look the new work matches. A script or story is the spine.

## The style families

Commit to one family. Each name is its prompt language: studio and campaign names select training data the way camera bodies do in live action.

| Family | Wins | Anchor language |
|---|---|---|
| Flat 2D vector | Clarity: ideas, systems, abstractions; the most reliable style | "flat design illustration, vector art, bold solid colors, minimal shading" |
| Stylized 3D | Character warmth, rich worlds | "Pixar-style 3D, expressive faces, rich materials" or "Spider-Verse style, non-photorealistic rendering" |
| Watercolor, painterly | Emotion, warmth, children's stories | "Studio Ghibli background painting style, visible brush strokes, paper texture" |
| Claymation | Charm, personality, playful pieces, food | "Aardman style claymation, plasticine texture, visible fingerprints, miniature set" |
| Anime | Energy and drama | "anime style, cel-shaded, bold outlines"; soft via Ghibli, cinematic via "photorealistic backgrounds with anime characters", energy via "speed lines, dynamic poses" |
| Paper cut-out | Tactile, handmade, editorial to children's book | "paper cutout animation, layered craft paper, torn edges, depth shadows" |
| Line art | Minimalism, teaching, architecture | "pen and ink illustration, continuous line drawing" |
| Isometric | Miniature worlds, systems explained | "isometric illustration, diorama, miniature world" |
| Pixel, retro | Nostalgia, gaming | "pixel art, limited palette, chunky pixels" |
| Mixed media | Live action with animated elements, premium editorial | "live-action footage with 2D animated overlays interacting with the real world" |

Forgiving families degrade gracefully: flat 2D because simple shapes hide flaws, watercolor, clay and paper because texture absorbs them. Geometry-strict families (isometric, pixel, low-poly) break visibly when the model smooths the grid, so they need precise start frames. The expected family is also a choice: every software explainer is flat isometric with blob people, so an unexpected family stands out before a frame moves. A technique word sharpens a family (risograph, gouache, cel, halftone, screen print, rubber hose). If a model ignores a studio name, describe what the name stands for (palette, line, render, texture) and let the style key carry it as a reference. Avoid the uncanny middle: a character seventy percent real is worse than one at thirty. When realism creeps in, push further into stylization.

## Characters

Design for readability and consistency, not realism.

- Silhouette first: recognizable from the outline alone, with the distinguishing feature exaggerated. Shape language casts: circles read friendly, squares reliable, triangles dynamic.
- Simpler than seems necessary: three colors, one or two features, clothing described in one phrase. Detail can be added later but not removed.
- Write the character down once: proportions and head-to-body ratio (a big head reads cute, one to eight heroic), palette, the feature, the resting expression. After that, references hold identity: a turnaround sheet generated as one image so the views agree (`skill-ai-influencer`). The prompt names the character and directs the action; re-describing the looks invites a redraw.
- Do not redesign a character the user brings: its artwork is the style key and the sheet's source.
- Slight color drift is acceptable, especially in painterly styles. Changed proportions, a missing accessory or a 2D character turning 3D means regenerate. A character that keeps morphing needs a simpler design, not a longer prompt.

## What only animation can say

Devices for a piece that argues or explains: a metaphor made literal (silos as walls crumbling), committed to as the piece's visual language; transformation, weeks compressed into seconds; scale, galaxy to atom with something revealed at every level; impossible physics in service of the story; emotional abstraction (anxiety as closing walls, relief as expanding space); the accumulation reveal, scattered elements resolving into the mark, which works only if they look random until they don't.

## Motion

- One main action per scene, at full energy. Secondary motion sits below it and ambient life below both, so the eye knows where to look.
- Keep the world alive in its own way. Characters moving in a frozen frame is the tell of a dead world, so every prompt carries the world's life (wind in leaves, water, smoke, a crowd). A breathing background or a camera drifting nowhere does not count.
- Exaggeration is animation's realism: surprise is eyes twice their size, weight is the ground cracking. Describe anticipation and follow-through ("she crouches, then leaps", "coat trailing behind").
- Timing is part of the style. Drawn and stop-motion styles look wrong at smooth full-rate interpolation: on twos for cel and 2D, the stutter cadence for clay and paper. If the model ignores that direction, `edit_timeline` updateClip with `patch: {holdFrames: 2}` holds every drawn frame for two frames (3 for threes) on a video, image or nested clip.
- Motion carries the energy: luxury glides on long eases with no bounce, tech snaps clean, playful overshoots and squashes, organic flows on soft sine eases. Up is growth, left to right is time passing, expanding is revelation, contracting is focus.
- Fix feet skating (a walk whose contact points slide) with less travel, not more prompt: a shorter distance, or the camera held on a planted contact with the movement at the frame's edge.

## One style across every shot

1. One style key: the image that defines the language, made with `generate_image` and carried into every later generation. Pass it as `reference_refs` where the model takes references and as the `source_refs` start frame where it does not; `end_ref` anchors a landing when one is needed.
2. A style block repeated verbatim in every prompt: the family anchor, the exact palette with its background color, texture level, line weight, lighting approach. Leave the background out and each scene improvises a different one.
3. One image model for keys, sheets and frames and one video model for motion, each chosen from `list_models` and kept for the whole piece.
4. Many short shots rather than a few long ones: a character holds best over a few seconds, and repeated actions do not come out the same twice. Keep the subject at a similar size from shot to shot, since a big jump in scale shows drift. A cutaway hides a seam. A `seed`, where the model takes one, keeps a remade shot close. Light and tone drift more than the subject, so match them in the edit (`skill-color-grading`).

Fewer colors hold better than many. Give the most important element the most saturated color. A gradient sliding from violet into blue is what generated work falls into when nobody chooses. A brand's palette, from what the user gives or their brand kit (`manage_brand_kit`), goes into the style block as hexes. Do not generate message text (titles, supers, claims) into frames; put it on the timeline as text, where it stays exact. Text that belongs to the world (a shop sign, a jersey number) is generated with the world. The exception is an explainer's designed frame: a still made with the copy quoted, then animated (`skill-explainer-videos`).

## What generated animation gets wrong

The photographic tells (sourceless light, flat planes, clean air) are in `skill-ai-filmmaking`. Animation's own:

| The tell | The fix |
|---|---|
| The template look: isometric office, blob people, blue-green palette | An unexpected family for the category, or the expected one pushed hard |
| Photorealism leaking into a stylized frame | The style anchor as the first words of the prompt, the style key riding as reference |
| The uncanny middle | Further into stylization |
| Everything moving at once, wobbling | One or two elements moving with intent while the rest holds steady; a camera move only with a destination |
| Motion that drifts or wobbles between shots, which viewers feel without naming | Short shots, each from its own start frame, the same scale from shot to shot |
| Rough 3D | Stylized 2D, motion graphics or painterly, which read as design; 3D only where it can be clean |
| Style drift between scenes | The protocol, without shortcuts |
| New colors every scene | The locked palette, background color named in every prompt |
| Too-perfect surfaces, uniform fills | A texture intention on every surface, hand-drawn wobble, directional light even in flat styles |

## Sound

Animation has no set audio. Every sound is designed, exaggerated to match the picture and timed to land on beats; silence is a dramatic choice. Sound carries more of how polished a piece feels than frame-perfect motion: a piece that looks right and still feels cheap is usually missing its sound. Build it with `skill-sound-design`.

## The order across tools

`list_models` for the image and video models; `estimate_cost` prices the shot list in advance. Judge the style key and the scene frames as stills before animating anything, while a change costs one image; whether to show them to the user first is the agent's call. Make the style key with `generate_image` and the character sheet per `skill-ai-influencer`. Run one test shot with `generate_video` against the key before the rest, since only a moving frame proves the key and the block. Then the shots, each a job, with `get_jobs` returning the items. Assemble with `manage_timelines` and `edit_timeline`, judge frames with `inspect_timeline`, add sound per `skill-sound-design`. The piece is in the app from its first media; `render_timeline` makes a file for use elsewhere. Bound regeneration on drift: a character that fails twice gets a simpler design.

## The check

Any two frames from different scenes, side by side, read as one piece. The main character is recognizable from silhouette alone. Any paused frame shows a world alive in its own way, nothing breathing or drifting to fill time. The palette reads as chosen.

## Handover

- Set the timeline to the destination's ratio before placing the first shot.
- Every sound designed and placed; no shot silent by accident.
- The last frame held, nothing mid-move on it.
- The piece is in the app from its first media, where it plays; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: one shot re-rolled alone, the piece at another shape, a recurring character kept for later pieces (`skill-ai-influencer`), titles and type in code (`skill-motion-graphics`), a score and designed sound.
