# Inner Circle Equestrian — Project Guidelines

## Ways of Working

### Always visually verify before completing a task
Before declaring any visual task done, screenshot the result and review it yourself. If something is obviously wrong — text clashing with a background, images not centred, layout broken, bleed between sections — fix it before presenting to the user. Do multiple iterations silently if needed. The user should see your best effort, not your first attempt.

### Self-assess image crops and visual adjustments
When cropping or adjusting images, look at the result critically before presenting it. Check: is the subject centred? Are important elements cut off? Does it look intentional? If not, iterate. Don't present three variations when you can see that two of them are clearly wrong.

### Don't stop to check in during long-running tasks
When told to keep going, keep going. Don't pause every few steps to ask if the user wants to continue. Only stop if something actually breaks or you need a genuine decision from the user that you can't make yourself.

### Step back when you're going in circles
If you've tried the same approach 3+ times and it's not working, stop and reconsider the entire approach rather than making incremental adjustments to a bad solution. Ask yourself: "Am I bandaiding, or solving the actual problem?"

### Make assets shareable
When generating content (descriptions, palette options, brand copy), provide it in formats the user can immediately share — images for SMS, PDFs for stakeholder review, text files for clean copy/paste. Don't just put it in the chat.

### Lead with a recommendation
When presenting options, lead with your recommendation and explain why. The user values opinionated guidance over neutral menus. "Go with your gut" is the default mode.

### Brand voice guardrails
- Tone down competition results — focus on confidence to participate, not winning
- Avoid imagery that could trigger anxiety (e.g. medals/podiums can alienate the target audience)
- The brand is warm, supportive, non-judgemental — all content should reflect this
- "Supported by Natalie Innes" not "run by" or "owned by"

## Technology

### Use Tailwind properly — don't bandaid CSS
Tailwind has utilities for almost everything. Before writing custom CSS or manipulating images to work around a layout problem, check if Tailwind already has a class for it. Hero overlays are `bg-gradient-to-r` with opacity modifiers. Image fitting is `object-cover` with `object-position`. Responsive visibility is `hidden md:block`. If you're writing raw CSS or doing image manipulation to fix a layout issue, you're probably doing it wrong.

### One clean image + CSS overlay for heroes
Use a single clean source image and let CSS handle the overlay gradient for text readability. Don't bake gradients or text into images — that makes them inflexible and creates the exact burned-in-text problem we hit. The CSS overlay is the correct approach.

### Brand data lives in `_data/brand.yml`
All brand values — colors, tagline, values, contact info — are defined in `_data/brand.yml` and referenced in templates via `site.data.brand.*`. This is the single source of truth. Don't hardcode brand values in templates.

### Tailwind custom colors via config
Brand colors are registered in the Tailwind config as custom colors (e.g. `ic-navy`, `ic-sage`) and used as utility classes throughout. Don't use hex codes directly in templates — always use the named color classes.

### Keep deployment simple
Use GitHub's built-in Pages builder (Deploy from branch), not a custom Actions workflow. No Node.js build step — Tailwind via CDN, Jekyll handles the rest. Avoid adding complexity unless there's a clear need.

### Static first
Avoid backends for as long as possible. Static is simpler, faster, and easier to maintain with AI assistance. Form submissions, email signups etc should use third-party services (e.g. Formspree, Mailchimp) not custom backends.

### Git hygiene
- `photos/` is gitignored — working/source photos stay local
- `assets/images/` is for committed site images only (optimised, final versions)
- `*.txt` in root is gitignored (scratch files, copy drafts)
- Don't commit large binary files unnecessarily

### Palette switching (future)
When content is finalised, implement palette switching via CSS custom properties and page front matter. Routes `/a` and `/b` serve the same content with different palettes for stakeholder review. Typography switching will follow the same pattern.
