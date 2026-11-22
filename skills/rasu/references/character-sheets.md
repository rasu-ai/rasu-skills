# Character sheets of real people

A Character's sheet is the contract for every later picture and clip of that
character. Whatever is wrong in it follows the character into every shot: a face
taken from an old photo, grain, a ring on the wrong hand. This is the method that
made sheets of real people look like them and stay clean, measured over 100
generations on real people's photos. Follow it in order.

## 1. Choose the pictures first

What goes in decides likeness more than any prompt.

- **Ask for their best photos, before any arrive.** People send whatever is
  nearest to hand; asked, they choose their best. Ask for the highest-quality
  originals they have: the files from their camera roll, never screenshots and
  never copies forwarded through a chat app, which sends a smaller file. Recent,
  sharp, well lit, with only that person in frame: a front view, a
  three-quarter view, a side view, and one full-body photo, head to toe. If all
  they have is lower quality or group photos, take them, and say what you will
  do with them: the person is cut out of a group photo, and the sharpest face
  leads.
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
- **Ask for one full-body photo, for how their clothes sit.** Ask for one photo
  of them standing straight, head to toe, taken from a few steps back with the
  camera at about chest height, and not on the ultra-wide lens. Add it as a
  reference like the others: it carries the outfit, where a hem falls and how
  the trousers hang. Check it before you use it. A phone held low and close
  stretches the legs and shrinks the head: the same person read about seven and
  a half heads tall in one photo and nine in another. If they look much more
  than eight heads tall, or fewer than seven, say the photo is distorted and
  ask for another taken from further back.
- **Attach them so they are sent.** Upload with `media_upload_link`, then
  attach each picture to the Character with
  `asset_sheet (type: asset_media_link)`, role `reference`. Attaching is not
  sending: list each picture on the look that uses it, with its role, through
  `asset_look (type: asset_variant_update)` (`inputs` and `roles`: identity for
  the person's photos and the master portrait, wardrobe for an outfit photo,
  prop for an item picture). Then quote the look: its estimate says
  "N of 10 reference slots used" and names each picture. 0 means nothing will
  be sent, whatever the look's words say. Never use
  `asset_sheet (type: asset_reference_version_append)` for a photo: it makes
  the photo a picture OF the look, not a reference for it.

Before you write the Character's rows, consult Casting:
`engine (type: engine_consult)` with domain video, concept asset, the asset's id
and focus identity. For a real person the pictures carry the face: write
identifying marks (glasses, moles, scars, a signature hairstyle), never shape
words such as "round face", "full cheeks" or "thick, dark, straight eyebrows".
Shape words overcorrected the face every time they were tried: the brow words
pressed one person's brows down onto his eyes in every sheet and clip.

## 2. Consent

A real person's likeness needs their consent. Ask the person you are helping to
confirm it before the sheet is made, in one plain question.

## 3. A master portrait from the best neutral photo

Before the full sheet, turn the sharpest real front picture into a clean studio
portrait: same face, new light and backdrop. This one step did more for likeness
than anything else tried.

Make it from a photo with a **neutral face, mouth closed**, never a smiling
one, even when the smiling one is sharper. A smile squints the eyes and pulls
the brows down onto them, and the master carries that into every sheet and
clip: in testing, a master made from a smiling photo put one person's brows
right on his eyes (a gap of 0.015 of the distance between his eyes, against
0.059 in his own neutral photo), and he said every clip "looks funny". Made
again from his neutral selfie, the gap came back to 0.050 and he said it looked
right. Use `generate_raw (type: gen_image_generate)` with
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
on. Look at the eyes and brows, not only the face shape: in their own photo
there is a clear gap between brow and eye, and a master where the brows sit on
the eyes reads as someone else. Then attach it with
`asset_sheet (type: asset_media_link)`, role `reference`, and list it first on
the look as identity (step 1).

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

Send, in this order: the master portrait, one neutral real photo, the
full-body photo, then each item picture as prop. Never the earlier sheet, and
never the smiling photo once a neutral one exists. Rasu AI never sends an
earlier sheet on a look that lists the person's photos (step 8).

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
the wrist, the ring on a hand and lose their exact design.

**Find the items, then confirm them with the person.** Look through their
photos for anything worn: a watch, glasses, earrings, a ring, a chain or
pendant, a bracelet, shoes, a bag. Name each one you see in one message and
ask, for each, either its make and model (a watch's model number, the shoe's
name) or a close-up photo of it, and whether it belongs on the sheet. Never
guess a brand from a blurry wrist. In testing, the answers named a watch's
maker and model, dropped a chain and a pair of glasses that were in the photos,
and spelled out the letters on a pin: none of that could be read from the
photos.

Then, for each item they keep:

- **One picture of it alone**: one object on grey with no hand, wrist or
  person, made from their close-up or a product photo of the model they
  named. That worked for every item tried.
- **A prop row on the Character** (a row of its own in the prop group, through
  `asset_write (type: asset_update)`): what it is, its make and model, colours
  and size, and where it is worn: "Tissot PR516 chronograph: 40 mm steel case,
  navy dial with three silver sub-dials; on his LEFT wrist." The sheet then
  keeps it in every body view. Never put a ring on the hand that wears the
  watch.
- **Its picture on the look as prop** (step 1), and any lettering on it in the
  Exceptions row.
- An item they drop goes in the Never row by name, so it is not drawn from
  their photos.

Picture each item from two or three angles, each angle a picture of its own:

| Item | Angles |
|---|---|
| Watch | The dial straight on; a three-quarter view showing the crown and the side of the case; the strap and clasp |
| Ring | From the top; in side profile; a three-quarter view |
| Glasses | From the front; a three-quarter view; from the side, showing the temple |
| Shoes | From the side; a three-quarter front view; from the top, or the sole |
| Anything else | From the front; a three-quarter view; the back, or a detail |

Make the first angle from the person's own photo of the item. Make each other
angle with that first picture as its reference, one step only: never an angle
made from another made angle.

Text that is physically on the object must be in its pictures: a watch's dial
text, numerals, date window and bezel markings, an engraving. Lettering that is
approximate or imperfect is acceptable; lettering that is missing is not. Write
it into the Prop's sheet, where its Exceptions row lists the lettering it
keeps. The model still draws no labels, captions or panel text.

## 6. Look before you keep it

Open the result with `asset_read (type: media_look)` and check, in this order:

1. **Likeness:** beside a real photo from the same period as the references, is
   it them? Portraits face the camera, head level. Look at where the brows sit
   above the eyes and at the hair's texture as well as the face. A no goes back
   to step 1 or the master portrait (step 3), never to more words about the
   face.
2. **Proportions:** beside the full-body photo, do the clothes sit as they do on
   the person: where the hem falls, how the trousers hang? Fix a miss in the
   wardrobe row and a new picture (step 8). Do not generate again to lengthen or
   shorten the body itself. In testing the image model drew the same body
   whatever it was told or shown: a stated ratio, a proportion guide and the
   person's own full-body photo each left the legs within a point or two of
   where they started.
3. **Grain:** at full size, look in the dark areas and the bright areas as well
   as the plain backdrop. Speckle, crunchy skin or white halos along edges mean
   grain. A sheet or view that fails is generated again from its references on
   GPT Image 2.5 Sunburst, never edited.
4. **Items:** each signature item matches its own pictures and sits where the
   wardrobe row says, and its own pictures carry the lettering on the object.
   The picture holds no other lettering.
5. **Verification for video:** once the sheet is kept, step 7.

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
   `asset_write (type: asset_update)`: height in the build row, clothes in the
   base wardrobe row. Say where a garment lands on the body, not only "longer
   legs": "the hem of his blazer lands at mid-thigh".
2. Quote and generate a new picture of the look from its references, as in step 4,
   with `preview (type: asset_variant_estimate)` and
   `generate (type: asset_variant_generate)`. The earlier picture stays as the
   previous version.
3. Keep the approved face by sending the same raw sources again: the master
   portrait, their photos and the item pictures. **Never send the earlier
   sheet, and never portraits cropped out of it.** An earlier sheet copies its
   own drift into the new one (the old proportions, a stale item, brows on the
   eyes), and a crop uploaded from it looks to Rasu AI like a photo of the
   person. If the face drifts, fix the master portrait (step 3), not the
   sheet. Rasu AI holds this for you: a look that lists the person's photos
   never sends an earlier sheet of the same Character, as its anchor or as a
   reference, and shows each such picture on the rail as never sent.
4. Check the new sheet as in step 6, likeness first.

No edit is allowed on a sheet, grain included: a sheet or view that fails on
grain is generated again from its references (step 6). The master portrait in
step 3 is a different thing: it edits a photograph, never a sheet.

## 9. Another look: a new outfit

A second look of a real person (night out, gym, travel) is drawn from their
photos, not from the first sheet. A new look on a Character whose base look
lists their photos starts with those photos on its list. Add the outfit's own
photo as wardrobe if there is one, its items as prop, and write only what
changes in the look's words. In testing, three outfits drawn this way held the
face as well as one drawn from the earlier sheet, and kept the right navy watch
dial; the one sent the earlier sheet as its anchor carried that sheet's wrong
light-blue dial into the new outfit.
