# CLAUDE.md — IE-nergy Spring Campaign Landing Page

Project conventions for Claude Code. Read this before writing or modifying any code in this repo.

## Stack

- **Vanilla HTML / CSS / JavaScript only.** No React, Vue, Svelte, or any other framework.
- **No build step.** No bundlers, no transpilers, no Tailwind/PostCSS pipeline. What lives in the repo is what ships.
- **No npm runtime dependencies.** A one-off Node.js setup script (e.g. `mailchimp-setup.js`) that runs once against the Mailchimp Marketing API is OK, but the deployed site itself is static.

## Deployment target

- **GitHub Pages**, served from the `main` branch root.
- Every asset path must work when the site is hosted at `https://<username>.github.io/ie-nergy-landing/`. Use relative paths, not absolute.

## Brand identity

### Palette
Sampled from `assets/ie-nergy-can.jpg`. Cold, premium, high-energy — with a warm aurora accent that keeps it from reading as sterile corporate blue.

Working set:
- **Signal blue** — `#1E7FD6` — the logo circle + brand midtone. Primary brand colour.
- **Deep arctic** — `#0D4E9B` — darker can body, shadow, dark surfaces.
- **Aurora indigo** — `#2A3A7A` — the swirl behind the can, secondary dark.
- **Ice cyan** — `#A3DCF5` — water splash highlights, light accents.
- **Frost white** — `#E8F4FA` — lightest surfaces, backgrounds.
- **Sunset coral** — `#FFA26C` — the warm backlight peeking through the ice. Use sparingly, as a complementary accent against the blue dominance.
- **Ink** — `#071128` — body text, near-black on light surfaces.

**Dominance rule:** blues carry ~70% of any composition. Coral is an accent (never more than 10% of surface area).

### Tone
- Confident, youthful, Madrid-urban.
- Short copy, strong verbs. No filler.
- Spanish-English code-switching allowed where it feels native (e.g. *"Go arctic"* ; *"Despierta el campus"*).
- Never corporate, never apologetic.

## Hero visual

- The hero image for every layout is `assets/ie-nergy-can.jpg`.
- Treat it as the visual anchor. All type, spacing, and colour decisions flow from it.
- Never crop or recolour the can without showing the user before and after.

## Accessibility + conversion baseline

- Minimum WCAG AA colour contrast on all text.
- All form inputs must have visible labels wired with `for` / `id`.
- Focus rings visible (never `outline: none` without a replacement).
- The primary CTA must be the single most obvious action on the page.
- The GDPR consent checkbox is required, unchecked by default, and must block submission when unticked.
