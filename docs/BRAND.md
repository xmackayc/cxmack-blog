# Brand Guidelines — cxmack.com

## Brand Story

This site is Connor's personal engineering log — a running record of what he's building, thinking about, and into (engineering, fitness, music). It doubles as a **working portfolio piece**: alongside his systems engineering work, Connor takes on a small number of freelance website design projects (roughly 1–2 a month). The site itself is the proof of that skill, not just a description of it — its design quality is doing sales work as much as the writing is.

That dual purpose shapes every decision below: the site needs to read as technically credible to an engineer, *and* as evidence of design taste to someone deciding whether to hire Connor to build their site.

## Audience

Two primary readers, both evaluating Connor rather than casually browsing:

1. **Recruiters / hiring managers** — scanning for technical depth and communication quality. They're judging substance over polish, but sloppy execution undercuts the substance.
2. **Prospective design clients** — people who found the site and are wondering "could this person build something like this for me?" They're judging the site *as a design artifact* first, content second.

Design and copy decisions should serve both: keep the technical voice intact (that's what recruiters want), but never let craft slip (that's what clients are buying).

## Voice & Tone

**Dry, understated humor over a terse, technical base.** The terminal/CLI framing (`whoami`, commit hashes, git branch in the prompt) already does a lot of personality work on its own — the writing shouldn't compete with it by being loud or jokey. Humor should read like a well-placed code comment: quiet, a little deadpan, easy to miss if you're skimming.

**Do:**
- Short, declarative sentences. Say the thing once.
- Let technical precision be the humor's straight man — deadpan delivery next to real substance.
- First person, direct address ("I'm into...", not "This blog explores...").

**Don't:**
- Reach for exclamation points, hype language, or "excited to announce" phrasing.
- Overexplain a joke or soften technical claims with hedging.
- Drift into corporate-portfolio voice ("passionate about delivering solutions") — that undercuts the credibility with both audiences.

## Visual Identity

### Color Palette

Dark is the default and primary experience; light is an explicit opt-in via the theme toggle (persisted in `localStorage`).

**Dark (default)**
| Token | Value | Use |
|---|---|---|
| `--bg` | `#0d0b10` | page background |
| `--surface-1` | `#161319` | hero / section backgrounds |
| `--surface-card` | `#110e18` | card backgrounds |
| `--text` | `#f3f1f4` | primary text |
| `--text-muted` | `#95909d` | secondary text |
| `--border` | `rgba(255,255,255,.07)` | hairlines |
| `--border-strong` | `rgba(255,255,255,.14)` | emphasized hairlines |
| `--grad-aurora` | `linear-gradient(135deg, #d946ef, #3b82f6, #06b6d4)` | avatar ring accent |

**Light (opt-in)**
| Token | Value |
|---|---|
| `--bg` | `#ffffff` |
| `--surface-1` | `#f8f6f5` |
| `--surface-card` | `#f0eeec` |
| `--text` | `#0d0d0d` |
| `--text-muted` | `#757575` |
| `--border` | `rgba(0,0,0,.08)` |
| `--border-strong` | `rgba(0,0,0,.16)` |

**Accent / syntax colors** (used sparingly, in the hero prompt line and status dot — think of these as the site's "syntax highlighting", not a general accent palette to reach for):
- Green `#4ade80`, Blue `#60a5fa` / `#3b82f6`, Purple `#a78bfa`

### Typography

Three-font system, loaded from Google Fonts:

| Role | Font | Weights | Used for |
|---|---|---|---|
| Display | `Space Grotesk` | 600–700 | Headings, name, card titles — tight letter-spacing (-0.01 to -0.02em) |
| Body | `Inter` | 400–600 | Paragraphs, excerpts, buttons |
| Mono | `JetBrains Mono` | 400–500 | Metadata, timestamps, tags, hashes, nav, the shell prompt — anything that should feel "system-generated" |

Base type scale uses a `62.5%` root-font trick (1rem = 10px), stepping down to `56.25%` under 720px. Reference sizes: hero name 3.6rem, article title 4rem, prose body 1.8rem @ 1.75 line-height.

### Layout

- Max content width: **1200px** (720px for the article reading column).
- Homepage: **12-column bento grid** (7/5/4 spans), collapsing to 2 columns at 900px, 1 column at 560px.
- Corner radius: **8px**, applied consistently to cards, images, hero art.
- Vertical rhythm is generous and deliberate — 4rem+ top padding on `<main>`, 5.6rem before article H2s (bordered like a changelog section break).

### Components

- **Identity hero** — avatar with gradient ring + status dot, name with a blinking terminal cursor, stat chips, GitHub-style weekly activity strip.
- **Dev card** — post preview with a left accent bar that brightens on hover, tag pills, monospace footer (hash · relative time · "read →"), subtle lift on hover.
- **Tag pill** — monospace, bordered, transparent background.
- **Section header** — uppercase muted eyebrow + bottom border, mono "view all" link.
- **Prose** — bordered H2 dividers, muted blockquotes, bordered inline code, dark code blocks (intentionally dark even in light mode).

### Motion

- Transitions throughout run 0.2–0.3s; link hovers fade to 0.7 opacity. Nothing snaps or bounces — the whole site should feel calm, not flashy.
- Respects `prefers-reduced-motion` (all animations/transitions collapse to near-zero duration).
- Dark is enforced as default even under system light preference — light mode only activates on an explicit user click, never automatically.

## Open Issue

`Header.astro`, `Footer.astro`, and `HeaderLink.astro` currently reference a legacy token set (`--color-bg`, `--color-text`, `--color-border`, `--accent-color`, `--body-S-font-size`, etc.) that no longer exists in `global.css` — leftover from before the dark-developer-aesthetic redesign. These should be migrated to the tokens documented above (`--bg`, `--text`, `--border`, etc.) so the nav/footer actually render with the palette this guide describes, rather than browser fallback values.
