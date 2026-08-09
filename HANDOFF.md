# Handoff — Francisco Zenteno Smith Personal Website

**Status:** Design prototyping phase, in-browser (Claude Artifacts). Not yet moved to a repo.
**Next environment:** Claude Code, scaffolding an Astro project.
**Owner:** Francisco Zenteno Smith — Senior Operations Research Consultant, American Airlines.

## 1. Goal

A personal site positioning Francisco for Principal/Staff-level OR/optimization roles, built around one wedge: *software-engineering rigor applied to operations research, shipped as production-grade decision-support software.* SEFOP (his open-source framework, github.com/sefop) is the flagship proof point.

## 2. Artifacts produced (source of truth — do not re-derive)

| Artifact | What it contains |
|---|---|
| `index.html` (this conversation's outputs) | Homepage prototype. All copy, section order, design tokens (CSS custom properties in `:root`), the converging-lines SVG signature element, and responsive breakpoints are implemented directly in this file. |
| `sefop.html` (this conversation's outputs) | Dedicated SEFOP page prototype. Two-tier layout: hook + mocked demo card above `.deep-divide`, technical deep-dive (math formulation, Clean Architecture diagram, solver grid, CI/CD table, repo grid) below. Separate dark/green design tokens from the homepage, also in `:root`. |

Both files are self-contained (HTML/CSS/JS in one file each, Google Fonts CDN for IBM Plex Sans + IBM Plex Mono). Treat their CSS variables as the canonical design tokens — don't ask Francisco to restate colors/type/spacing, read them from the files.

## 3. Locked decisions

- **Build path:** prototype in Claude Artifacts (done for v1) → port to Astro via Claude Code → deploy to GitHub Pages.
- **Site structure:** homepage + one dedicated SEFOP page (not a full multi-page site, not single-scroll-only).
- **Creative direction:** technical minimalism. Homepage = light, monochrome + single blue accent (`#1F4FD1`), sans-serif (IBM Plex Sans/Mono). SEFOP page = its own dark, green-accented (`#3ECF8E`) identity — separate from homepage per Francisco's explicit choice, since SEFOP has its own icon/brand that should stay distinct.
- **Homepage section order:** hero (bridge thesis) → proof bar → about (with headshot) → SEFOP teaser → experience → skills (two-column OR|SWE split) → awards/education → contact.
- **Tech stack:** Astro (static output, no client-side app framework needed).
- **Hosting/domain:** GitHub Pages, custom domain `fjzs.com` (**availability unconfirmed — see Open Items**).
- **Contact:** LinkedIn as primary CTA; lightweight backup form (needs a real Formspree — or equivalent — endpoint; current form in `index.html` is UI-only, `onsubmit="return false"`).
- **No dedicated résumé/CV feature** (no PDF download, no "Quick View" toggle) — the homepage sections are considered sufficient.
- **Photo:** headshot placeholder in the About section only (not hero). Real photo not yet supplied — placeholder div in `index.html`.
- **SEFOP page depth:** two-tier — plain-language hook + live demo link up top for all visitors, full technical substance (math formulation, architecture, CI/CD) below for engineers who scroll.

## 4. Key facts locked in Claude's memory (persist across sessions, may not be visible to a fresh agent/session)

- Technical debt reduction figure is **67%**, measured via static code analysis tools. Use 67% consistently across LinkedIn, resume, and site — **not 25%** (an earlier inconsistent figure). This is written into `index.html`'s Experience section already.
- Proof-bar/hero framing uses a **capability statement**, not the 67% figure directly: *"Transforms legacy systems into production-grade decision-support software."* The 67% figure lives deeper in the page (Experience section), not the hero/proof bar.

## 5. Open items / blockers

1. **American Airlines start date** — unconfirmed. Marked inline in `index.html` timeline as `[confirm start date]`.
2. **Role between education (UCSD/PUC Chile) and American Airlines** — unconfirmed, possibly a gap or an unlisted prior role. Marked inline in `index.html` timeline.
3. **`fjzs.com` availability** — not verified through a registrar (direct fetch was blocked by robots.txt, which is inconclusive). Check on Namecheap/name.com/GoDaddy before purchasing or wiring DNS.
4. **Real SEFOP icon/brand assets** — Francisco has a chosen icon concept (from an earlier session, 20 SVG concepts generated) but the actual file/hex values haven't been shared in this conversation. `sefop.html`'s dark/green palette is a placeholder pending this.
5. **Headshot photo** — not yet supplied. Placeholder box in `index.html` About section.
6. **Formspree (or equivalent) account** — needs to be created and wired into the contact form's `action`/endpoint before launch.
7. **Live demo card in `sefop.html` is a styled mockup**, not an actual embed of `sefop-python-advanced.onrender.com` (artifacts can't reliably iframe external Render apps). The "Try the live demo" button does link out correctly. Decide in the Astro build whether to attempt a real embed or keep the mockup + link pattern.

## 6. Next steps, in order

1. Resolve open items #1, #2, #4, #5 with Francisco (content/assets only — no new design decisions needed).
2. Confirm `fjzs.com` availability and register it if free; otherwise pick a fallback and update all references.
3. Any final visual/copy tweaks to `index.html` / `sefop.html` while still in the fast-iteration Artifacts environment — do this *before* porting to Astro.
4. Hand off to Claude Code:
   - Scaffold an Astro project.
   - Port `index.html` → homepage route/layout, `sefop.html` → `/sefop` route, preserving the CSS custom properties as shared/page-scoped design tokens.
   - Extract shared nav/footer into Astro components (both prototypes currently duplicate this markup).
   - Wire the real Formspree endpoint.
   - Set up GitHub Pages deployment + custom domain `CNAME`.
5. Deploy, then update `fjzs.github.io` and LinkedIn to point at the new domain (or decide whether `fjzs.github.io` redirects or retires).

## 7. Reference links consulted this session

- Reference design inspiration: https://www.adamdejans.com/
- Current (outdated) portfolio: https://fjzs.github.io/
- LinkedIn: https://www.linkedin.com/in/francisco-zenteno-smith/
- SEFOP org: https://github.com/sefop
- SEFOP repos referenced for content: `sefop-python-starter`, `sefop-python-advanced` (source of the math formulation, Clean Architecture breakdown, and CI/CD workflow list used in `sefop.html`)
- SEFOP live demo: https://sefop-python-advanced.onrender.com
