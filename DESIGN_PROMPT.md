# MyBayt — Design prompt for Claude (claude.ai)

Paste the fenced block below into Claude on claude.ai whenever you want to (re)generate, redesign, or add a page to the MyBayt site. The brief is opinionated about the brand and **explicitly bakes in the routing/link-hygiene rules that caused the GitHub Pages 404 on `MyBayt Redesign.html`** — so the regenerated output won't ship with broken cross-page navigation.

---

````
## Project brief

Design a multi-page static marketing site for **MyBayt**, a Marrakech-based developer of luxury riad-style residences. The site is in **French** and addresses high-net-worth international buyers who already love Morocco — the visit's job is to make the buyer want to schedule a private viewing of a real project, not browse a listings catalogue. The site lives at `https://amineouarzazi.github.io/mybayt-website/` (GitHub Pages, project repo). It must feel like a magazine spread for a place worth flying to, not a real-estate portal.

Pages in scope:
- `index.html` — homepage (hero, brand promise, featured projects, story, contact)
- `Nos Projets.html` — projects index (filterable grid of all residences)
- `projects/<Project Name>.html` — one detail page per residence (currently five: Eden Gueliz, Vallée de Guéliz 1, 2, 3, 4)

## Aesthetic direction

Named direction: **"warm Marrakech editorial luxury"** — think a high-end print monograph about Moroccan architecture, not a SaaS landing page. Restrained, slow, confident. Lots of whitespace, hairline rules, italic display serifs used like marginalia. Photography does the heavy lifting; UI gets out of the way. NOT: glassy frosted cards, purple gradients, big rounded shadows, hero video loops with overlay text.

## Visual system

- **Palette (use these exact tokens — they are the existing brand):**
  - `--bg` warm cream (oklch 97.2% 0.009 62, ~`#F7F2EA`)
  - `--bg-warm` slightly deeper cream surface
  - `--ink` espresso (~`#1F1814`) — primary text
  - `--ink-mid` muted clay — secondary text
  - `--ink-light` faded stone — tertiary text / metadata
  - `--gold` tarnished brass (~`#B8945A`) — accent + italic display + active state only
  - `--gold-light` paler brass — hover/selection backgrounds
  - `--rule` warm hairline (~`#E5DCCF`) — every border, divider, and grid line
- **Typography:** display = `Cormorant Garamond` weights 300/400/500 with italic; body = `DM Sans` weights 300/400/500. Both via Google Fonts. Display headlines are large and almost-skinny: `clamp(56px, 8vw, 132px)` on hero, `line-height: 0.95`, `letter-spacing: -0.02em`. Italic + gold is the brand's signature accent inside headlines (e.g., *projets* set in italic gold inside an otherwise espresso headline).
- **Spacing:** generous, editorial. Sections breathe at ~80–128px vertical padding on desktop, ~48px on mobile. Hairline rules (`1px solid var(--rule)`) separate sections instead of background fills.
- **Imagery:** photo-led. Use real Marrakech architectural photography (Palmeraie courtyards, Hivernage facades, terracotta walls at golden hour). Project photos live under `/uploads/` in the repo and are referenced relatively. No stock people-in-suits, no drone shots of generic skylines, no AI-look renders.

## Layout — page by page

**`index.html`**
1. Sticky nav: logo center, "← Retour à l'accueil" link variant left when nested, gold-bordered phone pill right.
2. Editorial hero: full-viewport, French display headline with one italic gold word, short italic lede behind a thin gold left rule, single ghost-button CTA "Demander une visite privée".
3. Brand promise strip: three short pillars ("Clarté", "Transparence", "Excellence") laid out as numbered editorial columns, no icons.
4. Featured projects: 2–3 hero project cards linking to their detail pages.
5. "Notre histoire" section: a single long-form column with a portrait-orientation photo bleeding to the right margin.
6. Contact block: three phone numbers, one email, address line, gold hairline above and below.
7. Footer: small caps, low-key, no social-media icon clutter.

**`Nos Projets.html`**
1. Same sticky nav with `← RETOUR À L'ACCUEIL` linking back to `index.html`.
2. Header: enormous "Nos *projets*" headline (italic gold "projets"), short italic lede on the right.
3. Filter strip (sticky under the nav): pill filters TOUS / EN COURS / NOUVEAU / DISPONIBLE / LIVRÉ with live counts and an "X projets affichés" indicator on the right.
4. Project grid: portrait-orientation cards, hairline border, no shadow, hover scales the image to 1.03 over 600ms ease-out. Metadata in small caps overlaid bottom-left.
5. Same contact block + footer as the homepage.

**`projects/<Project>.html`**
1. Sticky nav with two back-links: `← Tous les projets` (to `../Nos Projets.html`) and the logo (to `../index.html`).
2. Hero: full-bleed photo, project name in display serif, status pill in gold.
3. Specs table: surface, rooms, delivery date, status — small caps, tabular numerics.
4. Photo gallery: lightbox-on-click, no carousel auto-rotate.
5. "Voir les autres projets" link back to `../Nos Projets.html`.
6. Same contact block + footer.

## Copy stance

The existing French copy is the source of truth — keep it verbatim where it exists. Where new copy is needed, write it in tight French with the same voice: declarative, understated, no exclamation marks, no marketing superlatives ("incroyable", "exceptionnel" used at most once on the whole site), no anglicisms. Numbers and dates use French conventions (espace insécable before units, "1 250 m²" not "1,250 sqm").

## Motion

Be selective. Earned moments only:
- Slow Ken Burns drift on the homepage hero photo, ~20s loop.
- Staggered fade-and-rise on first paint, 80ms per element, only on the hero.
- Hover lift + image scale on project cards (already specified above).
- Smooth in-page scroll on anchor links.
No parallax, no scroll-jacking, no entrance animations on every section.

## Technical setup — and the link rules that MUST be followed

This site is deployed on **GitHub Pages**, which is **case-sensitive** and serves `index.html` as the root URL. The previous version of this site shipped a 404 because the homepage was renamed to `index.html` for deployment but several internal links still pointed to the old `MyBayt Redesign.html`. Do not repeat that mistake. The rules below are non-negotiable.

- **Filename rules.**
  - The homepage **must** be named exactly `index.html`. No alternate name, no marketing-y filename, no spaces, no capitalization variants. GitHub Pages will only serve a directory at its root URL if `index.html` is present.
  - Subpages may keep their human-readable names (e.g., `Nos Projets.html`, `projects/Eden Gueliz.html`) for continuity with the existing URLs, but treat the casing and spacing as load-bearing — once a filename ships, every link that references it must spell it identically.
  - Prefer no spaces in *new* filenames you introduce (use kebab-case: `nos-projets.html`, `projects/eden-gueliz.html`). Spaces survive on GitHub Pages but force `%20` encoding in every href, which is fragile and bug-prone.
- **Internal link rules.**
  - Every internal href must be **relative**, not absolute. Never write `href="/MyBayt Redesign.html"` or `href="https://amineouarzazi.github.io/..."` — those break on local preview, and absolute paths break the moment the repo is renamed or moved to a custom domain.
  - From a top-level page (e.g., `Nos Projets.html`), link to the homepage as `href="index.html"` — never as a re-named alias.
  - From a subdirectory page (e.g., `projects/Eden Gueliz.html`), link to the homepage as `href="../index.html"` and to the projects index as `href="../Nos Projets.html"`. Always include the `../` — testing locally with `file://` will sometimes mask a missing one; GitHub Pages will not.
  - Asset references (`<img src>`, CSS, JS) follow the same relative-path rule with the same `../` discipline.
  - Filename casing in every href must match the actual file on disk **exactly** — `Nos Projets.html`, not `nos projets.html` or `Nos%20Projets.HTML`. Windows local previews are case-insensitive and will hide casing bugs that GitHub Pages exposes.
- **Navigation consistency.** Every page must include the same sticky nav with the appropriate "back" target:
  - Homepage: no back link (or anchor links to in-page sections only).
  - `Nos Projets.html`: back link to `index.html` reading `← Retour à l'accueil`.
  - `projects/<Project>.html`: back link to `../Nos Projets.html` reading `← Tous les projets`, **and** the logo links to `../index.html`.
- **Artifact format.** Each page is a single self-contained `.html` file with inline `<style>` and inline `<script>`. External resources allowed: Google Fonts, the existing logo URL on `mybayt.ma`, and Unsplash/local repo images. No bundlers, no build step, no React — these pages are served as plain static HTML by GitHub Pages.

## Quality bar / things to avoid

- No purple→blue gradients, no glassy frosted cards, no Inter / Roboto / Poppins anywhere.
- No emojis in headings or body copy. No emoji bullets.
- No "Lorem ipsum" — write tight French placeholder copy in the brand voice if the real copy isn't supplied.
- No generic stock-photo people-in-suits. No drone shots of city skylines that aren't Marrakech-specific.
- No "Powered by" / "Built with" / "Made with ❤" footers.
- No JavaScript-mandatory navigation — every link must work with JS disabled, because GitHub Pages users include search-engine crawlers and link-preview bots that don't run JS.

## Self-check before finishing

Before you stop, run this checklist explicitly. **Do not skip step 1** — it is exactly what shipped broken last time.

1. **Link audit.** For every page you produced, list every `href=` and `src=` value. For each one that points to a local file (not `http://`, `https://`, `mailto:`, `tel:`, or a `#` anchor): confirm a file with that **exact name and casing** exists in the page's relative directory. If the homepage is named `index.html`, then no link anywhere in the codebase may say `MyBayt Redesign.html`, `home.html`, `accueil.html`, etc. — they must all say `index.html` (or `../index.html` from subdirectories).
2. **Casing audit.** Pretend the filesystem is case-sensitive (because GitHub Pages is). `Nos Projets.html` ≠ `nos projets.html` ≠ `Nos%20projets.html`. Every reference must match the on-disk filename byte-for-byte.
3. **Palette audit.** No off-palette default colors leaked in (no white `#FFFFFF` if the page background is warm cream, no system-blue link colors, no browser-default red error states).
4. **Typography audit.** No fallback `Times New Roman` or `Arial` is showing because a Google Fonts link is missing on a subpage — every page must include the same Cormorant Garamond + DM Sans `<link>` block.
5. **Re-read as the target user.** A French-speaking buyer who already loves Marrakech opens this on a laptop in Paris. Does it feel like the named direction — warm Marrakech editorial luxury — or did it drift toward generic real-estate / generic luxury / generic agency? If it drifted, fix it before stopping.
```
````

---

## Notes (for you, Amine)

- **Why this prompt prevents the bug from recurring.** The "Technical setup — and the link rules that MUST be followed" section explicitly names the `MyBayt Redesign.html` → `index.html` mistake as the failure mode and converts it into hard rules: relative paths only, `index.html` as the canonical homepage filename, `../` discipline from subdirectories, exact-casing requirement. The self-check step 1 is a forced link audit — Claude has to enumerate every `href` and confirm it resolves before stopping. That is the gate that would have caught this last time.
- **Aesthetic direction was crystallized from the existing site.** I read the actual CSS in `Nos Projets.html` (oklch palette, Cormorant Garamond + DM Sans, hairline rules, gold italic accents) and named the direction "warm Marrakech editorial luxury." If you want to push the site in a different direction, swap that section — the technical guardrails will still apply.
- **Filename pragmatism.** I left the existing spaced filenames (`Nos Projets.html`, `projects/Eden Gueliz.html`) in place because changing them would break inbound links and SEO, but I recommended kebab-case for *new* pages. If you ever do a clean migration, rename them all to kebab-case in one PR and update every href in the same commit.
- **Suggested follow-up prompts** if a regenerated page doesn't land:
  - Too quiet: *"Make the hero louder — bigger headline, full-bleed warm-tinted photo, drop the lede, add a thin gold rule under the headline."*
  - Drifts toward generic real-estate: *"Re-read as a Paris-based buyer flipping through *Architectural Digest* — the page should feel like a feature article, not a listing card."*
  - Cards feel too app-like: *"Strip the shadows and rounded corners off the project cards. Hairline border, no fill, no radius."*
