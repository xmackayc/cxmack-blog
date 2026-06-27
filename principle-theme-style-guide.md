# Principle Theme — CSS & Style Guide
> Ghost theme by HighFive Themes · Demo: principle.highfivethemes.com
> Inspected: 2026-06-26

---

## 1. Color System

Colors are defined as CSS custom properties on `:root`. Light/dark mode is toggled via a `data-theme="dark"` attribute on `<html>` — there is no `prefers-color-scheme` media query.

### Tokens

| Token | Light | Dark |
|---|---|---|
| `--color-bg` | `#ffffff` | `#1d1d1f` |
| `--color-bg-block` | `#f8f6f5` | `#29292a` |
| `--color-text` | `#0d0d0d` | `#ededed` |
| `--color-border` | `#ede6e3` | `#343435` |
| `--color-border--hover` | `#dfcdc6` | `#515151` |
| `--color-secondary-foreground` | `#757575` | `#757575` |
| `--color-cur-nav-page` | `#f9f3f2` | `#343435` |
| `--accent-color` | `#D74000` (burnt orange) | `#ededed` |
| `--ghost-accent-color` | `#D74000` | — |

### Notes
- The palette is intentionally minimal: warm off-whites, near-black text, warm beige borders, and a single bold burnt-orange accent.
- In dark mode the accent flips to near-white so CTA buttons remain legible.
- The CTA button gradient is built from two derived tokens:
  ```css
  --clr-1: var(--accent-color) 0%;
  --clr-2: var(--color-border) 100%;
  /* used as: background: linear-gradient(to right, var(--clr-1), var(--clr-2)) */
  ```

---

## 2. Typography

### Font Families

```css
--font-family:        var(--gh-font-body, "Instrument Sans", sans-serif);
--font-family-titles: var(--gh-font-heading, "Instrument Sans", sans-serif);
--font-weight-body:   400;
--font-weight-titles: 600;
```

A single typeface — **Instrument Sans** — is used for both body and headings. The `--gh-font-*` hooks allow Ghost's admin to override fonts at the CMS level.

### Heading Scale

All headings share `line-height: 138%` and use negative letter-spacing for a tight, modern feel.

| Token | Size | Letter-spacing |
|---|---|---|
| `--h1` | `4rem` | `-0.04em` |
| `--h2` | `3rem` | `-0.02em` |
| `--h3` | `2.3rem` | `-0.02em` |
| `--h4` | `1.9rem` | `-0.02em` |
| `--h5` | `1.6rem` | `-0.02em` |

### Body Text Scale

| Token | Size | Line-height |
|---|---|---|
| `--body-M` | `1.6rem` | `155%` |
| `--body-S` | `1.4rem` | `155%` |
| `--body-XS` | `1.2rem` | `155%` |

### UI Text Tokens

These are used for specific interface elements:

| Token | Size | Weight | Letter-spacing | Notes |
|---|---|---|---|---|
| `--subheading` | `1.1rem` | `700` | `0.1em` | Uppercase; sidebar labels e.g. "TAGS" |
| `--button` | `1.4rem` | `600` | `0em` | All CTA buttons |
| `--tags` | `1.4rem` | `600` | `-0.02em` | Tag pills |
| `--tabs` | `1.4rem` | `600` | `-0.02em` | Board/List toggle |
| `--author` | `1.1rem` | `700` | `0.1em` | Byline label (e.g. "LEO JOHNS") |
| `--date` | `1.2rem` | `500` | `0em` | Date badges on cards |
| `--semi-14` | `1.4rem` | `600` | `0em` | Utility label |
| `--medium-14` | `1.4rem` | `500` | `0em` | Utility label |
| `--semi-12` | `1.2rem` | `600` | `0em` | Utility label (tooltips) |

---

## 3. Header

```css
.header {
  border: 1px solid var(--color-border);
  border-radius: 16px;           /* strongly rounded pill shape */
  background: transparent;
  transition: border-color 0.3s;
}
```

- Sits in a left-column sidebar layout.
- Contains: circular avatar (40×40px, `border-radius: 50%`), author name, search icon, dark/light toggle, and the CTA "Sign In" button.
- Overflow nav items appear in a `.header-popup` — fades in with `opacity: 1` / `z-index: 1`, styled with `border-radius: var(--border-radius-blocks, 8px)` and `border: 1px solid var(--color-border)`.

---

## 4. Button (`.btn`)

```css
.btn {
  background: var(--linear-gradient);   /* accent orange gradient */
  color: #ffffff;
  padding: 11.5px 20px;
  font-size: var(--button-font-size);   /* 1.4rem */
  font-weight: var(--button-font-weight); /* 600 */
  border-radius: var(--border-radius-button, 6px);
  transition: opacity 0.3s, background-color 0.3s;
  display: flex;
  align-items: center;
  gap: 4px;
}

/* Hover: light wash overlay */
.btn::after {
  content: "";
  background: linear-gradient(0deg, rgba(255,255,255,0) 0%, rgba(255,255,255,0.2) 100%);
  opacity: 0;
  transition: opacity 0.3s;
}

.btn:hover::after {
  opacity: 1;
}

/* Arrow icon rotates on hover */
.btn:hover svg {
  rotate: 45deg;
  transition: rotate 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}
```

A sidebar subscribe-banner variant reverses the gradient and uses the accent color as text:
```css
.sidebar-subscribe-banner .btn {
  background: linear-gradient(#ffffff 0%, #f4eee b 100%);
  color: var(--accent-color);
  border: 1px solid var(--color-border);
}
```

---

## 5. Post Cards

### Board View (`.board-card`)

```css
.board-card {
  width: 32%;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.board-card__img-link {
  border-radius: var(--border-radius-images, 8px);
  overflow: hidden;
  aspect-ratio: 4 / 3;
}

/* Image zoom on hover */
.board-card__img-link:hover .board-card__img-wrapper {
  scale: 1.05;
}

.board-card__title {
  font-family: var(--font-family-titles);
  font-size: var(--h4-font-size);       /* 1.9rem */
  font-weight: var(--font-weight-titles); /* 600 */
  letter-spacing: var(--h4-letter-spacing); /* -0.02em */
  line-height: var(--h4-line-height);   /* 138% */
}

.board-card__excerpt {
  font-size: var(--body-S-font-size);   /* 1.4rem */
  line-height: var(--body-S-line-height); /* 155% */
}

/* Date badge — top-left of image */
.board-card__date {
  position: absolute;
  top: 16px; left: 16px;
  background-color: var(--color-bg);
  color: var(--color-secondary-foreground);
  border-radius: 4px;
  padding: 3px 7px;
  backdrop-filter: blur(15px);
  font-size: var(--date-font-size);     /* 1.2rem */
  font-weight: var(--date-font-weight); /* 500 */
}
```

### List View (`.list-card`)

```css
.list-card {
  display: flex;
  gap: 24px;
  width: 100%;
}

.list-card + .list-card {
  padding-top: 32px;
  margin-top: 32px;
  border-top: 1px solid var(--color-border);
}

.list-card__img-link {
  max-width: 435px;
  border-radius: var(--border-radius-images, 8px);
  aspect-ratio: 16 / 9;
}

.list-card__title {
  font-size: var(--h3-font-size);       /* 2.3rem */
  font-weight: var(--font-weight-titles); /* 600 */
  letter-spacing: var(--h3-letter-spacing); /* -0.02em */
}

.list-card__excerpt {
  font-size: var(--body-M-font-size);   /* 1.6rem */
  line-height: var(--body-M-line-height); /* 155% */
  margin-top: 8px;
}
```

---

## 6. Tag Pills (`.post-tag`)

```css
.post-tag {
  background-color: var(--color-bg-block);  /* warm light grey */
  border: 1px solid var(--color-border);    /* beige border */
  border-radius: var(--border-radius-tags, 100px); /* full pill */
  padding: 5px 12px;
  height: 27px;
  font-size: var(--tags-font-size);         /* 1.4rem */
  font-weight: var(--tags-font-weight);     /* 600 */
  letter-spacing: var(--tags-letter-spacing); /* -0.02em */
  transition: background-color 0.3s, border-color 0.3s, color 0.3s;
}
```

---

## 7. Sidebar

```css
.sidebar {
  border-left: 1px solid var(--color-border);
  max-width: 350px;
  width: 100%;
}

.sidebar__posts {
  padding: 32px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

/* Section labels: "TAGS", "FEATURED POSTS" */
.sidebar__title {
  font-size: var(--subheading-font-size);     /* 1.1rem */
  font-weight: var(--subheading-font-weight); /* 700 */
  letter-spacing: var(--subheading-letter-spacing); /* 0.1em */
  text-transform: uppercase;
}

/* Featured post thumbnails */
.sidebar-list-post__img-link {
  width: 80px;
  height: 80px;
  border-radius: var(--border-radius-images, 8px);
}

/* Divider between posts */
.sidebar-list-post + .sidebar-list-post {
  margin-top: 4px;
  padding-top: 20px;
  border-top: 1px solid var(--color-border);
}
```

---

## 8. Board/List Toggle (`.main-header__toggle`)

A custom CSS sliding toggle — no native input element.

```css
.main-header__toggle {
  border-radius: 100px;
  border: 1px solid var(--color-border);
  background-color: var(--color-bg-block);
  padding: 4px;
  height: 40px;
  max-width: 150px;
  transition: border-color 0.3s, background-color 0.3s;
}

/* The sliding pill indicator */
.main-header__toggle::after {
  content: "";
  background-color: var(--color-bg);
  position: absolute;
  width: calc(50% - 3px);
  border-radius: 100px;
  border: 1px solid var(--color-border);
  transition: transform 0.3s, background-color 0.3s, border-color 0.3s;
}

.main-header__toggle-btn {
  font-size: var(--tabs-font-size);         /* 1.4rem */
  font-weight: var(--tabs-font-weight);     /* 600 */
  letter-spacing: var(--tabs-letter-spacing); /* -0.02em */
  color: var(--color-text);
  z-index: 5;
  position: relative;
}
```

---

## 9. Navigation (`.nav-item`)

```css
.nav-item {
  border-radius: var(--border-radius-navigation, 6px);
  background-color: var(--color-bg);
  transition: background-color 0.3s;
}

/* Active/current state */
.nav-item--current {
  background-color: var(--color-cur-nav-page); /* warm blush: #f9f3f2 */
}

/* Hover: background fill + accent icon stroke */
.nav-item:hover {
  background-color: var(--color-cur-nav-page);
}

.nav-item:hover .nav-item__link svg path {
  stroke: var(--accent-color);
}
```

---

## 10. Border Radius Tokens

All radius values use a CSS custom property with a hardcoded fallback, allowing Ghost's design system to override them:

| Token | Default fallback | Used on |
|---|---|---|
| `--border-radius-button` | `6px` | `.btn`, `.header-popup` |
| `--border-radius-images` | `8px` | Card images, sidebar thumbnails |
| `--border-radius-navigation` | `6px` | Nav items |
| `--border-radius-tags` | `100px` | Tag pills (full pill) |
| `--border-radius-tag-icons` | `50%` | Tag icon circles |
| `--border-radius-blocks` | `8px` | Popup blocks |
| Header itself | `16px` (hardcoded) | `.header` |

---

## 11. Transitions

All transitions use `0.3s` duration consistently with no explicit easing (defaults to `ease`). The one exception is the button arrow icon rotation:

```css
/* Standard transitions used across the theme */
transition: background-color 0.3s, color 0.3s;
transition: background-color 0.3s, border-color 0.3s, color 0.3s;
transition: opacity 0.3s;
transition: border-color 0.3s;
transition: scale 0.3s;

/* Button arrow — spring easing */
transition: rotate 0.5s cubic-bezier(0.16, 1, 0.3, 1);
```

A global reduced-motion override sets all durations to `0.01ms`:
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 12. Dark Mode

Dark mode is toggled by setting `data-theme="dark"` on the `<html>` element (via the sun/moon icon button in the header). All color tokens flip via scoped CSS rules:

```css
/* Example pattern */
[data-theme="dark"] {
  --color-bg: #1d1d1f;
  --color-bg-block: #29292a;
  --color-text: #ededed;
  --color-border: #343435;
  --color-border--hover: #515151;
  --color-cur-nav-page: #343435;
  --accent-color: #ededed;
}
```

Body and all components inherit the new values automatically since everything references tokens. The `body` transition `background-color 0.3s, color 0.3s` ensures a smooth mode switch.

---

## 13. Custom Author Injection

A page-level inline style tag injects author-specific overrides for the Leo Johns demo:

```css
.logo {
  height: 28px;
}

.header-author__leo,
.sidebar-author-card__leo {
  &::after {
    content: 'developer';
    font-family: var(--font-family);
    font-size: var(--subheading-font-size);
    font-weight: var(--subheading-font-weight);
    letter-spacing: var(--subheading-letter-spacing);
    text-transform: uppercase;
    color: var(--color-secondary-foreground);
  }
}
```

This appends the role label "developer" beneath the author name using a CSS `::after` pseudo-element — no extra HTML required.

---

## Summary

The Principle theme is built around three principles:

1. **Token-first design** — every color, size, spacing, and radius is a CSS custom property, making the entire theme reskinnable by swapping `:root` values.
2. **Warm neutral palette + single accent** — the warm beige/off-white/near-black palette with burnt orange (`#D74000`) as the sole accent keeps the focus on content.
3. **Consistent 0.3s motion** — all hover and state transitions use the same 0.3s duration for a cohesive, unhurried feel.
