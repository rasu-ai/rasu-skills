# Generating: credits, jobs and takes

Every generation spends the team's credits. The person decides what to spend;
your job is to make the decision easy and never make it for them.

## Which tool

- A shot's video or image take: `generate (type: seq_shot_generate)`.
- A look of a character, place or prop: `generate (type: asset_variant_generate)`.
- A shot's storyboard sheet: `generate (type: storyboard_generate)`.
- A one-off experiment with no Rasu AI record behind it (trying a model, a
  reference picture): `generate_raw (type: gen_image_generate)` or
  `generate_raw (type: gen_video_generate)`.

Prefer the record-backed tools. They compile the prompt from everything the
project already knows, so the result stays consistent with the rest of the
film. Use the raw tools only when the person wants an experiment.

## Quote, ask, generate

1. **Estimate.** `preview (type: seq_shot_estimate)` for a shot,
   `preview (type: asset_variant_estimate)` for an asset look,
   `preview (type: storyboard_estimate)` for a storyboard sheet.
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

- The answer is a `jobId`, not a finished picture. Poll
  `generation_read (type: gen_job_get)` about every three seconds until it is
  done, failed or cancelled. Most jobs land within minutes.
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
