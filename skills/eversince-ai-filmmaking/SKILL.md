---
name: eversince-ai-filmmaking
description: "AI-generated video that looks like film: how shots, light, movement and coverage are designed and prompted. Drawn frames are animation."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-ai-filmmaking"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Cinema is the design of film-like video: what the camera does, where the light comes from, how shots vary and cut, and the words that get a generation model to make those choices instead of the average of everything tagged cinematic. It applies to generated shots and brought footage alike; with footage, the same eye works in selection and the cut. Drawn, painted or rendered worlds are `skill-ai-animation`, which takes its shot design from here. A face that recurs across shots is `skill-ai-influencer`. A brief, reference or brand from the user outranks every default here.

## The plan

1. list_models {type: "video"} → the models, their types and prices; list_models {model: the one chosen} → its limits and guidance; estimate_cost for the shot list's price
2. generate_image {type: "text-to-image", prompt: the start frame with motion space} → job; get_jobs → the frame item (per shot that grows from a frame)
3. generate_video {type: "image-to-video", source_refs: [from step 2], prompt: the shot with its verb, end, light and the piece's look phrase, settings: {duration}} → job, one per shot; get_jobs {ids} → the shot items
4. manage_timelines {create: {name, aspect_ratio: the destination's, clips: [{ref: each shot} in cut order]}} → timeline_id
5. edit_timeline {timeline_id: from step 4, operations: [updateClip {clipId, patch: {transitionIn: {style, durationInFrames}}} where a cut is not hard, trims where a shot runs long]}
6. sound per skill-sound-design on the timeline from step 4
7. inspect_timeline {timeline_id, seconds: [the cuts]} → frames at the cuts
8. check_timeline {timeline_id}
9. publish_preview {timeline_id}: returns the preview the piece already has, its app_url the link to hand over

## What the job starts from

Story decides an AI film more than its pictures: at Runway's AI Film Festival, audiences responded to films with a real story and feeling, and reviewers found most entries thin on plot. Name the feeling the viewer has at the end, precisely (for a single shot, one feeling in one sentence). Know what exists: real footage, generated material, or both. References the user supplied outrank any description of a look. Work in order: feeling, language, shot list, frames, motion, cut. Prompt shot one before the language exists and every later shot inherits a look chosen by accident. Sizing: fifteen seconds carries three to five shots and one beat; thirty carries six to ten and one turn; sixty carries a full arc. Generated video is the most expensive work there is. Price the shot list in advance (`estimate_cost`) and judge each start frame as a still before animating it, where a change costs one image. Whether to show the list and its cost first is between the agent and its user.

## The language of the film

Each kind of film has its own camera language. Drifting between them by accident is the failure; a deliberate shift lands on a beat.

- Anthem: sweeping and unhurried, wides that build scale, slow reveals, warm light, crane and dolly confidence; the feeling arrives first, the subject enters late.
- Product hero: the object is the spectacle, macro detail, controlled light, slow orbits, rack focus from detail to whole; premium is the product's own color, accurate, no film look over it.
- Documentary: the camera is a witness, handheld energy, natural light, close-ups with real texture, moments that feel caught.
- Narrative naturalism: motivated moves, eyeline-driven coverage, light from where the room says it comes.
- Formal: locked frames, symmetry, the subject placed and left there; the stillness is the tension.
- Mood piece: atmosphere over information, fragments, unusual angles, silhouettes, negative space, heavy grade.
- Face to camera: the face is the frame and stability is trust, at most a slow push in.

Feeling maps to picture: longing is shallow focus, warm, slow, subjects looking away; tension is tight framing, handheld micro-movement, cool and desaturated, confined space; joy is width, movement, open light; intimacy is extreme close-ups, soft light, stillness, texture; awe is a small subject against a vast environment, the camera revealing scale; urgency is fast cuts, hard light, motion filling the frame.

## The frame

- Shot sizes are grammar: extreme wide for scale and geography, wide for context, medium for connection, close-up for emotion, extreme close-up for emphasis, insert for a story-critical detail. Wide to medium to close builds intimacy; close to extreme wide is a reveal; a sudden jump in scale is emphasis; every shot at one distance is monotony.
- Build three planes of depth. A generated frame defaults to a subject floating on one flat plane; an out-of-focus foreground element (a shoulder, a doorframe, a plant) is the single highest-impact habit.
- Give light a source and a direction: "warm light from a window camera left, soft fill from the right", not "warm lighting". Rembrandt is the portrait default, split light is drama, rim light separates, practicals in frame motivate color, light through haze shows depth; hard light is drama, soft is warmth, time of day is a palette.
- Perspective comes from distance, not glass. A wide lens exaggerates depth because it is used close; a long lens compresses because it is used far back. Name the lens width and the distance: compression without the camera moving back returns a crop of the same perspective. Anamorphic reads as cinema; shallow focus is intimate, deep focus documentary and epic.
- Height and angle are two controls. A camera on the floor pointing level and one at eye height tilted up are both "low angle" and look nothing alike, so name the lens height and where it points.

## Movement

Each move is a statement: push in narrows attention, pull back reveals context, an orbit examines, a crane shifts power, tracking is journey, handheld is presence, locked off is authority, a slow zoom is creeping tension, a rack focus redirects attention without a cut, a whip pan is chaos and a dolly zoom is dread, both rare. Lock off any move that cannot answer "why is it going there".

Models confuse three kinds of move: translation carries the camera through space (dolly, track, crane), rotation pivots it in place (pan, tilt, roll), and a lens-only change moves the glass (zoom, rack focus). A push in is translation; a slow zoom is not. Ask for one camera idea and one action per shot: an orbit, a push and a zoom together come back pumping and reversing. Locked off is strict: no drift, no zoom, no focus change.

To make a move repeat, write it as a measured operation: the rig first ("on a tripod at 1.5 m lens height"), then one continuous move named once ("one continuous orbit, one direction"), with its numbers (radius in meters, a pan in degrees, a zoom from 18° to 84°), its speed curve ("constant speed, settling in the final half second"), its end ("into a static hold on the closing frame"), and an event it lands with ("timed so the landing frame and the cup touching the table arrive together"). Describe a dolly with strong parallax, near objects sweeping past the frame edges; a zoom with zero parallax, the camera's position and height identical in the first and last frames. Moves are the one place to name a lookalike and rule it out ("a zoom, not a dolly; no travel"), since the model substitutes the nearest move; "no reversal" keeps a move from turning back on itself. A tracking shot holds its angle ("a strict side view throughout"), the subject at constant size and the layers in front and behind moving at their own speeds. The same move wording, lens and light across shots and re-rolls keeps the camera logic consistent.

Life speed is the default. Slow motion is a spice one moment earns; a piece where every shot is slowed reads as a screensaver.

## Coverage and the cut

A scene is a set of shots that cut, sizes varying, the end energy of one shot motivating the opening of the next (a hand reaching in close cuts to the action in wide; a slow dolly out lands in an establishing frame). "And then" is the weakest transition logic. In a dialogue scene, establish the geography once, then cut shot and reverse shot from the same side of the line between the speakers (crossing it flips who looks which way), and cut to the listener as often as the speaker.

Cuts: hard cut for energy, match cut for a metaphorical link, scale jump for emphasis, movement match for flow, J-cut and L-cut so sound leads or lingers, smash cut for shock, cutting on action where the motion masks the edit. Pacing is felt: a four-second face can feel longer than an eight-second wide with movement, a dense frame needs longer, a sparse one reads instantly, and the power move is the sudden shift, rapid into stillness or stillness into burst.

A scene can come out of one generation with the cuts directed inside it, or shot by shot and cut on the timeline. One generation holds light, faces and place together but cannot re-roll a single shot; shot by shot can, and puts continuity on the consistency signals. `list_models` says which a model supports, as a shots array or as prose in the prompt.

## One world across shots

Independently generated shots read as one film when the consistency signals repeat verbatim in every prompt: the same look phrase, light temperature, lens character and atmospheric texture. Pass a recurring subject as a reference (`reference_refs`) where the model takes one, and as the same start frame (`source_refs`) where it does not; re-description drifts. Hold light direction across a sequence unless the shift is the point. Footage not shot together reads as one world once matched, and the match comes before any look: `skill-color-grading`.

Between shots of one scene, the last frame of one can be the first frame of the next (`extract_media` frame at its end, passed as `source_refs`), so the cut hands over cleanly. A long shot that keeps breaking down becomes two shorter shots joined that way. AI directors in 2026 plan about eight to fifteen shots for a minute of film and approve every look as a still before animating any of it.

## The register

Think in the full professional vocabulary; prompt in what the model's training data can see. These models learned from captioned frames, so the words photographers tag their work with change the output, and invisible set mechanics do nothing.

- Works: lens width, named lighting, shot types, camera moves, anamorphic, depth of field described, palette names, golden hour. On image models and older video models, equipment names select training data rather than aesthetics: a cinema camera body pulls highlight rolloff and color science, a film stock its whole look. The newest video models (Seedance 2.5 among them) follow a look described in words more reliably, and gear names can break a complex move there; write the look instead (tonal latitude, highlight roll-off, grain, color response).
- On a video model, give lens width as a field of view in degrees from a few steps; millimeters are read loosely: 107° (14 to 16 mm, huge interiors), 84° (20 to 24 mm, wide, group blocking), 63° (28 to 35 mm, observational), 47° (40 to 50 mm, neutral eye), 29° (75 to 85 mm, portrait compression), 18° (100 to 135 mm, close portrait), 12° (180 to 200 mm, detail), 8° (300 to 400 mm, extreme compression).
- Unreliable: f-stops, ISO, lighting ratios, grip equipment, LUT names. Describe the visual result instead ("deep shadows on the left of the face, background falling to darkness").
- Carry one look through every prompt of the piece; stacked competing signals produce mud.
- "Photorealistic" and "cinematic" do no work; they stand where a choice should be. So do mood words: write "tense" as what is seen ("he freezes, his fist closes slowly, light only from the side, half the face in shadow").
- Write what happens, not what should not ("stays upright, feet planted", not "does not fall"). Give speeds in km/h, haze in percent or meters of visibility, white balance in Kelvin held across a scene. Tie color to a material and the light on it ("a crimson silk scarf catching the cold tungsten spill from the corridor") rather than listing it. Name left and right from the camera.

A shot needs a verb; without a directed action the result is a still that subtly moves. A shot grown from a start frame is decided by that frame: compose it with motion space in the direction of movement and a clean silhouette. Say where the shot ends as well as where it starts, where the camera travels, and what the light does on the way: an arc that opens on side light closes on frontal unless the prompt holds it.

The same shot twice. Weak: "A cinematic shot of a woman drinking coffee in her kitchen, professional lighting, photorealistic, 4K quality." Strong: "Close-up, 29° anamorphic, shallow depth of field. A woman's hands wrapped around a ceramic mug, steam rising through a beam of morning light from a window camera left, 4300K. Her out-of-focus shoulder in the near foreground, the kitchen dissolved into oval bokeh behind. Warm amber tones, subtle grain, gentle halation around the window highlight, soft highlight roll-off." The light has a source, a direction and a temperature, the foreground holds a plane, the look is described.

## The long prompt

The newest models carry a whole scene, up to thirty seconds, in one generation, and a long, exact prompt is what holds it. Nothing carries over between generations, so each prompt is complete on its own: no "as before", and no tag for something not in the shot (a tagged reference is forced into frame). The blocks, in order, only those the shot needs:

1. Scene: what happens, where, when, who stands where, in one or two sentences.
2. References: each tag with a minimal anchor and "matches the reference exactly"; the reference sets the look, the text sets what happens. State small text, logos and exact colors in words too, since the model drops them.
3. Location in layers: foreground, midground, background, where the camera is, where the light comes from.
4. First frame: who is where, facing which way, looking where; the first frame holds the subject.
5. Form: one continuous shot ("one continuous shot, the camera does not cut on its own"), cuts in order (CUT 1, CUT 2) when the order matters and the timing does not, or timed cuts when beats land on a clock ("0.0 to 1.0 s ... 1.0 s HARD CUT"), with "cuts only at the stated points".
6. Lens: shot size and field of view per segment, "no drift mid-segment" in a multi-shot.
7. Camera: height, distance, movement, focus, and why it moves.
8. Action, then performance where acting matters (muscle-level emotion, eyeline, breath), then physics (mass, inertia, contact, fluids).
9. Light, then color where the grade is strong, wardrobe where costume matters, sound only as needed.
10. Format and look at the end (ratio, real time or slow motion per segment, grain), then a few short positive lines restating what holds: the faces, the props, the light, the count of people.

Across internal cuts, hold the same people, geometry, screen direction, light and wardrobe. A whip pan needs about 0.8 s to blur or it renders as a hard cut; change speed (real time to slow motion) only at a hard cut.

## Dialogue and sound in the prompt

Most frontier video models now generate sound with the picture, but not all of them sync lips: some give ambience and effects only. `list_models` with the model says which. Where a model speaks, Google's guide for Veo sets the form other models also follow:

- Speech in quotation marks, with who says it: `A woman says, "We have to leave now."`
- Effects labeled: `SFX: thunder cracks in the distance`.
- The room tone named: `Ambient noise: the quiet hum of a starship bridge`.

Lock the dialogue before generating, since every take is timed to it. Lip sync is weakest where it shows most, so a hero close-up gets the most takes. Where a model makes no speech, generate the shot silent and lay the voice on the timeline (`skill-voiceovers-and-music`).

Prompt order differs by model: Google puts the camera first (cinematography, subject, action, context, style); Kling builds the scene first and puts the camera last; Seedance binds its references, then a one-line summary, then numbered shots. `list_models` gives each model's own form.

## Editing generated shots

- **Generate more than you keep.** Productions keep a fraction of what they generate; one animated episode cut 164 clips to 41. Regenerate a shot that is wrong in idea; trim one that is right but rough.
- **Keep the best seconds.** A generated clip often ramps up slowly before its real motion; cut in where the action starts. Often five good seconds come from a fifteen-second clip.
- **Let sound carry the cuts.** Music, narration or a room tone running across a picture change (an L-cut) hides the seam between shots made separately.
- **Take off the plastic.** Generated footage comes back too sharp, with waxy skin. A light pass of grain (the `noise` effect, amount about 0.05 to 0.15) and one grade across the shots (`skill-color-grading`) makes them sit together as film.

## What generated video gets wrong

| The tell | The fix, in the prompt |
|---|---|
| Sourceless, even light, the biggest giveaway | Every light has a visible or implied source, with direction |
| Subject floating on one flat plane | An out-of-focus foreground element in the frame |
| Optically clean air | Haze, dust, steam, rain between camera and subject |
| Poreless, symmetrical faces | Skin texture, subsurface warmth where light passes ears and fingers |
| Showroom-empty rooms | One or two lived-in details: the coffee ring, the stacked mail |
| Highlights clipped to white, blacks crushed | Halation around lights and windows, shadows lifted to dark grey |
| Grade instead of composition | A frame that fails in black and white is not saved by orange and teal |
| Everything in slow motion | Life speed by default, the action prompted at real speed |
| A camera drifting nowhere | Locked off, or a move with a destination |

## The shape of the frame

A different ratio is a different composition, not a crop. Vertical favors vertical movement and puts the subject off dead center with the top 12% and bottom 24% clear for platform UI; square carries symmetry and centered subjects; ultra-wide makes the environment the story, with slow horizontal moves and no tight close-ups wasting the width. A wide two-shot in landscape becomes two close-ups in vertical.

## The order across tools

`list_models` first: a short list with a price per model, then `model: "<id>"` for one model's references, durations, audio and multi-shot form. Hold the choice for the whole piece, since each model has its own fingerprint; a shot that needs a move another model does better can use it, with the same references and look phrase so it still reads as the same film. A model with fixed lengths (4, 6 or 8 seconds) sets the shot list: plan shots at those lengths and trim on the timeline. The model's aspect ratio and the timeline's setAspectRatio are separate settings; neither follows the other. One generation with directed cuts suits a scene whose light and faces must hold together; shot by shot suits a piece re-rolled one shot at a time; a 20-second piece of three to five shots takes either. Footage the workspace holds of the subject (`search_footage`) can fill shots, with generation for the rest or the whole piece; grain, halation and a film look on the finished cut are `skill-color-grading`.

A start frame is a `generate_image` job whose item becomes `source_refs` on `generate_video`; `end_ref` anchors a landing on models that take one. Each generation is a job; `get_jobs` returns the item. Assembly: `manage_timelines` create with the shots as clips in cut order, then `edit_timeline` for trims, transitions and sound-led cuts placed by where the audio sits, sound per `skill-sound-design`. `inspect_timeline` renders real frames of the composite. The piece is in the app from its first media, its app_url in every result; `render_timeline` makes a file for use elsewhere, and a second ratio is a second timeline per `skill-content-repurposing`.

## The check

Judge from rendered frames, not from intent. Someone who only watched it can state the emotional journey in two sentences. Any paused frame is worth pausing on. Every light has a source, every move has a reason, no two adjacent shots share a scale by accident, and the shots read as one world: one look, one light logic, the same faces.

## Handover

- The timeline at the destination's ratio before the first shot is placed.
- Sound under every shot per `skill-sound-design`; no shot silent by accident.
- The last frame held, nothing mid-move on it.
- In the app from its first media, where it plays; the result's app_url is the link the user opens. `render_timeline` makes a file for use elsewhere.
- Also possible: a shot re-rolled alone, the piece at another shape, a film look on the cut (`skill-color-grading`), a score and sound design under it, a recurring face held across shots (`skill-ai-influencer`).
