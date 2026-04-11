# Inner Circle Equestrian — Project Guidelines

**If the user says something looks wrong, it is wrong. Don't second-guess. Don't explain why your screenshot disagrees. Fix it.**

## Ways of Working

### Always visually verify before completing a task
Screenshot the **deployed or locally-served URL in a real browser** — never a file:// path, never DevTools-only. DevTools viewport captures don't match real browser rendering (browser chrome eats vertical space, shifting centering and layout). If the user sends a screenshot that contradicts yours, theirs is correct — fix it, don't explain the discrepancy.

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

### Brand vs website separation
- `brand/` contains the canonical brand definition — the brand-pack.html exploration kit and captions.json research archive. This is implementation-agnostic and upstream of everything else.
- `_data/site.yml` contains only what Jekyll templates consume — colors, contact details, tagline, values. This is the downstream implementation of brand decisions.
- When making a brand decision, update `brand/` first, then propagate to `_data/site.yml`.

### Site data lives in `_data/site.yml`
Contact info, colours, tagline, and values are defined in `_data/site.yml` and referenced in templates via `site.data.site.*`. Don't hardcode these values in templates.

### Tailwind custom colors via config
Brand colors are registered in the Tailwind config as custom colors (e.g. `ic-navy`, `ic-gold`) and used as utility classes throughout. Don't use hex codes directly in templates — always use the named color classes. Current palette is B (Classic & Clean — navy-dominant with gold accent).

### Keep deployment simple
Use GitHub's built-in Pages builder (Deploy from branch), not a custom Actions workflow. No Node.js build step — Tailwind via CDN, Jekyll handles the rest. Avoid adding complexity unless there's a clear need.

### Static first
Avoid backends for as long as possible. Static is simpler, faster, and easier to maintain with AI assistance. Form submissions, email signups etc should use third-party services (e.g. Formspree, Mailchimp) not custom backends.

### Git hygiene
- `photos/` is gitignored — working/source photos stay local
- `assets/images/` is for committed site images only (optimised, final versions)
- `brand/` is committed — it's the shareable brand source of truth
