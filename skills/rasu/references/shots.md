# The shot method, in detail

Rasu AI has specialists (the director, cinematographer, script supervisor,
casting and others) that know the project, its World and the craft standards
for each part of a shot. You consult them before you write, the server records
that you did, and after the write they check the result. A shot written
without consulting is refused, so this is not optional; done well, it is also
what makes shots cut together.

## Before the first shot

1. **Story form.** `studio_read (type: project_get)` and read `form`. If it is
   empty, ask the person whether this is a feature film, an episode, a short or
   an ad, then save it as `fields.form` with
   `project_write (type: project_update)`. Some checks stay blocked until the
   form is known, so do this first rather than after a failed check.
2. **The look and physics.** Call `engine (type: engine_consult)` with domain
   video, concept project and the project's id, then write the project's look
   and physics once with `project_write (type: project_style_update)`: how the
   film looks, and what has weight, what a body does on contact, how long a
   fall lasts. Every shot shares them, so never write them into a shot. While
   either is unwritten, every shot's compile warns.
3. **The sequence.** Find or create it (`sequence_read (type: seq_sequences_list)`,
   `sequence_write (type: seq_sequence_create)`). While it has no shots, call
   `engine (type: engine_consult)` with domain video, concept sequence and the
   sequence's id. Its relationships tell you how the characters in this scene
   stand toward each other, which shapes how the scene opens.

## Each shot

1. **Create it bare.** `seq_shot_create` with the shot number you choose and
   one line of intent: what the shot is for ("Mara realises the key is gone"),
   not what it looks like. A shot is addressed afterwards by its sequence and
   number; its id for the specialists is `<sequenceId>-<n>`.
2. **See who must be consulted.** `engine (type: engine_inspect)` with domain
   video, concept shot and the shot's id names the roles an update needs
   evidence from, and any check blocked on the story form.
3. **Consult once.** Call `engine (type: engine_consult)` with domain video,
   concept shot and the shot's id, and no focus. That one reply proves every
   update to the shot, whichever fields you write. If you consult again later,
   send `held` with the versions earlier replies named: only what changed
   comes back whole.
4. **Read the relationships before the beats.** The shot consult carries how
   the characters in frame stand toward each other; they are never a focus of
   their own. Play that in what each one does and says. Never write the
   relationship itself into the shot ("his rival"); show it ("he doesn't
   offer his hand").
5. **Write once, with everything.** `seq_shot_update` with the shot's current
   `rev`, only the fields you change, and every consult's `evidenceId` in
   `evidenceIds`. If the update is refused for consultation evidence, the
   refusal names the roles still missing: consult those and send again.
6. **Judge it yourself.** Read your write against the standards the consults
   gave. The server checks structure and rules; it does not decide whether the
   shot is good. That is your job, and in the end the person's.
7. **Check it.** A shot write that made checks due answers with them: its
   `checks` is the shot's `engine (type: engine_check)` answer, read after
   waiting for them. If it still lists pending checks, call
   `engine (type: engine_check)` on the shot with `waitMs` 10000: empty
   findings with pending checks is not a clean result. A reply whose
   `verification` names a `waitMs` carries no checks: make that call yourself.
8. **Answer every finding.** Either fix it with another `seq_shot_update`
   (passing the check's `evidenceId` as `exposureId`), or dismiss it with
   `findings (type: story_finding_dismiss)` and a real reason the person
   would accept. "Not important" is not a reason.

## Craft notes that save rework

- **Length comes from the beats.** A shot has no duration field; it lasts as
  long as its beats add up to, and that must fit the video model it will run
  on. `sequence_read (type: storyboard_context_get)` shows the length the
  beats come to.
- **Dialogue lives in the beat it is spoken in**, with who says it and how.
- **Name assets with `@`** (`@Mara`) in any prose, and only after the asset is
  in the shot's assets list. Characters, places and props are referenced by
  asset id in `assets`, never by typing a name.
- **Continuity carries forward.** What a body or prop carries out of a shot
  (torn sleeve, wet hair, the key in her left hand) goes in the shot's
  continuity, spelled as the asset is named, so the next shot opens on it.
- **Light a sequence once**, on the sequence. A shot writes its own lighting
  only where it differs.
- **Before generating**, `preview (type: seq_shot_compile)` shows the prompt
  Rasu AI will send and whether anything blocks it. Show the person the
  plan in plain words if they want to review it before spending.

## Working on many shots

Direct shots in order, one at a time: consult, write, check, answer findings,
then move on. A later shot's continuity reads the earlier one's end state, so
batching all the writes first and checking later produces drift you then have
to unwind. Keep the person posted in a line per shot ("Shot 3 written; the
cinematographer flagged the lens, fixed"), not a wall of JSON.
