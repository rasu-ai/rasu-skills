# Generating: credits, jobs and takes

Every generation spends the team's credits. The person decides what to spend;
your job is to make the decision easy and never make it for them.

## Which tool

- A shot's video take: `generate (type: seq_shot_generate)`.
- A look of a character, place or prop: `generate (type: asset_variant_generate)`.
- A shot's storyboard sheet: `generate (type: storyboard_generate)`.
  A sheet guides the shot's video only once it is starred, and the person
  decides that: star one with `shot_manage (type: storyboard_star_set)` only
  when they ask.
- A shot's blocking still, its opening frame drawn as one picture:
  `generate (type: seq_shot_still_generate)`. A drawn still is a candidate.
  The shot's clip opens on it only once it is approved, and the person decides
  that: approve one with `shot_manage (type: seq_shot_still_approve)`, or take
  the approval off with `shot_manage (type: seq_shot_still_clear)`, only when
  they ask.
- A picture or clip with no Rasu AI record behind it (a one-off image, a
  photo restyled, variations, a reference picture):
  `generate_raw (type: gen_image_generate)` or
  `generate_raw (type: gen_video_generate)`, then
  `media_show` with the jobIds to show them in Rasu AI's panel.

Inside a project, prefer the record-backed tools. They compile the prompt from
everything the project already knows, so the result stays consistent with the
rest of the film. The raw tools are for what belongs to no record.

## Quote, ask, generate

1. **Estimate.** `preview (type: seq_shot_estimate)` for a shot,
   `preview (type: asset_variant_estimate)` for an asset look,
   `preview (type: storyboard_estimate)` for a storyboard sheet,
   `preview (type: seq_shot_still_estimate)` for a blocking still.
2. **Read `blocked` first.** If it is true, nothing would generate: `missing`
   lists each thing the shot still lacks and `refusal` says why when it is
   the model, the resolution or the shot's length. Fix those (or tell the
   person what is needed) before quoting a price. A blocked estimate costs 0.
3. **Ask in one sentence.** "Shot 4 as an 8-second video on Seedance 2.0 Fast
   will cost 120 credits, leaving 880. Go ahead?" Name the model when the
   person might care. Wait for a clear yes.
4. **Generate with the receipt.** Pass the estimate's `compileReceipt`
   unchanged. If anything about the shot changed since the quote, the call is
   refused before any credits move: estimate again and re-ask if the price
   changed.

Some clients also show their own confirmation before a spend goes through.
That does not replace your quote: the person should hear the price from you.

If the person turned on Generate without asking (Settings, Model defaults),
the server tells you so when you connect. Then generate without quoting or
waiting, and do not mention cost unless they ask. Never tell anyone to turn
that setting on to save time; it is their choice.

For several shots at once, quote the total and list what each costs, then ask
once.

## After you press generate

- The answer is a `jobId`, not a finished picture. Read it with
  `generation_read (type: gen_job_get)` and `waitMs` 6000: the read answers
  when the job ends or the wait runs out. Read again while it is still
  queued or running, until it is done, failed or cancelled. Most jobs land
  within minutes.
- When it is done, find the result: `generation_read (type: gen_takes_list)`
  for a shot's takes, or the asset read for a look. Show it to the person.
- Picking which take goes in the cut is the person's call:
  `shot_manage (type: gen_take_cut_set)` once they choose.
- **Never press generate twice for the same thing.** If an answer was lost or
  a call timed out, look first with `generation_read (type: gen_jobs_list)`.
  A second submit for a shot already in flight returns the same job without a
  second charge, but a different request does charge again.
- A failed job refunds its credits. Retrying it with
  `generate (type: gen_job_retry)` pays again, so ask before retrying.
- `engine (type: engine_verify)` asks the specialists to verify a finished
  take against its shot. Use it before calling a take good.

## Models and settings

`generation_read (type: gen_models_list)` lists the models, resolutions,
durations and reference limits. Leave the model out to use the person's own
default. Only change model or resolution when the person asks, or when the
estimate says the current one cannot run the shot.

## Face changes and neck review

Every face change starts with a fresh whole-person still from the chosen character
sheet, then image-to-video opening on that still. Clip-based swaps are for things,
wardrobe and props. A reference-clip face change can leave the new head sitting on
the old neck; disclose that risk before spending and offer Face change. Raw calls
remain one caller-authored request: face-changing words do not select composition.

Choose the explicit Face change workflow with `preview (type: face_change_estimate)`
and `generate (type: face_change_start)` before sending; keep its retry key for
replay. Read progress with `generation_read (type: face_change_get)` and cancel
with `generation_manage (type: face_change_cancel)`. Show the
selected sheet, preparatory still, dependent video, actual reference roles,
settings and both prices, then ask for consent. Never send the old clip as the
final video's picture or edit input. For a shot, request a fresh blocking-still
candidate even when an older still exists, and wait for the person's approval;
never approve it yourself. Quote again when the approved plan moves; accept a current moved-stage quote with
`generate (type: face_change_consent_set)` only after renewed consent. For recipes,
read the recipe's own instructions and disclosures before starting.

After a face-change take lands, inspect the clip itself to find the frame where
the face is largest. Use the original-download/client inspection path available
through the media read, then review 3–5 selected frames including that frame.
Six uniform contact-sheet thumbnails alone do not establish this review. In
**each** reviewed frame require one continuous neck from jaw to collar: no edge
or colour break under the jaw, a neck wide enough for the head, and the collar
sitting on that neck. Report this neck judgement separately from the face-match
score: a high face score does not pass the join. If you cannot inspect the clip
and selected frames, report the neck review as unperformed. No server neck check,
automatic seam mark or pre-cut guarantee is supplied by this workflow.
