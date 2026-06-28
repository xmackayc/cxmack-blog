# Personal blog style guide

Base: Ghost "Principle" theme structure and components.
Reskin: dark mode default, gradient-texture highlights on interactive elements.
Purpose: a build spec another Claude instance (or any developer) can read and implement without seeing the original references.

---

## 1. Intent

A personal blog. Editorial, content-first, magazine-style index — same bones as Principle (featured post, dense card grid, persistent sidebar) — but reskinned dark, with colour appearing only where the reader interacts: links, buttons, tags, focus states. Everything else stays quiet charcoal so the gradients read as alive rather than decorative wallpaper.

---

## 2. Color tokens

Near-black base with a faint cool undertone, not pure `#000`.

```
--bg:             #0d0b10   page background
--surface-1:      #161319   cards, sidebar panels
--surface-2:      #1d1922   raised elements, ghost-button fill
--border:         rgba(255,255,255,0.09)   default hairline
--border-strong:  rgba(255,255,255,0.16)   hover / active state
--text:           #f3f1f4   primary text
--text-muted:     #95909d   metadata, captions, excerpts
--radius:         10px      default corner radius, 12px for cards
```

No separate "accent colour" token — the gradient textures (section 3) serve that role. Don't introduce a flat brand blue/green on top of them.

---

## 3. Gradient highlight system

Five photographic gradient textures, each named, each with a job. Use them as `background-image`, not as generated CSS gradients — the grain and asymmetry is the point.

| Name | Character | Primary use |
|---|---|---|
| Aurora | cool magenta → blue → teal, dreamy | primary buttons |
| Ember | warm red → orange, moodier | secondary buttons |
| Mint drift | teal → cream → orange | hover sweep on ghost/outline elements |
| Nightglow | dark magenta → blue → teal | blurred halo glows |
| Prism | full-spectrum diagonal, most saturated | featured CTA, underline accents, tag highlights |

### Interaction techniques

**Filled button** — texture sits behind a dark scrim at rest; scrim lifts on hover so colour blooms in.
```css
background-image: linear-gradient(rgba(10,8,12,0.62), rgba(10,8,12,0.62)), var(--grad-aurora);
/* on hover, drop scrim opacity to ~0.3 over 0.35s */
```

**Halo button** — plain dark button with a blurred, oversized copy of the gradient glowing behind it via `::before` (`inset: -6px`, `filter: blur(14px)`, `opacity: 0.55`, intensifying on hover). Use for actions that shouldn't be loud at rest — subscribe, sign in.

**Ghost / outline button** — transparent border at rest; texture sweeps in from hidden to visible on hover via the same scrim technique, starting at full opacity scrim (invisible) and dropping to ~0.4 on hover.

**Text link underline** — no fill at all. A 2px gradient bar under the link grows from a short stub to full width and pans across on hover. Cheapest, quietest highlight — use for inline article links and nav items.

**Featured CTA (signature element)** — filled + halo combined, with a slow ~9s ambient `background-position` drift so it feels alive before the reader even touches it. Use once per page maximum (hero, newsletter signup) — this is the one place to spend visual boldness.

### Accessibility rules for gradients

- Always keep a dark scrim under text on filled gradient buttons — never raw texture behind text. Minimum scrim opacity 0.5 at rest.
- Respect `prefers-reduced-motion`: disable the ambient drift and hover transitions, snap to end state instead.
- Visible focus ring on every interactive element (`outline: 2px solid #fff; outline-offset: 3px`) — gradients alone are not a sufficient focus indicator.

---

## 4. Typography

```
--font-display: 'Space Grotesk', sans-serif   headings, nav, button labels
--font-body:    'Inter', sans-serif            body copy, excerpts, metadata
```

Display face carries personality (slightly technical, geometric — works with the gradient aesthetic). Body face stays neutral and highly legible at small sizes for dense card grids. Don't substitute a serif for the display face — that would pull toward Principle's literary-magazine feel, which doesn't match a dark gradient-tech aesthetic.

Headings: `font-weight: 500–600`, tight letter-spacing (`-0.01em`). Body: `400`, `line-height: 1.6–1.7`.

---

## 5. Layout & components

Carried over from Principle, restyled dark:

- **Index page**: large featured post at top, then a dense card grid (image, category tag, title, excerpt, date). Same density as Principle — magazine-style, not minimal.
- **Sidebar**: persistent — author bio, tag filter list, "featured posts" mini-list. Dark `--surface-1` panel, `--border` hairline separators.
- **Post cards**: image (consistent crop ratio across the grid), category tag as a small pill using `--grad-prism` as a 1px gradient border instead of a flat colour fill, title, one-line excerpt in `--text-muted`.
- **Markdown elements**: full H1–H6, italic/bold/bold-italic, blockquote (left border in `--grad-aurora` instead of flat grey), ordered/unordered lists.
- **Callouts**: emoji-flagged aside boxes, `--surface-2` background, no gradient needed here — keep these calm and readable.
- **Header cards**: heading + subheading + CTA button (use the filled or halo button technique from section 3), optional background image.
- **Sign-up / newsletter cards**: repeated through long-form content and in the footer. CTA button uses the featured-CTA treatment if it's the primary one on the page, otherwise the standard filled button.
- **Bookmarks**: rich link preview cards for citing other posts/sites — `--surface-1` card, `--border` outline, no gradient.
- **Galleries**: 2 and 3-image grid layouts, unchanged from Principle.
- **Audio/video embeds**: native players; restyle scrubber accent colour to `--grad-prism` if the platform allows a custom accent, otherwise leave native.
- **File download blocks**: filename, size, download icon — `--surface-1` row, no gradient.
- **Product cards**: image, name, short description, "Buy now" CTA using the filled button technique.

---

## 6. Tone (carry-over, adjust per writer)

Principle's default voice is confident, benefit-led, slightly corporate-blog. For a personal blog, loosen this: first person where natural, shorter sentences, less "value prop" framing in excerpts. Keep the structural pattern (colon-based contrast headlines are fine if they suit the writer) but drop the marketing register.

---

## 7. What NOT to carry over from Principle

- Light mode as default — this theme is dark-only by spec.
- Flat single-colour accent buttons — always use a named gradient texture instead.
- Sponsored-ad sidebar slot — drop unless monetization is actually planned.
