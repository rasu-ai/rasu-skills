# Character sheets of real people

A Character's sheet is the contract for every later picture and clip of that
character. Whatever is wrong in it follows the character into every shot: a face
taken from an old photo, grain, a ring on the wrong hand. This is the method that
made sheets of real people look like them and stay clean, measured over 100
generations on real people's photos. Follow it in order.

## 1. Choose the pictures first

What goes in decides likeness more than any prompt.

- **One look only.** Ask which pictures show the person as they look now (hair,
  glasses, build), unless they clearly come from one session. Pictures from
  different years blend into someone who never existed: younger, slimmer, "like a
  cousin". Such a sheet can look clean and still fail.
- **Pick, don't pile.** The sharpest frontal, neutral face first; then a
  three-quarter and a profile if they exist; one or two full-body pictures for
  build and clothes. Three good pictures from one session were enough. Two near
  duplicates add nothing.
- **Group photos:** crop the person out before you upload. A face of about
  230 × 260 pixels still worked.
- **Ask their height.** A phone photo cannot show how tall someone is, and
  height changes how the whole body is drawn. Ask "How tall is this person?"
  every time, and write the answer in the Character's build row in centimetres
  (187 cm; keep feet and inches beside it if that is how they said it) with
  `asset_write (type: asset_update)`. If they don't know, write their rough
  height in words ("about average height") rather than a guessed number. The
  character-sheet recipe asks the same question.
- Upload with `media_upload_link`, then add each picture to the Character with
  `asset_sheet (type: asset_reference_version_append)`.

Before you write the Character's rows, consult Casting:
`engine (type: engine_consult)` with domain video, concept asset, the asset's id
and focus identity. For a real person the pictures carry the face: write
identifying marks (glasses, moles, scars, a signature hairstyle), never shape
words such as "round face" or "full cheeks". Shape words overcorrected the face
every time they were tried.

## 2. Consent

A real person's likeness needs their consent. Ask the person you are helping to
confirm it before the sheet is made, in one plain question.

## 3. A master portrait from the best real photo

Before the full sheet, turn the sharpest real front picture into a clean studio
portrait: same face, new light and backdrop. This one step did more for likeness
than anything else tried. Use `generate_raw (type: gen_image_generate)` with
GPT Image 2.5 Sunburst, mode Edit, 1:1, 2K, and the photo as the only input. The
wording that kept the face untouched:

> Edit image 1 into a clean studio head-and-shoulders portrait of the same person.
> Keep their face exactly as it is in the photo: the same width and fullness of
> the face and cheeks, jaw, nose, lips, eyes, eyelids, brows, ears, moles, skin
> tone and hairstyle. Do not slim, lengthen, reshape, beautify or idealise
> anything. Change only the photography: one large soft octabox with a white fill
> bounce, a seamless neutral mid-grey backdrop, facing the camera with a neutral,
> relaxed, mouth-closed expression, head level. Clean, noise-free and low-ISO like
> a medium-format studio photo, with natural skin, no grain, no digital
> sharpening, no halos, no plastic skin. No text.

Show it beside the real photo and ask "Does this look like them?" before going
on. Then add it to the Character as its first reference with
`asset_sheet (type: asset_reference_version_append)`.

## 4. The sheet

Generate the Character's look with `generate (type: asset_variant_generate)`,
quoting it first with `preview (type: asset_variant_estimate)`. For a real
person, set the model to GPT Image 2.5 Sunburst and the resolution to 2K. It
costs more than the house default, so say so when you quote it, and offer
GPT Image 2.5 Flare for trial looks:

| Model | What happened with real people |
|---|---|
| GPT Image 2.5 Sunburst | Clean in every run, and held the face. Use it |
| GPT Image 2.5 Flare | Also clean; fine for cheap trial looks |
| GPT Image 2 | The main source of grain and over-sharpening, different every run; worst at 4K |
| Nano Banana Pro or 2 | Drifted toward a generic "AI version" of the person and added texture |

If you write or override a sheet prompt yourself, three lines carry the result:

- "Take only their identity from the references, never their lighting, poses,
  head tilts or image quality."
- A low-ISO medium-format studio photograph, soft even light, natural skin with
  faint real texture, detail resolved optically, never by digital sharpening.
- Never write "8K", "hyper-realistic", "ultra-detailed", "razor-sharp" or
  "pores": those words made the grainiest sheets, and oily skin even on 2.5.
- Say the height in words: "They are 187 cm tall: the full-body views keep the
  proportions of someone that height."

## 5. Signature items get their own pictures

A watch, ring, glasses or shoes the person always wears belong in their own
pictures, never only on the body: asked for a whole outfit, models put the watch on
the wrist, the ring on a hand and lose their exact design. Make each recurring item
a Prop asset with its own look, one object alone on grey with no hand, wrist or
person; that worked for every item tried. In the Character's wardrobe row, say
where each is worn, and never put a ring on the hand that wears the watch.

## 6. Look before you keep it

Open the result with `asset_read (type: media_look)` and check, in this order:

1. **Likeness:** beside a real, current photo, is it them? A no goes back to step
   1, never to more words about the face.
2. **Grain:** at full size, a speckled backdrop, crunchy skin or white halos along
   edges mean grain. Generate again on GPT Image 2.5 Sunburst. A sheet that is right
   but grainy was repaired by a Sunburst edit that "keeps absolutely everything
   identical and changes only the image quality: remove grain, sensor noise,
   over-sharpening and halos".
3. **Pose and text:** portraits face the camera, head level; the picture holds no
   lettering.

Grain in a sheet did not reach video: Seedance re-draws skin. A wrong face did,
in every clip. So likeness is the check that matters most.

## 7. Verification for video

Seedance checks the people in its references. A generate call on such a model
checks any unchecked picture first, free, and waits for the answer. To have the
sheet ready before the first clip, submit it once it is kept, with the person's
consent: `asset_write (type: asset_verify)`. It is free and took seconds.

## 8. Changing a sheet: a new picture, never an edit

When the person wants something on a sheet changed (taller, longer legs, other
trousers, a new haircut, another watch), never edit the sheet picture to change
it. Edits drift the parts meant to stay: in testing, an edit to the legs also
changed the untouched portraits. A sheet built by edits also can't be made again
from its references.

1. Write the change into the Character's rows with
   `asset_write (type: asset_update)`: height and proportions in the build row,
   clothes in the base wardrobe row. Say where the body lands, not only "longer
   legs": "the hem of his blazer lands at mid-thigh".
2. Quote and generate a new picture of the look from its references, as in step 4,
   with `preview (type: asset_variant_estimate)` and
   `generate (type: asset_variant_generate)`. The earlier picture stays as the
   previous version.
3. Keep the approved face. If the new picture's face drifts, crop the approved
   portraits out of the earlier sheet, upload them with `media_upload_link`, and
   add them as the first reference with
   `asset_sheet (type: asset_reference_version_append)`. Don't add the whole
   earlier sheet when the body changes: it copied the old proportions even when
   told not to.
4. Check the new sheet as in step 6, likeness first.

The one edit allowed on a sheet is the grain repair in step 6, which changes
nothing the sheet shows.
