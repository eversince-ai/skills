---
name: eversince-timeline-operations
description: "Reference for edit_timeline operations: every field, unit and default, effects, grade, masks, transitions, keyframes and camera moves."
metadata:
  publisher: eversince
---

This is Eversince's own skill. When Eversince is connected, `manage_skills` with `action: "get", id: "skill-timeline-operations"` returns its current version; read that one, since this copy can be older. A skill named here as `skill-<name>` is installed beside this one as `eversince-<name>`, and a part of this skill sits in its folder as `<id>.md`.

Reference for edit_timeline operations: every field, unit and default, effects, grade, masks, transitions, keyframes and camera moves.

# The document
- Typed EdlOp[], discriminated on "type", applied in order; each operation sees the ones before it.
- "from" is the frame a clip sits at, on every track; no track packs its clips, and a gap an edit opens stays until closed (rippleDelete closes the gap a removed clip leaves on every track the cut point is clear on, so captions and cued sound keep their place against the picture; a bed running through the cut keeps its shape; closeGaps closes the rest).
- Visual tracks stack by index, higher on top, and hold every picture clip alike (video, image, solid, text, logo, motion); audio tracks sit below. A fresh timeline has one visual track, id "visual-main".
- A clip that would overlap another on its track lands on a fresh track just above. Sound added onto a visual track lands on an audio lane, a free one or a new one. A track left empty is removed once the call has run (the last visual track and any track the call opened stay).
- A detailed read is get_timeline with response_format "detailed". A hand-built media clip's source.url must be a file in the workspace's own storage (a web link is refused: import it with import_media and place it by ref); a GIF clip takes no grade, effects or masks.
- Frames count at the timeline's fps (get_timeline). Every frame field has a seconds twin, one or the other on a value: from → from_seconds, durationInFrames → duration_seconds, trimStart → trim_start_seconds, toFrame → to_seconds, deltaFrames → delta_seconds, atFrame and a keyframe's frame → at_seconds. A detailed get_timeline gives both units per clip.

# Placing
{type:"addClip", trackId?, ref, id?, from?, source?, snap?, durationInFrames?, trimStart?}: a library item by ref, on trackId or the first track of its kind; the clip is built for you, id naming it so later operations in the same call take it as clipId: its own length unless given, 4s for a still, whole passes of its loop (3s at least) for a GIF. A picture shaped unlike its box is cropped to the person a read of the item found, else a portrait one to its upper part, else to the center, unless the op sets transform.focus or objectFit "contain".
- from left out, on a ref or a hand-built clip: after the last clip on that track, counting clips added earlier in the same call, so an assembly is one add per select in order.
- source: [start_seconds, end_seconds] lifts one moment on the way in, the span search_footage and inspect_media hand back, no conversion; trimStart and durationInFrames are the same thing in frames. Where the item's words are timed, the edges move to the quiet between words and the response says by how much; snap:false keeps the span as given.
{type:"addClip", trackId, timeline_id, from?, durationInFrames?, trimStart?}: another timeline of the workspace as one nested clip (type "sequence") on a visual track: plays that timeline, trims like a clip, takes a transform, opacity and fades; no speed, grade or effects of its own; refused for an empty timeline or one that would loop back. Reads back with timeline_id; its clips are edited on their own timeline.
{type:"addClip", trackId, clip:{…}}: a hand-built clip, for what a ref cannot express: {id? (minted unless given), type:"video"|"image"|"audio"|"text"|"logo"|"solid"|"shape"|"adjustment", from?, durationInFrames, source:{url}}. video, image, audio and logo carry source:{url}; text, solid, shape and adjustment carry none.
- text: {type:"text", preset, content, from, durationInFrames}, preset "title"|"kicker"|"lower-third"|"quote"|"callout"|"end-card" (the editor's looks; fills every field left out, content is yours).
- solid: a full-frame wash of one color.
- shape: {shape:"rect"|"ellipse", color, strokeColor?, strokeWidth?, cornerRadius?} in its frame rect; moves, scales, turns and fades on keyframes like a picture.
- logo: an image source with width as a percent of the canvas and position ("top-right"); holds its corner and size when the cut is re-framed.
- adjustment: draws nothing; applies its grade and effects to every picture clip on the tracks below it for the frames it covers, after their own. Its opacity 0..1 is its strength (mixes the grade, scales each effect's amount). Text, logo and motion clips are not affected. On a track above the pictures (addTrack), one look covers the cut.
{type:"insertClip", trackId, ref, from, …}: the fields of addClip; pushes everything at or after the frame right by the clip's length, on every track, and places the clip in the room.
{type:"overwriteClip", trackId, ref, from, …}: the fields of addClip; clears the clip's span on that track and places the clip in the hole.
{type:"replaceSource", clipId, ref}: swap the media, keep the slot: place, length, grade, effects, transitions, fades, framing. The trim resets; a slot longer than the new media is shortened to fit, moving nothing else (a split-screen partner or linked audio keeps its length and wants adjusting in the same call). Media of its own kind: a still for a shot and a shot for a still, audio for audio.

# Cutting and moving
{type:"removeClip", clipId}
{type:"rippleDelete", clipId}
{type:"rippleDeleteRange", from, durationInFrames, trackIds?}: takes a span out and closes it on every track (or the named ones): what sits under it goes, a clip straddling an edge is trimmed to it, what follows comes left by the span, markers with it; a locked track keeps its shape.
{type:"overwriteRange", trackIds, from, durationInFrames, exceptClipIds?}: clears the span on those tracks and moves nothing: clips inside go, clips over an edge are trimmed to it, one covering the span is split around it, its tail becoming its own clip named "<clip id>-<frame>" and unlinked; exceptClipIds are left alone.
{type:"rippleInsert", from, deltaFrames, trackIds?, exceptClipIds?}: pushes everything at or after the frame right by that much, every track unless named (a linked partner on an unnamed track moves with its clip), splitting a clip that straddles the point so its tail moves; exceptClipIds stay where they are.
{type:"closeGaps", trackIds?, from?}: pulls clips left over empty stretches from that frame on, track by track; linked clips move as one, a partner on a track left out coming along as far as its own track allows.
{type:"closeGap", trackId, atFrame}: closes only the gap under that frame on that track.
{type:"moveClip", clipId, toFrame, toTrackId?}
{type:"trimClip", clipId, edge:"start"|"end", deltaFrames, ripple?}: ripple closes or opens the timeline behind the edge as it moves. An edge with no room is refused; one with some room moves as far as it can and the response says how far.
{type:"slipClip", clipId, deltaFrames}: slides the media under a video or audio clip without moving the clip (+ later source, − earlier, clamped to the source's ends; linked partners slip together).
{type:"splitClip", clipId, atFrame, newClipId?}: atFrame is a timeline frame; the clip keeps its id as the left part, the right part takes newClipId (minted unless given; give one to name it later in the call).
{type:"setSpeed", clipId, speed}: rescales the clip and its linked clips to match, clamps 0.1x to 16x, moves what follows by the change; refused on an adjustment or nested clip. A speed ramp is splitClip into parts and one setSpeed per part, with a short blur transitionIn on each piece after the first.
{type:"linkClips", clipIdA, clipIdB, linkId} / {type:"unlinkClips", clipId}
{type:"detachAudio", clipId, trackId?}: a video's sound becomes its own audio clip on the first free unlocked unmuted audio track, else a new one, or on trackId (same span, trim, volume and volume keys; the picture goes silent; speech tools read its words on the video, or on the audio once the video is removed), linked to the video so they move, trim, split and delete together until unlinkClips; the response names the new clip.
{type:"reattachAudio", clipId}: folds a detached audio clip back into the video it came from, while that video still plays the same media.

# Keyframes and camera moves
{type:"setKeyframes", clipId, property, keyframes:[{frame|at_seconds, value, interp?}]}: animates one property; the list replaces that property's whole track, an empty list clears it and the static field takes over.
- Times are clip-relative (0 is the clip's first frame), so the animation travels with the clip through trims, splits and speed changes. Logo, solid and motion clips take no keyframes. A position value stays within ±10 of the clip's own box on each axis.
- property: "position" {x, y} as fractions of the clip's own box (0,0 where it sits, matching transform.x/y; a full-frame clip's box is the frame, so 0.5 is half the picture; a clip in a `frame` slot moves by its own width and height, so y:1 carries a tile down exactly its height, which is how a grid of tiles scrolls together) · "scale" a factor 0.01..20 (1 = full; on a clip in a `frame` slot it scales the tile itself, and `auto_reframe` view is what zooms the picture inside it) · "rotation" degrees · "opacity" 0..1 · "volume" 0..2 (1 = as recorded) · "crop" {l,t,r,b} insets 0..1 that mask the clip's box, the picture inside unmoved · "focus" {x, y} source fractions 0..1 (0.5,0.5 center): where the fill crop sits on the source when the media overflows its box, so a 16:9 clip on a 9:16 timeline shows its left third at x 0 and its right third at x 1; it moves the crop, never the box · "fx<n>.<param>" a number parameter of the effect at position n in the stack, counting from 0, on that effect's own scale ("fx0.radius" racks a blur in, "fx1.amount" brings grain up); colors, switches and choices do not animate.
- interp on the left key eases the segment: "smooth" (default) eases out of one key and into the next, so every key is a stop; "linear" keeps speed through a key; "hold" jumps at the next key. Before the first key and after the last the edge value holds.
- Fades multiply on top and never touch keys; setting a static value on an animated property clears its track. Video, image and text clips animate the picture properties; video and image animate focus; video and audio animate volume; shape and nested clips animate position, scale, rotation and opacity; an adjustment clip animates opacity, its strength.
- A detailed get_timeline returns every track under "keyframes", a clip's own on the clip and an effect's inside its stack entry, in this shape.
{type:"cameraMove", clipId | trackId, move}: a camera move on a video, image or nested clip, or with trackId on every picture clip of that track, written as its scale, position and rotation keys, timed to the clip's length and always covering the frame; replaces those tracks; "none" takes it off. Moves: zoom-in and zoom-out (steady, over the whole clip) · pan-right, pan-left (steady) · slow-push (eases into a slow push, still moving at the cut) · slow-pull (a steady pull back that rests on the last frame) · reveal (tight on the middle, a fast pull back with a hard slow-down, then settles) · punch-in (a closer framing held for the whole clip) · snap-zoom (a quarter-second zoom that lands hard and holds) · handheld (a gentle sway for the whole clip) · impact (a hard shake in the first half second that dies away) · drift (a slow diagonal drift, for stills and b-roll) · rise (the view rises slowly up the picture) · roll-in (turns level and eases closer in the first second, then holds).

# Tracks
{type:"addTrack", track:{kind:"visual"|"audio", index?, id?, clips?, muted?, hidden?, locked?, height?}}: index is its place in its own stack, after the last of its kind when left out; one of its kind already there moves up one, with those above; the id is minted unless given; clips are checked as addClip checks a clip.
{type:"removeTrack", trackId}
{type:"reorderTrack", trackId, index}: the same push.
{type:"updateTrack", trackId, patch:{muted?, hidden?, locked?, height?}}: locked: nothing on it moves, trims, slips, splits, is added or removed, and a ripple leaves its shape, until locked:false. height is the editor lane's px, a whole number from 28 to 200 (null resets it), display only.

# Markers
{type:"addMarker", marker:{from, label, kind:"chapter"|"note", body?, durationInFrames?, id?}}: the id is minted unless given. note: a mark the cut keeps at a frame (a beat, a select, a place named for what happens there), body the line about it; it carries no state and nothing waits on it (asking somebody for a change is manage_comments). An unlabeled marker is numbered in the editor. chapter: publishes as a line of the video's timestamp list, so the first sits at frame 0, there are three or more, and each runs at least ten seconds; nothing checks these when a marker is written, so check them yourself. A marker body takes at most 4000 characters.
{type:"updateMarker", markerId, patch:{from?, label?, kind?, body?, durationInFrames?}}
{type:"removeMarker", markerId}

# Document settings
{type:"setCaptionSettings", settings}
{type:"setAspectRatio", ratio:"16:9"|"9:16"|"21:9"|"9:21"|"1:1"|"4:3"|"3:4"|"3:2"|"2:3"|"5:4"|"4:5"}: sets the canvas ratio and render dimensions together; independent of any generation aspect ratio.
{type:"setDimensions", width, height}: each side rounded down to an even number of pixels, 2 to 7680, larger values capped without an error; the stored aspect ratio is not updated with it.
{type:"setFrameRate", fps:24|25|30|50|60}: re-times the whole document so every clip stays at its second; frame numbers in later calls count at the new rate.
{type:"setMeta", patch}: platforms (where it goes, by platform name: Instagram, TikTok, YouTube, X, LinkedIn, Snapchat, Facebook, Threads, Pinterest, the ad platforms (Meta Ads, TikTok Ads, LinkedIn Ads, Google Ads, Pinterest Ads, X Ads, Reddit Ads, ChatGPT Ads), Twitch, Spotify, Newsletter, Website; no other name is taken; a video destination id is read the same way), drawn as marks under it in the app; note, one line shown under the timeline's tile in previews and published clip sets (on a clip from cut_clips, why this moment). The timeline's name is manage_timelines rename.

# updateClip
{type:"updateClip", clipId, patch}: null clears a field. trackId in place of clipId patches every clip on that track (volume 0 on a whole lane); sourceCode and values go by clipId. from, durationInFrames, trimStart and keyframes are ignored in a patch (moveClip, trimClip, slipClip, splitClip, setKeyframes), and speed is refused (setSpeed).
- volume 0..2 · opacity 0..1 · fadeInFrames, fadeOutFrames · objectFit "cover" (default, fills and crops) | "contain" (fits whole, with bars) · color (solid, shape and text) · width and position (logo) · shape, strokeColor, strokeWidth, cornerRadius (shape) · label · category ("voiceover"|"music"|"sound-effect": an audio clip's kind, which names its lane and decides ducking; an item brought in with no word lands as voiceover).
- frame {x, y, w, h}: the layout slot, top-left and size as fractions of the frame (split screen, picture in picture, grid; apply_layout writes it); null fills the frame. Video and image clips (other clips accept it without effect).
- holdFrames: each drawn frame held this many timeline frames, 2 for "on twos", 3 for threes, up to 6, 1 or null for none; keyframes and moving effects step with it; video, image and nested clips only.
- transform {scale, x, y, rotation, crop, focus, flipH, flipV, radius}: static values; scale, x, y, rotation, crop and focus are what a setKeyframes track animates; flipH/flipV mirror; radius rounds the clip's box in composition pixels.
- placeholder {hint?: what belongs in this slot, wants?: "video"|"photo"}: marks a video, image or title clip a hole in a template, its media or words the author's sample; null clears it, and a fill clears it too.
- transitionIn / transitionOut {style, durationInFrames, direction?}: video and image clips only (on any other clip it is refused, so fade that clip instead); drawn at the cut with the neighboring clip on the same lane, centered on it; the arriving clip's transitionIn wins over the leaving clip's transitionOut. style: "fade" (dips through black) | "dipwhite" | "dissolve" | "wipe" | "push" | "slide" | "iris" | "clock" | "zoom" | "blur" | "whip" (a push that smears, in a direction) | "spin"; all but fade draw the arriving clip over the leaving one from each side's own frames past the cut, or its edge frame held where the source has none. durationInFrames: about half a second (12 to 15 at 30 fps) is the even default. direction "left"|"right"|"up"|"down" for wipe, push and slide.

## grade (replaced whole: send it back whole with the change in it; video, image and adjustment clips only)
- preset?: a filter id (get_templates kind filter) · intensity? 0..1
- exposure?, brightness?, contrast?, saturation?, temperature?, tint?, highlights?, shadows?: each -100..100. exposure and brightness are one multiplier on pixel values and add: 100 doubles (about a stop), both at 100 triple. contrast and saturation scale by 1 + n/100. temperature above 0 mixes in sepia (40% at 100); below 0 it turns hue 0.18° a unit, and tint 0.15° a unit (positive: red toward yellow, blue toward violet). Neither moves a gray, so a cast comes off with the wheels. highlights and shadows add up to ±0.35 at the bright or dark end, fading out by 35% and 65% brightness.
- lift?, gamma?, gain?: each [r, g, b] -100..100 per channel (the three wheels: shadows, mids, highlights; 0 untouched). gain 100 scales a channel by 1.5, lift 100 adds 0.25 at black, gamma 100 raises 20% gray to 34%. inspect_color correction.grade is the exposure, gain and gamma that correct a still.
- lut?: {url, itemId?, name?, intensity? 0..1}, a .cube file kept as a library document, applied last

## effects (the ordered stack, replaced whole; applied in order after the grade; video, image and adjustment clips only)
[{type, params?, disabled?, maskId?}]: a param left out takes its default; maskId confines the effect to one of the clip's masks (not shake, which moves the whole picture; halftone and colorkey not to a person or matte region unless it is inverted).
- vignette: amount 0..1, radius 0..1, feather 0..1, roundness 0..1, color
- glow (bloom on the bright parts): radius 0..200, intensity 0..4, threshold 0..1, color
- duotone (two colors, shadows and lights): darkColor, lightColor, threshold 0..1
- invert: amount 0..1
- thermal (heat-camera ramp over luminance): amount 0..1
- blur (gaussian): radius 0..100, horizontal, vertical
- zoomblur (radial streaks from a point): amount 0..200, centerX 0..1, centerY 0..1
- chromatic (channels pulled apart): amount 0..60, angle 0..360
- fisheye: fieldOfView 0.5..3.14, radius 0.1..2, zoom 0.25..4
- mirror (one half onto the other): direction horizontal|vertical, position 0..1, invert
- wave (ripple moving with the clip): amplitude 0..200, wavelength 10..1000, speed 0..5, direction horizontal|vertical
- pixelate: blockSize 1..200
- halftone (print dots in the picture's own colors): dotSize 1..200, dotSpacing 1..200, rotation 0..360, shape circle|square|line
- emboss (relief): amount 0..1, size 1..200, depth 0..1, lightAngle 0..360
- noise (film grain, a fresh pattern every frame): amount 0..1, premultiply
- scanlines: amount 0..1, spacing 1..40, thickness 1..20, speed 0..200
- lightleak (a warm leak that evolves across the clip): seed 0..100, hueShift 0..360
- shake (handheld jitter, the frame kept covered): intensity 0..1
- colorkey (one color see-through, the track below shows): keyColor, similarity 0..1, smoothness 0..1, spillSuppression 0..1. A pixel's distance from keyColor is the straight line between the two in RGB, channels 0..1, divided by √3 (black to white is 1). Under similarity − smoothness it is removed, over similarity + smoothness kept, part transparent between. spillSuppression pulls the key's channel toward the mean of the other two on what is kept. inspect_color key_color returns the distances a frame holds and key.suggested {similarity, smoothness}.

## masks (replaced whole; video and image clips only, an adjustment clip draws no picture of its own to shape)
[{id, shape:"ellipse"|"rect"|"linear"|"person"|"matte"|"object", x, y, w, h, angle?, feather?, invert?, grade?}]
- x/y is the center and w/h the size, fractions of the clip's box; angle degrees; feather a fraction of the shorter side, softening every edge of the shape, one that lies on the clip's own edge included (a shape that overhangs that edge by the feather keeps it hard); invert keeps the outside.
- "linear": a straight edge through x,y keeping what the angle faces; w and h are still required.
- "object": a region grown from seed points on what to keep, read through the desktop app only.
- "person": no geometry; the region is the transparency of a cut-out of this clip's own footage, found for the people near the camera once written on a clip already on the timeline (invert:true treats the background once a grade or an effect uses the mask; alone it draws the people either way). The cut-out is made and the mask draws once it lands (the response says the region is being read); cloud processing; a small figure or nobody in shot is refused with a sentence.
- "matte": the paid cutout, made by remove_background with as:"mask", for a small figure, a product or anything with glass.
- grade on a mask: a color grade for that region alone, the same shape as the clip's, and the only way to confine exposure, saturation or a LUT to part of the picture.
- A mask no effect names shapes the clip: only what is inside it is drawn (a spotlight, a porthole, a reveal). A mask an effect names by maskId confines that effect alone (one face blurred, one corner graded, one part keyed). Each mask an effect names costs the clip another draw of its media.

## text clips
content · fontSize · fontFamily and fontWeight (a face the renderer lacks is refused with the ones it has; a preset's weight on a family that lacks it is set to that family's nearest, with a note) · color · backgroundColor, backgroundOpacity, backgroundPadding, backgroundBorderRadius · position ("top-left"|"top-center"|"top-right"|"center-left"|"center"|"center-right"|"bottom-left"|"bottom-center"|"bottom-right"): the words sit in a block 80% of the frame wide (60% portrait, 70% square, 85% past 2:1) and wrap at that width. left, right, top and bottom hold the block 6.5% of the frame's short side from that edge (10% portrait, 5% square); on 9:16 and taller, top is 12% of the height down and bottom 24% up, clear of a platform's controls, so bottom text ends at 0.76 of the height. center is the middle · textAlign ("left"|"center"|"right") · offsetX/Y (composition pixels from where position holds the block, not the frame's edge: top on 1080×1920 is 230 px down, so offsetY 100 starts the block at 330. Positive is right and down from a left, top or center position, negative back toward or past that edge; from a right or bottom position the offset's size moves the text away from that edge whatever its sign; a preset's offsets apply at its own position only) · animation ("none"|"fade"|"slide"|"pop", the entrance) · stylePreset ("none"|"readable"|"bold"|"boxed") · strokeEnabled, strokeColor, strokeWidth · shadowEnabled, shadowColor, shadowBlur · fontStyle ("italic") · letterSpacing (a fraction of the font size) · lineHeight (a multiple, 1.2 unset) · textTransform ("uppercase"|"lowercase"|"capitalize") · textDecoration ("underline"|"line-through") · tiltX/tiltY (degrees of lean in depth).
