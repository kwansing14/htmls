# CLAUDE.md

## HTML Style Guide

All HTML pages generated in this repo should follow the visual style established by `mlx-mac-guide.html` (the reference implementation). Aim for the same look and feel: a dark, warm, terminal-flavored "field guide" aesthetic with amber accents.

### General rules

- Single self-contained `.html` file: all CSS in one `<style>` block in `<head>`, all JS in one `<script>` before `</body>`. No external CSS/JS files.
- Include `<meta name="viewport">`, a descriptive `<title>`, and a `<meta name="description">`.
- Mobile-responsive via `clamp()`, `auto-fit` grids, and `@media` breakpoints (~920px for layout collapse, ~780px to hide top nav).
- Respect `prefers-reduced-motion`: disable animations, typewriters, and smooth scrolling when set.

### Color palette (CSS custom properties on `:root`)

```css
:root{
  --bg:#0b0a08;            /* page background — near-black warm */
  --bg-2:#100e0a;
  --surface:#15130d;       /* cards, callouts, stats */
  --surface-2:#1d1a12;     /* inline code background */
  --border:rgba(255,200,120,.12);
  --border-strong:rgba(255,200,120,.26);
  --text:#f4efe4;          /* primary text — warm off-white */
  --muted:#a89c84;         /* secondary text */
  --faint:#6f6755;         /* labels, comments, footnotes */
  --accent:#ffb43d;        /* amber — links, numbers, highlights */
  --accent-bright:#ffd584; /* inline code text, bright accents */
  --warn:#ff7a59;          /* warnings, secondary accent */
  --grid:rgba(255,180,60,.045);
}
```

Body paragraph/list text color is `#d8d0c0` (slightly dimmer than `--text`); `<strong>` uses `--text`.

### Typography

Load from Google Fonts (with `preconnect`):

- **Display** (`--disp`): `'Bricolage Grotesque'` — headings (`h1`–`h4`), big stat numbers. Weight 800 for h1/h2, 600 for h3/h4. Tight letter-spacing (−.02em) and line-height (~1.0).
- **Body** (`--body`): `'Hanken Grotesk'` — paragraphs, lists. Line-height 1.6.
- **Mono** (`--mono`): `'JetBrains Mono'` — code, nav, TOC, labels, section numbers, table headers, footnotes, eyebrow badges.

Sizes: hero h1 `clamp(2.7rem, 8.2vw, 6.4rem)`; h2 `clamp(1.7rem, 4vw, 2.7rem)`; code 13px; mono UI labels 11–13px; small uppercase labels get letter-spacing `.14–.18em`.

Limit paragraph/list width to `max-width:66ch` for readability.

### Page atmosphere (fixed background layers)

Three fixed, pointer-events-none layers behind content:

1. **Grid** — 64px amber line grid (`--grid`), faded with a radial `mask-image` ellipse anchored at the top.
2. **Glow** — large radial gradients of amber (`rgba(255,180,61,.16)`) and warm-red near the top corners.
3. **Noise** — inline SVG `feTurbulence` data-URI at `opacity:.04` with `mix-blend-mode:overlay`.

### Layout

- Fixed translucent top bar: `rgba(11,10,8,.72)` + `backdrop-filter:blur(14px)`, 1px bottom border. Left: brand (small gradient square logo mark + mono wordmark). Right: mono nav links that turn amber on hover. Hide nav below 780px.
- Content wrapper: `.wrap{max-width:1180px;margin:0 auto;padding:0 clamp(16px,4vw,40px)}`.
- Two-column shell for the article body: sticky left TOC (226px) + main content, `gap:48px`; collapses to one column (TOC hidden) below 920px.
- TOC: mono 12.5px links with two-digit numbers (`01`, `02`…), 2px left border on the active item, scroll-spy via `IntersectionObserver` (rootMargin `-30% 0px -60% 0px`).
- `html{scroll-behavior:smooth;scroll-padding-top:90px}` and `scroll-margin-top` on sections so anchors clear the fixed header.

### Hero

- Eyebrow pill: mono text + pulsing amber dot, rounded-full border, faint amber background.
- Huge display headline with one key word wrapped in `.glow` (amber color + `text-shadow` glow + italic).
- Muted subtitle (max-width ~620px) with `<b>` highlights in `--text`.
- A decorative **terminal window** (mac traffic-light dots, mono title, typewriter-animated lines via JS) when the topic suits it.
- Row of **stat chips**: bordered surface cards with a big display-font number (amber) and a mono `// label` below.
- Staggered entrance animation: `.rv` class (fade + translateY 22px rise, `.8s cubic-bezier(.2,.7,.2,1)`) with incremental `animation-delay`s.

### Content sections

- Each `<section class="block">` separated by a 1px top border, `padding:54px 0`.
- Section header: mono two-digit number in amber beside the h2 (`.sec-head` flex row).
- `p.lead` for section intros (muted, 1.08rem).
- List markers (`li::marker`) in amber.
- Inline links: amber text, thin amber bottom border, no underline.
- Inline code `code.ic`: mono, `--surface-2` background, 1px border, 6px radius, `--accent-bright` text.

### Components

**Code blocks** — bordered rounded container (`#0d0b07` background) with a header bar (icon + filename/label in mono, faint amber tint background) and a **copy button** (mono, bordered, turns green "copied ✓" briefly via JS clipboard). Syntax highlighting with manual token spans:
`.tok-c` comments (faint, italic) · `.tok-k` keywords (amber) · `.tok-s` strings (`#9fe6a0`) · `.tok-f` functions/flags (bright amber) · `.tok-n` numbers (`#e6b3ff`) · `.tok-p` punctuation (muted).

**Callouts** — surface card with 3px amber left border and a mono `▎i` icon; `.warn` variant swaps to `--warn` color with `▎!`.

**Tables** — wrapped in a bordered rounded `.table-wrap` with horizontal scroll; mono uppercase amber `th` on faint amber background; row hover tint; mono first column.

**Card grids** — `.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr))}`; each card is a bordered surface panel with a mono amber index label (e.g. `pkg / mlx-lm`, `err / oom`), a display-font h4, and small muted text. Hover: `translateY(-3px)` + stronger border.

### Footer

Top border, multi-column flex of link groups with mono uppercase faint headings; external links suffixed with `↗`. End with a `.foot-note` — small mono disclaimer/credits paragraph in `--faint`.

### JavaScript behaviors

Keep JS minimal, vanilla, and dependency-free:

1. Copy-to-clipboard buttons on every code block.
2. Scroll-spy highlighting for the TOC.
3. Optional hero terminal typewriter animation (skipped entirely under reduced motion).
