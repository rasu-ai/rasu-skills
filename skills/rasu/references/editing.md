# Editing: assembling and finishing a cut

This is how an experienced short-form editor builds and finishes a sequence's
cut, held to what Rasu AI can do today. The numbers are where to start; the
person's eye and ear overrule them.

## 1. Say what the download holds before you promise a finish

The download is one .mp4 of the storyline. Tell the person so, in plain words.

**In the file today:**

- The storyline's clips in order, as trimmed, sped up, slowed or reversed, at
  up to 1080p and 30 frames a second. A gap, or a shot with no take, is black
  and silent.
- Each storyline clip's own sound at its level, with its fades, under its
  role's and the master's fader and mute.
- A basic grade on each clip: exposure, contrast, saturation, a warm or cool
  tint and a vignette (lift, gamma and gain fold into brightness and
  contrast). The sequence grade's exposure and contrast sit under every clip.
- Captions, drawn into the picture. Where the server's encoder cannot draw
  text they are a subtitle track a player switches on, and the job says so.

**Not in the file yet:**

- Any other row: a picture above the storyline, a music or sound-effect row,
  sound detached from its picture, an adjustment clip.
- Transitions: every one is a straight cut.
- A LUT, curves, and a saved Look applied to a clip, which counts as a LUT.
- Crop, scale, position, rotation, opacity, masks and keys. Nothing can be
  keyframed, so there is no punch-in and no slow push.
- A hold (the viewer freezes the frame, the file plays on), split edits, pan,
  frame blending, and a clip's equaliser, compressor, limiter, de-esser,
  reverb or duck.
- Loudness: the cut stores a target and a peak ceiling; nothing applies either.

**What you cannot perceive.** Rasu AI has no transcript, no silence or beat
detection and no loudness reading. `asset_read (type: media_look)` shows a clip
as six frames marked with their times, and nothing of its sound. So you cannot
hear. Where a word starts, which take is cleanest, where the beat falls: ask
the person for the times, or a transcript with timings made elsewhere, then
ask them to play the result and confirm it. If your own environment can analyse
a file, `asset_read (type: media_download)` gives you the original; say those
readings are yours, and still ask the person to listen.

## 2. Name the edit before you cut

Which of these is it? When it is unclear, ask; never pick.

- **Talking head:** one person to camera, cut tight on the words.
- **Story or promo:** shots that tell a story or sell one thing.
- **Speaker-led recap:** sound bites carry it, pictures cover them.
- **Montage-led recap:** music carries it; no speech holds it together.
- **Beat-cut montage:** every cut sits on the music's grid.
- **Clips from a long talk:** several short pieces, one idea each.
- **Captions only:** a finished cut gains captions and nothing else moves.

Then say what that kind needs that the file cannot hold: pictures over a
speaker's voice need a second picture row or a split edit, and a montage needs
its music row. Cut to the times the person gives you; the missing layer is
added outside Rasu AI for now.

## 3. Build it in this order

1. Read the cut with `sequence_read (type: seq_cut_get)`. Every cut write
   carries the cut's `docVersion`: take it from that read, then from each
   write's answer. On a conflict, read again and redo your change.
2. Put the footage on the storyline: every picked take at once with
   `cut_edit (type: cut_picks_place)`, or one clip, an uploaded file included,
   with `cut_edit (type: cut_lane_clip_add)`. Which take a shot plays is the
   person's call; `cut_edit (type: cut_clip_take_replace)` tries another in one
   slot.
3. Trim and order with `cut_edit (type: cut_clip_update)`, blade with
   `cut_edit (type: cut_clip_split)`, and take a stretch out and close the gap
   with `cut_edit (type: cut_range_edit)`. Times are seconds. The file is cut on
   whole frames: one frame is about 0.04 s at 24 or 25 frames a second.
4. Set sound, then colour, then captions last: closing a gap on the storyline
   moves no caption, so an early caption ends up over the wrong words.
5. Save a version, then download and check (section 12).

## 4. Structure

- **Hook inside the first 2 to 3 seconds**, on the most arresting line that is
  true. A deliberate opener the speaker recorded wins over your choice.
- **Keep every distinct supporting line.** Cut repeats, never substance.
- **Hold about 0.45 s after the last word** before the ending, so it lands.

## 5. Cutting speech

You need each word's times from the person (section 1). With them:

- **Take out pauses over about 0.10 s inside a thought.** Never eat a word.
- **Leave about 0.04 s before a word and 0.06 s after it:** tails run long.
- **A piece under about 0.3 s reads as a glitch.** Join it or drop it.
- **Fade both sides of every voice cut, about 5 frames,** since a hard audio
  cut clicks. A clip's `fadeIn` and `fadeOut` hold tenths of a second, so 0.2 s.
- **Of a line said twice, keep only the cleanest complete take.** Ask which.

## 6. Pacing

- **Talking head:** something changes on screen every 2 to 2.5 s, the longest
  stretch with nothing new is about 6 s, and an average shot is about 3 s. The
  changes you have today are a cut to another take or angle and a new caption.
- **Montage:** a median shot of about 0.6 s. Shots last 1, 1.5 or 2 beats,
  with short flurries of half-beat cuts after a drop and runs that speed up
  into one. A beat is 60 divided by the tempo, in seconds: ask the tempo and
  where the drop falls.

## 7. Montage grammar

- **Use every shot once.** Check the cut for a take or file placed twice.
- **Start a moving shot already moving:** trim in past the start of the move.
- **Order a run by direction of movement,** and cut inside the blur of a whip.
- **Uneven shot lengths, on purpose.** Equal lengths read as a slideshow.
- **Land the music's drop on a picture frame you chose,** and keep the last
  spoken word at least 0.5 s clear of it.
- **No zoom, text or slow motion until the person asks.**

## 8. Captions

- **Short phrases, 2 to 5 words,** one caption each: add it on the captions row
  with `cut_edit (type: cut_lane_clip_add)`, its words as the label, and change
  the words with `cut_finish (type: cut_caption_update)`.
- **A caption never straddles a cut.** It starts and ends inside one clip.
- **One typeface per video.** The cut has one caption style, set with
  `cut_finish (type: cut_caption_track_update)`. Never override the typeface on
  a single caption.
- **Everything inside the safe area.** Leave the style's `title.safe` on title
  safe, 10 percent of the frame, its default. Rasu AI wraps a caption at its
  spaces between those margins; keep it to two rows of about 40 characters.
- The studio advises against a caption under a second or over 20 characters a
  second. Phrase captions often trip the first; nothing is refused.

Not possible yet, so say so: timing to each word (you place every caption by
hand from the person's times), an emphasised word (a caption has one style;
capitals are the only emphasis), motion, and a second language.

## 9. Sound

- **The targets:** the voice clearly above any music, by about 8 LU; music as
  one instrumental pass, never looped; the finished piece at about -14 LUFS,
  with true peak at or under -1 dBTP.
- **You can set** each clip's level and fades with
  `cut_edit (type: cut_clip_update)`, and each role's and the master's fader
  and mute with `cut_manage (type: cut_update)`. Say nobody has heard them.
- **You cannot** measure LU, LUFS or true peak, normalise the file, duck music
  under speech or put a music row in the file. Tell the person all three
  targets have to be measured and met outside Rasu AI for now.

## 10. Colour

- **One look across the whole cut:** the same values on every storyline clip,
  or the sequence grade for exposure and contrast. A saved Look applied to a
  clip is not in the file (section 1).
- **For social, bright and natural:** a little saturation, about 12 percent
  (`basicSaturation` 1.12), and no skin smoothing.
- **Fix a colour cast before styling.** Only a warm or cool tint reaches the
  file; a green or magenta cast cannot be fixed here, so say so.
- You see six frames of a clip: ask the person to judge the colour.

## 11. Versions and notes

- **Save a version before a round of notes,** named for the round, with
  `cut_manage (type: cut_version_create)`.
- **Never overwrite a version someone commented on.** Change the working cut
  and save a new version; the one they saw stays as it was. A restore with
  `cut_manage (type: cut_version_restore)` replaces the working cut: save first.
- **Answer every note in one line:** done, done differently and how, or not
  possible and why. Tick a note left as a marker with
  `cut_manage (type: cut_marker_update)`, and show what changed with
  `sequence_read (type: seq_cut_version_diff_get)`.
- **Say what you could not see or hear.**

## 12. Download and check

`cut_finish (type: cut_download)` is free and changes nothing. Read its job
with `generation_read (type: gen_job_get)` and `waitMs` until it is done. The
finished job lists what the file left out as warnings: relay every one. Look
at the file with `asset_read (type: media_look)`, hand it over with
`asset_read (type: media_download)`, and ask the person to watch it with the
sound on before anyone calls it finished.
