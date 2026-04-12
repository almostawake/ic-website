# Adding a new testimonial

Before you start, read the existing testimonial cards in `index.html` under `<section id="testimonials">` and open the images in `assets/images/testimonial-*.jpg`. Those four cards (Monika, Jan, Eden, Alison) are your anchor for consistency. Every decision below should be checked against them.

## Inputs you'll be given
- A photo file path (usually a raw iPhone shot, portrait orientation, with EXIF rotation)
- A block of raw testimonial text (2–6 paragraphs)
- The rider's first name and the horse's first name (horse name is often in the photo filename)

## 1. Crop the photo

### Always use PIL, never sips
sips mishandles EXIF rotation and has burned us before. Start every crop job with:

```python
from PIL import Image, ImageOps
im = ImageOps.exif_transpose(Image.open(src))
```

### Target format
- **3:2 landscape aspect ratio**, exact. The card uses `aspect-[3/2] object-cover`, so any mismatch will cause the browser to re-crop.
- Max 1600px on the long edge, JPEG quality 88, `optimize=True`.
- Filename: `assets/images/testimonial-<firstname-lowercase>.jpg`.

### Centre the subjects, both axes
The single most important rule: **vertically and horizontally centre the double subjects (rider + horse) in the crop.** Don't take a centred slice of the source — take a slice centred on *the subjects*, wherever they happen to sit in the frame.

1. Eyeball the auto-oriented source and estimate the bounding box around both subjects: `(x_min, y_min, x_max, y_max)`.
2. Compute the subject centre: `sx = (x_min + x_max) / 2`, `sy = (y_min + y_max) / 2`.
3. Pick a crop width (see scale guidance below), compute `crop_height = crop_width * 2/3`.
4. Set `x_start = sx - crop_width/2`, `y_start = sy - crop_height/2`, then clamp to the image bounds.

If the subjects are near-centred horizontally but low in the frame (common in portrait iPhone shots where the photographer aimed at the faces), a naive centred slice will leave the subjects drifting bottom-right. Always compute offsets from the subject centre, not the image centre.

### Subject scale — match the anchors
Compare candidates directly against Monika, Jan, and Eden in the same terminal session. The existing cards put horse + rider at roughly **60–75% of the frame height**. Not a wide landscape where they're tiny, not so tight you lose context. If your candidate is noticeably tighter or looser than those three, adjust.

For portrait iPhone sources, start with `crop_width = 0.75–0.85 × source_width`. Full source width usually leaves subjects too small; below 0.60 gets claustrophobic.

### Hard constraints
- **Both horse ears intact.** Never clip them. Leave a visible margin of sky/ceiling above the ears — a crown-tight crop looks wrong.
- **No half-cropped people or objects** at the edges.
- **Rider's expression readable.** Face clear, eyes unobstructed.
- **Trim the bottom before the top.** Grass/floor is usually discardable; sky/ceiling gives context. When in doubt, shift the crop window up.

### Candidate generation
Generate 3–5 candidates with different vertical offsets (and a couple of different zoom levels if the subjects might benefit from a tighter shot). Read each one back with the Read tool. **Don't guess — iterate.** Pick the one that best satisfies all the rules above.

### Self-critique checklist before saving
Open your best candidate alongside Monika, Jan, and Eden. Ask:
1. Are the subjects centred both axes?
2. Do they occupy a comparable proportion of the frame to the other three?
3. Is the emotional energy similar (warmth, candidness, engaged moment)?
4. Are both horse ears intact with breathing room above?
5. Is anything distracting at the edges (signage, half-people, clutter)?
6. Would this look out of place next to the others in the 4-up grid?

If any answer is "no", iterate. Don't ship a crop you wouldn't choose if it were the first one.

## 2. Pick the herald (pull quote)

The **herald** is the 1–3 sentence pull quote shown by default. The full testimonial is revealed when the reader clicks `… READ MORE`. Then `… SHOW LESS` collapses it back.

### The herald does not have to be contiguous
You may stitch non-adjacent sentences into a tighter pull. You may pick from the middle or end of the testimonial. The herald's job is to land hard, not to be faithful to paragraph structure.

### What makes a good herald
- **Standalone punch.** Reads powerfully without context. Passes the "fortune cookie" test.
- **On-brand emotional notes.** Warmth, trust, feeling supported, horse wellbeing, "it clicks", confidence growth, belonging. These are the IC positioning beats — prioritise them.
- **Horse-centred moments are gold.** Anything about the rider trusting Nat with the horse's wellbeing is IC's strongest differentiator. Prefer these if present.
- **Avoid review-y language.** "Highly recommended", "five stars", "would recommend", explicit lists of services — these flatten the editorial voice.
- **Length matches the neighbours.** Aim for 2–3 sentences, ~30–60 words. The grid uses `items-stretch`, so a 1-sentence pull quote leaves a visibly short card with awkward whitespace above the button.

### Check rhythm
Read all four compact cards in sequence after your edit. They should feel similar in length, voice, and weight. If yours is noticeably shorter or lighter, stitch a stronger version.

## 3. Build the card

Copy Eden's card — it's the canonical template for the expand/collapse pattern — and swap:
- `<img src>` and `alt` (alt describes the specific moment, not generic "rider with horse")
- Compact paragraph → your herald
- `<div x-show="expanded">` → full testimonial split into paragraphs. Clean up typos, convert curly apostrophes to straight, trim filler — but don't rewrite the voice.
- Name block: `<p class="font-sans...">FirstName</p>` and `<p class="font-mono...">with HorseName</p>`
- Keep the `… READ MORE` / `… SHOW LESS` buttons exactly as they appear in Eden's card.

### Grid columns
Current layout is `md:grid-cols-2 lg:grid-cols-4` — designed for four cards. If you're adding a 5th+, reconsider the grid (e.g. 3×2 on desktop).

## 4. Verify in the browser

After editing:
1. Reload the dev server page (cache-busted if needed).
2. Scroll to `#testimonials` and screenshot the full grid.
3. Read the screenshot yourself.

Check:
- Card heights equalise cleanly
- The new image reads at the same scale/warmth as the others
- The herald is legible and doesn't orphan words
- `… READ MORE` sits on its own line flush at the bottom of the quote area
- Click expand → full text reveals without layout jump
- `… SHOW LESS` collapses it back

If anything is off, iterate. Don't ship.
