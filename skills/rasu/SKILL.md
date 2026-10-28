---
name: rasu
description: >
  Work in Rasu AI, the studio that turns stories into video, through its MCP
  server (named rasu). Use whenever the person mentions Rasu AI, one of
  their Rasu AI projects, sequences, shots, characters, Worlds, cuts or
  scripts, or asks to plan, direct, storyboard, write, generate or fix
  anything there. Covers finding the right project first, the shot method
  (consult the specialists, write, then check), keeping a project's script,
  quoting credits and waiting for a yes before any generation, and recovering
  from refusals and missing tools instead of retrying blindly.
---

# Working in Rasu AI

Rasu AI turns stories into video. The person you are helping is a filmmaker,
writer or producer, not an engineer. They talk about "the diner scene", "Mara"
or "shot three"; you turn that into the right calls. The Rasu AI MCP server
sends its own short instructions when you connect. This skill is the judgment
around them: what to do first, in what order, and what to do when something
goes wrong.

## Five rules that hold everywhere

1. **Plain words, always.** Never ask the person for an id, a rev, a tool name
   or a parameter. Look it up by name. If two things share a name, describe
   them ("the Black Queen project in the Chess World, or the one on its own?").
2. **Read before you write.** Every write takes the record's current `rev`
   from a fresh read. Never guess one, never reuse one from ten minutes ago.
3. **Never spend credits without a yes** (unless the person turned on
   Generate without asking; see [generating](references/generating.md)).
4. **A refusal is an answer, not noise.** It says why. Fix that cause, then
   try once more. Never loop the same call.
5. **Never decide the story for them.** Story form, tone, who a character is:
   ask when it is not written down. Suggest, then wait.

## First call: find where you are

Before anything else, work out which project the person means.

- A pasted handle such as `shot:opening-m2p8q1z0aa-3` or `asset:mara-7hk2l0x9qe`:
  open it with `studio_read (type: handle_get)`. It tells you what it is and
  which ids to use next. Do this before searching by name.
- A project named in words: `studio_read (type: studio_projects_list)`, match
  by name, then `studio_read (type: project_get)`. Its `form` field is the
  story form (feature, episode, short or ad), which the shot method needs.
- Nothing named: list the projects and ask which one, in a single short
  question. An empty list is not an error; offer to start one with
  `project_write (type: project_create)` once they want it.
- A script belongs to its video project: `studio_read (type: document_list)`
  shows the project's script documents. See [Scripts](#scripts).

Then read what you are about to touch: `sequence_read (type: seq_sequences_list)`
and `sequence_read (type: seq_shots_list)` for sequences and shots,
`asset_read (type: asset_list)` for characters, places and props.

If the person belongs to more than one team, some tools also take a team; the
server says so in its answer. Ask which team only when it matters and they
have not said.

## The shot method

Whenever you plan, direct, create or improve shots, follow the method the
server describes, every time, without being asked. In short:

1. Know the story form. If the project has none, ask whether it is a feature
   film, an episode, a short or an ad, and save the answer as `fields.form`
   with `project_write (type: project_update)`. Never pick one yourself.
2. Before any shot, consult on the project and write its look and physics
   once with `project_write (type: project_style_update)`: every shot shares
   them, so never write them per shot. Before the first shot of a sequence,
   consult on the sequence itself.
3. Create each shot with `seq_shot_create`: number and intent only.
4. Consult on the shot once for each part you will write, then write it with
   `seq_shot_update`, passing every consult's `evidenceId` in `evidenceIds`.
5. Judge your own work against what the consults said. The server gives
   context and standards; it does not grade taste.
6. A shot write's reply carries the shot's checks. Answer each finding, and
   call `engine (type: engine_check)` on the shot again while any is pending.

The details that make this go well, including what each consult focus is for
and how to handle findings, are in [the shot method](references/shots.md).
Read it before you direct your first shot in a session.

## Generating costs money

Generation spends the team's credits. Before any call to `generate` or
`generate_raw`:

1. Quote it. For a shot, `preview (type: seq_shot_estimate)`; for an asset
   look, `preview (type: asset_variant_estimate)`; for a storyboard sheet,
   `preview (type: storyboard_estimate)`; for a shot's blocking still,
   `preview (type: seq_shot_still_estimate)`.
2. Tell the person the cost and what they will get, in one sentence, and wait
   for a yes.
3. Generate with the estimate's `compileReceipt`, unchanged.
4. Wait for the job and show the result.

The full flow, including a blocked estimate, polling and takes, is in
[generating](references/generating.md).

## Scripts

A video project keeps its script as a document of its own. Start one with
`project_story (type: document_create)` and kind `script`; you choose its
`documentId`. Every revision is `project_story (type: document_version_append)`
with the complete text and the document's current `rev`: earlier versions
are never rewritten, so a correction is one more append, and a version that
leaves a scene out has cut it. Before revising, ask the Screenwriter:
`engine (type: engine_consult)` with domain video, concept document and the
document's id reads the script whole. Notes and other markdown are files,
written with `project_story (type: file_write)`, never script versions.

## When something goes wrong

- **Refused:** read the reason and fix that cause. A stale `rev` means re-read
  and redo your change on the fresh record; a missing consultation means
  consult the roles it names; a blocked estimate names what is missing.
- **A tool you expect is not in your list:** it is probably switched off in
  the client, or the connection lacks a permission. Tell the person which
  tool to turn on, and where. Never say Rasu AI cannot do it.
- **Not connected at all:** the server is added separately from this skill.
  Point them to the setup steps in the
  [README](https://github.com/rasu-ai/rasu-skills#readme).

More cases, and how to explain each to the person, are in
[recovery](references/recovery.md).

## Deleting

`delete` removes records permanently, with no undo, and ignores any change made
since you read the record. Only delete what the person asked you to delete, by
name, after saying exactly what will go. Prefer archiving (most write tools
offer it) when they only want something out of the way.
