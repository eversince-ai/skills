---
name: eversince-ai-influencer
description: "An AI person or character made once, face and voice, and kept identical in every scene: the reference set, generating from it, posting it."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-ai-influencer"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

A consistent character is a person or designed figure who looks the same in every generation. References hold identity; descriptions do not. A character who exists only as words in prompts becomes a different person each time. The work is building the reference set and generating from it. Shot design is `skill-ai-filmmaking`; a drawn or rendered world is `skill-ai-animation`. Build a character on a real person's likeness only from photos that person supplied for it; otherwise invent the character. A brief, reference or brand from the user outranks every default here.

## The plan

1. list_models {type: "image"} → the short list, each model's types and price; list_models {model: the one for the set} → its full entry: image-to-image and ingredients-to-image among its types, its guidance on holding a person across references
2. generate_image {type: "text-to-image", model: from step 1, prompt: the written character with realism markers, the register's camera language} → job; get_jobs → the canonical
3. generate_image {type: "image-to-image", model: from step 1, source_refs: [from step 2], prompt: one grid of the angles, expressions and distances the jobs need, in the same style} → job; get_jobs → the sheet
4. inspect_media {refs: [from step 2, from step 3]} → the sheet checked against the canonical
5. generate_image {type: "ingredients-to-image", model: from step 1, reference_refs: [from step 2, from step 3], prompt: the scene, the character named, the action} → job per scene; get_jobs {ids} → the scene images
6. generate_video {type: "image-to-video", source_refs: [a scene from step 5], reference_refs: [from step 2], prompt: the action} → job per shot; get_jobs {ids} → the shots

A speaking scene, on a model whose entry has `max_characters`:
7. search_library {query: "voice <name>"} → a voice already kept for the character, which is the voice_ref; then step 10
8. otherwise the voice recording: generate_audio {kind: "voiceover", voice_id: the character's, text: three or four natural sentences} → an audio item (or the user's own recording, or the first generated video of the character the user accepted)
9. update_items {updates: [{ref: from step 8, name: "<name> · voice", add_tags: ["<name>"]}]}; organize_items group with the set
10. generate_video {type: "ingredients-to-video", model: that model, characters: [{image_refs: [from step 2, from step 3], voice_ref: from step 7 or 8}], prompt: the scene, @Element1, and the line in quotation marks} → job; get_jobs → the generated video. Every later generated video of the character passes the same voice_ref

## What the job starts from

The jobs the character will do (talk to camera, appear in scenes, front a brand, mascot a product) decide what coverage the set needs. Decide photoreal or designed; they are built differently. Check what exists: approved photos, an earlier character, a style the brand owns. `list_models` with `model: "<id>"` shows the chosen model's reference modes, multi-character tags and voice behavior.

## What makes an AI influencer work

Facts for the user to weigh before building one:

- **Engagement usually trails real creators.** CreatorIQ found Lil Miquela at 0.6% engagement on BMW posts against 3.6% for BMW's human creators, and a 2024 study of about 100,000 posts found human influencers drew more engagement than virtual ones.
- **Fit beats reach.** The best-documented earner, Aitana Lopez, has a few hundred thousand followers and beat human averages on fashion and beauty campaigns, because the persona fits the brands it works with.
- **The persona comes first and stays fixed**: an age, a life, a style, a niche, a point of view, posted steadily, so followers follow someone, not a series of good pictures.

## Invent the person

Write the character before generating anything: age, where they live, what their life plausibly contains, and why anyone would believe them in their jobs. Recognition comes from two or three strong visible marks (the glasses, the gray streak, the always-the-same jacket), not a list of facial features. State ethnicity once at invention; some models default to one appearance. Hair, wardrobe and the state of their room come from the bio, decided once and kept as identity.

## A brought face

Don't invent a real person (approved photos of a founder, a hired actor). Choose the canonical from what exists: the frontal, evenly lit frame showing how the person looks now. Supplied photos disagree (haircut, weight, light), so pick one appearance as canon. Regenerate a canonical from the best photo only when no supplied frame is usable. If the person's own footage exists, a model that drives a character image with a real performance video is the shortest path to motion; `list_models` says which take one. The likeness holds best from a recent, sharp, unfiltered, unretouched frame with the whole face visible and the eyes open, and in shots with the face toward the camera, slow motion and framing medium close or closer. The body holds with a full-body photo beside the face. Words that change a body or an age (younger, slimmer) pull the result away from the likeness. Use only the person's own likeness and voice, or ones they have permission for.

## Making a photoreal character real

- Put two or three realism markers in every prompt: visible skin texture and pores; subsurface scattering, the warm translucency where light passes through skin; hair with individual strands catching light; fabric with real weave and drape. Without them the face looks plastic.
- Keep expression understated: a slight smile, a knowing look. Wide eyes and open-mouthed surprise read as generated on sight. Make big expressions as set variants for moments that earn them.
- Don't prompt asymmetry, moles or quirks; models over-render them into caricature. Individuality comes from the markers and the light.
- Describe what you want (skin texture, hair strands, real fabric), not the plastic look to avoid. Several models render the avoided look instead of excluding it.

Pick one of two registers from the jobs. Casual, phone-shot: everyday phone content with deep depth of field, the whole scene sharp. Describe it as "phone camera, deep depth of field", never by device name, which can put the phone in frame. No bokeh, film stocks or cinema lenses; they read as produced. Produced, cinematic: a film stock chosen for skin (Portra's warm rolloff, Vision3's robust tones), a camera body as a dataset selector, shallow focus, textured motivated light. Carry one stock through every generation of the character in that world.

## The reference set

- Start with the canonical: one portrait, the character at their most themselves. Generate every other member of the set from it, passed as the reference, so the set holds one identity instead of drifting. The canonical alone can carry a character through a single piece.
- Make coverage as a sheet, not single rolls. Consistency holds better inside one generation than across many, so generate the angles, expressions and distances the jobs need as one grid with the canonical as reference (`list_models` says how a model wants a grid prompted and sized). Keep the sheet whole as one item and pass it beside the canonical where a generation needs it: the sheet anchors the variety, the canonical carries the fidelity. Don't cut out a cell to stand alone; generate a needed look at full size from the canonical with the sheet as second reference.
- Check every sheet and variant against the canonical before it joins the set. One drifted member poisons everything generated from it.
- Why references: in a 2026 test with fixed seeds, one reference photo held the face far better than the prompt alone (a median match of 0.78 against 0.53). Training a custom model on the character holds longest, from 15 to 30 images, but that is not available here; references are the method.
- For video, a sheet reads best on a plain mid-gray background with even soft light and no props: full body from the front and the back plus a head-and-shoulders close-up, so the model separates the person from any place. Crop a close-up from it (`extract_media` crop) as the face's one source and pass it where the face matters most. For a creature or a face that talks, two close-ups, mouth open and closed, keep the mouth from glitching.
- Two characters of very different size in one shot hold their scale from one image showing them together at the right proportion: a composite made from both sheets with image-to-image, passed as a size reference. The prompt states the count ("exactly one rider, one dragon") and that neither is enlarged.
- When the jobs repeat a place, the world is part of the identity: give their room, desk or kitchen references too. Rooms drift as visibly as faces.
- A designed character (a mascot, a drawn figure) holds identity by geometry and style: simple strong shapes, with the family, line, palette and proportions stated once and carried in every reference. Its canonical is a turnaround (front, side and three-quarter) generated as one image so the views agree.

Size the set to the jobs named; `estimate_cost` quotes it. A canonical plus one sheet covers most single pieces. Add world references and extra variants when a job needs them.

## Holding identity in use

Every generation with the character passes the canonical or the variant closest to the shot. The prompt names the character and directs the action; describing their appearance competes with the pixels. For a look no variant covers, generate the variant from the canonical first, check it and add it to the set; don't improvise it inside the scene. A strong style name beside the reference overrides the identity anchors and redraws the face, so carry the style in the canonical instead.

A character also has a state, separate from identity: the torn sleeve from the fence, soot under one eye, hair wet since the rain. State changes once and persists until the story changes it back: name it in every generation after the change, not in the shots before it. State sides as the character's own left and right, never screen left, and check them on the way back. Generations mirror, and a bracelet that swaps wrists is what a viewer notices without knowing why.

## Two or more characters in one scene

Characters stay apart best with a sheet each; one image holding both is the main cause of blending. Use the same layout for each sheet and the same outfit in every panel. To tell them apart, the model needs different wardrobe colors and stated age, build and height, with heights in centimeters and their ratio, repeated in every scene prompt. The prompt counts them ("one Lucy and one Jerry") and says the sheets give identity only, not a layout; otherwise a sheet's panels come back as extra people. Each character keeps their side of the frame from the first frame across shots. Assign hands (which hand holds the remote, whose arm the other holds) and keep them clear of what they are not to touch. A location reference shows the place empty, with the instruction that it gives geography and palette only and the camera finds new positions in it; otherwise its composition becomes the first frame. A recurring prop gets its own sheet the same way, its size and colors fixed.

## What the models get wrong here

| The default | Why it fails | Instead |
|---|---|---|
| Describing the face in words each generation | Appearance drifts, and detailed physical descriptions can trip safety filters | The reference carries the face; the prompt carries the action |
| Quality anchors on a phone-real character | On models whose guidance says anchors push editorial polish, the opposite of a person filming themselves | Realism markers for phone-real; anchors only for produced work |
| One reference for every situation | A portrait forced into full-body scenes or new wardrobe drifts | The variant built from the canonical first |
| Letting each scene generate the voice | Native audio invents a new voice per generation | One kept voice per character (A voice that stays) |
| Multi-character scenes with unlabeled references | The model guesses who is who | The model's reference tags, one per character |

## The order across tools

Make the canonical, the sheet and the variants on one image model, chosen once from `list_models` type image unless the user names one. Its types include image-to-image and ingredients-to-image, so the sheet and every scene can take the canonical, and its entry describes holding a person across references and edits. Bring in a second model only when the first declines or drifts; each model draws the same reference a little differently. A video model's entry says whether it takes `reference_refs` beside a start frame or the start frame alone. Make one canonical with `generate_image` and check it; make another only if the first misses the written person. Make the sheet and each variant as image-to-image generations from the canonical. Every use of the character passes the canonical (and the sheet where the shot needs its range) in `reference_refs`, or as `source_refs` on models that take a start frame only. A speaking character follows the next section. Each generation is a job; `get_jobs` returns the item. Fix drift found later in the set (a missing or wrong variant), not by describing harder in a scene prompt. If a character exists only as scattered generations, choose its canonical from them, the one most like the character at their best, and build everything after from it.

## A voice that stays

Two terms here mean one thing each. A "generated video" is what `generate_video` makes. A "voice recording" is the library item the voice is learned from: an audio or video item in which one person speaks.

A video model that makes its own sound gives a speaking character a new, random voice in every generated video. A character who speaks in more than one generated video can keep one voice, as below; a character in a single scene does not need one.

Two ways to keep a voice:
- Inside the generated video, on a video model whose `list_models` entry has `max_characters`: the character speaks on screen with the lips matching the words. This is simpler when the chosen model has it, and the rest of this section covers it.
- Under the generated video, on any other video model: make every line with `generate_audio` voiceover in one `voice_id` (`list_models` type voice), lay it under the shots on a timeline, and make the generated video silent or set it to volume 0. The lips do not match the words.

How a kept voice works. In `generate_video`, each character in `characters` has a look and, optionally, a voice. The look is `image_refs` (the canonical first, then up to three more angles from the sheet) or `video_ref` (a video item of them). The voice is `voice_ref`: a voice recording with at least 5 seconds of one person speaking clearly. A recording longer than 25 seconds is cut to its first 25 seconds, so those seconds must be the person talking, with no music and no other voice. The first time a voice recording is used, the model learns the voice and that library item keeps it: the item is tagged `voice` and the model's id, and `get_items` shows `voice_for` with the model and the date. Every later generated video that passes the same item as `voice_ref` speaks in that voice, in this session or any later one. A different item is a different voice. The recording itself is not played; the character says the prompt's new lines in that voice.

For a character who speaks, in order:
1. Look for a voice already kept: `search_library` with query "voice" plus the character's name. A result tagged `voice` with that name is the `voice_ref` from here on; go to step 4.
2. Otherwise pick the voice recording, the first option that exists:
   - the user gave a voice recording, audio or video: use that item (bring it in with `import_media` if it is not in the library);
   - the brand or the character already has a `voice_id`: make a `generate_audio` voiceover of three or four natural sentences in that voice, which creates an audio item;
   - neither: make the first generated video of the character with no `voice_ref`, so the model invents a voice. Make it 6 seconds or more, mostly the character talking, in a quiet place with no music. When the user is happy with how the character sounds, that generated video is the voice recording.
3. `update_items` on the voice recording: name "<name> · voice" and `add_tags` with the character's name; then `organize_items` group puts it with the character's images. This is how step 1 finds it in a later session.
4. Every generated video of the character passes that same item as `voice_ref`. Put the character's lines in quotation marks right after the character's tag (`@Element1`), in a language the model's entry lists.

Several characters: each gets its own entry in `characters`, cited in the prompt as `@Element1`, `@Element2` in the order given. Write each line after the tag of the character who says it. Each character has its own voice recording. `list_models` gives the limits (how many characters, how many with a voice, how many as video). A video-to-video edit takes characters as images only, with no voice.

Cost and failure: making a voice adds a fraction of a credit, once; later generated videos that reuse it add nothing. A character given as a video item is charged as video input. If the model cannot learn from a voice recording, that generation ends refunded; a different recording fixes it.

## Posting an AI persona

Before an AI influencer posts, give the user these facts to act on:

- TikTok and YouTube ask for their AI label on realistic AI content; Meta labels photoreal AI video and audio and asks creators to mark it. A profile line saying the persona is AI-made, plus each post's AI label, covers both levels.
- In the US, an AI persona that endorses a product needs the same disclosure as a paid human endorser, and the persona should not claim to have used a product it cannot have used. India's ad standards require virtual influencers to say upfront that they are not human.
- The EU AI Act requires AI-generated people and voices to be marked as such from August 2026.
- Disclosure works best early and visible: in the first seconds or the first line, not buried in hashtags.

## The check

The lineup test: a new generation beside the canonical, seen cold, reads as the same person to someone not asked to look for it. The coverage test: the set as it stands can do the jobs named at the start. The recognition test: the distinctive marks are visible in every member at thumbnail size.

## Handover

- Each generation appears in the app as it is made, so the user sees the canonical, the sheet and every scene there without publishing anything.
- Name the canonical, the sheet and each variant for the character with `update_items` ("<name> · canonical", "<name> · sheet") and group them with `organize_items` group, so later work finds them.
- Name a speaking character's voice recording "<name> · voice", tag it with their name, put it in the same group, and state it in the answer (with its `voice_id` if it came from a voiceover), so a later session passes the same `voice_ref`.
- The answer names which refs make up the set, for every later generation to pass.
- Also possible: a fixed voice, a first short scene, the character in other outfits or styles, a set of posts for social, the character talking to camera.
