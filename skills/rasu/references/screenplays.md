# Screenplays

Screenplays live on Rasu AI's story side: projects, episodes and a ladder
of documents from one-line idea to finished script. The server serves the whole
method as the resource `rasu://workflow/screenplay`, and as the
`develop_screenplay` prompt. **Read the resource before your first write in a
session.** It is the source of truth; this page is the judgment around it.

## Start by reading, never by writing

1. `story_project (type: project.list)` to find the writer's project, or
   `story_project (type: project.create)` only once they ask for a new one.
2. `story_context_brief` shows which rungs exist, which are empty and every
   open review. It is the cheapest way to learn what is already written.
3. Ask for the idea and the audience if they are missing. Continue the
   writer's existing work; never replace it with your own.

## The ladder

Develop in order: one line, pitch, topic analysis, characters, story outline,
episode outline, scene outline, then script. Agree the premise, the character
arcs and the scene outline with the writer before drafting full scenes. Use
the writer's chosen method; the method guides are listed as resources (for
example the Braintrust framework for a feature).

## Every save, the safe way

A save replaces the **whole** document body with a new version. It never
appends. So:

1. Read the document's current rev and head first
   (`story_document_read` for a range of a long script).
2. Call `story_prepare` for that exact document and story point, and use the
   context it returns.
3. Save with `story_write` (`document.version.append`) with that rev and the
   `contextReceipt` from the prepare call.
4. Keep every scene that was already there. Never shorten the text to fit a
   limit; if it is too long, say so.

If the project changed since you prepared, prepare again. On a revision
conflict, read the head, reconcile the writer's changes into yours, and only
then retry. Never reuse a stale receipt.

## Characters, places and structure

- Register characters, locations and props with `story_world_register`,
  including aliases, and record how characters stand toward each other. Those
  relationships are what the shot specialists later hand the director.
- Keep the story's spine (`story_spine`) grounded in the written scenes.
- After saving, anchor reveals, plants and payoffs with `story_annotate`, and
  run `story_check_run` after revisions.

A quiet check is not proof the screenplay is good. Before calling a draft
done, read back the saved ending and walk every planned scene and payoff.

## Handing on

`story_review_open` pins a version for review; `story_package` exports the
saved script. Neither approves or releases it: release is the writer's own
action in the browser. When the script is ready for production, the video side
can import it as sequences; preview that import with
`studio_read (type: video_story_package_preview)` and show the person what will
change before importing.
