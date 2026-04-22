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

### Adding a new testimonial
When adding a new rider testimonial (new photo + quote to the `#testimonials` grid), read `CLAUDE-TESTIMONIALS.md` before making any edits or crops. It covers the cropping rules, herald-quote selection, and card structure we've iterated on.

## Brand

### Identity
- **Palette B — Classic & Clean**: Navy-dominant (#1C2856) with Soft Gold (#B8976A) accent. No greens, no blush, no terracotta. Cool neutrals (white, cool grey #F1F3F5).
- **Typography**: Cormorant Garamond (display/editorial — matches the logo's serif), Montserrat (UI/body — matches the logo's tracked "EQUESTRIAN"), JetBrains Mono (technical/labels).
- **Logo usage**: Horse mark SVG + CSS typography in web contexts. Full lockup PNGs/SVGs in `brand/logo/` for print/merch/social only. Never use the full lockup as an image in hero sections — it doesn't scale well.
- **Target audience**: Amateur horse owners. Not "adult" — just "amateur."

### Positioning
- The equestrian industry caters to children and elite competitors. Amateur horse owners are assumed to be elite — or invisible. IC exists for them.
- Everything under one roof: agistment, lessons, horse schooling, competition support.
- No pressure, no assumptions, no judgement — support at your pace.
- "Supported by Natalie Innes" — never "run by" or "owned by."

### Voice
- Warm, supportive, non-judgemental — all content should reflect this.
- Tone down competition results — focus on confidence to participate, not winning.
- Avoid imagery that could trigger anxiety (e.g. medals/podiums can alienate the target audience).

### Design language
Read `brand-pack.html` (at repo root) CSS and components before building any UI. The aesthetic is editorial-luxury, not SaaS. Key non-obvious rules that are easy to default away from:
- Cormorant Garamond at weight 300–400, never bold
- Cream (#F5F0E8) page base, not white
- Section labels in JetBrains Mono, steel-coloured
- Small architectural radii (4–16px), never pills
- Faint navy borders on cards, not grey blocks

### Brand source of truth
- `brand-pack.html` (repo root) — visual brand bible (colours, type, logos, copy bank, mockups). This is upstream of everything else. Served publicly at `/brand-pack/` so it's shareable with Nat and collaborators. The rest of `brand/` is excluded from the Jekyll build.
- `brand/logo/` — master logo assets (8 variants in PNG + SVG: full lockup, icon+wordmark, icon-only, text-only, each in navy + reverse white).
- When making a brand decision, update `brand/` first, then propagate to `_data/site.yml` and templates.

### Historical research (for when you need community voice, testimonials, or original rationale)
- `brand/captions.json` — 80 curated Facebook and Instagram posts. Each entry has caption, date, comments, and a `note` field flagging the best material. Grep for `"note":` to find curation; look for `GREAT`, `OUTSTANDING`, `EXCELLENT`, `key brand messaging` to surface the strongest testimonials and copy.
- `brand/research/IC_Brand_Palettes.pdf` — the original three-palette exploration (A Warm, B Classic, C Soft) that led to the Palette B decision. Historical context for why we chose what we chose.

## Technology

### Use Tailwind properly — don't bandaid CSS
Tailwind has utilities for almost everything. Before writing custom CSS or manipulating images to work around a layout problem, check if Tailwind already has a class for it. Hero overlays are `bg-gradient-to-r` with opacity modifiers. Image fitting is `object-cover` with `object-position`. Responsive visibility is `hidden md:block`. If you're writing raw CSS or doing image manipulation to fix a layout issue, you're probably doing it wrong.

### One clean image + CSS overlay for heroes
Use a single clean source image and let CSS handle the overlay gradient for text readability. Don't bake gradients or text into images — that makes them inflexible and creates the exact burned-in-text problem we hit. The CSS overlay is the correct approach.

### Site data lives in `_data/site.yml`
Contact info, colours, tagline, and values are defined in `_data/site.yml` and referenced in templates via `site.data.site.*`. Don't hardcode these values in templates.

### Tailwind custom colors via config
Brand colors are registered in the Tailwind config as custom colors (e.g. `ic-navy`, `ic-gold`) and used as utility classes throughout. Don't use hex codes directly in templates — always use the named color classes.

### Dev server
- Start: `bundle exec jekyll serve` (run in background, serves on :4000)
- Stop: `lsof -ti:4000 | xargs kill`

### Keep deployment simple
Use GitHub's built-in Pages builder (Deploy from branch), not a custom Actions workflow. No Node.js build step — Tailwind via CDN, Jekyll handles the rest. Avoid adding complexity unless there's a clear need.

### Static first
Avoid backends for as long as possible. Static is simpler, faster, and easier to maintain with AI assistance. Form submissions, email signups etc should use third-party services (e.g. Formspree, Mailchimp) not custom backends.

### Use Chrome DevTools Protocol for browser automation
Prefer `mcp__chrome-devtools__*` tools over `mcp__claude-in-chrome__*` extension tools — CDP is more reliable. Use the extension only if DevTools can't do what's needed.

### Git hygiene
- `brand/photos/` — committed working/source photos (excluded from Jekyll build)
- `assets/images/` is for committed site images only (optimised, final versions)
- `brand/` is committed — it's the shareable brand source of truth
