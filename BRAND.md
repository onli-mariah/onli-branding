# Onli Brand Design System

> The canonical design reference for all Onli-branded sites.
> Any agent or developer can use this repo to build an on-brand site in minutes.

---

## Quick Start

```html
<!-- Import the single globals.css file for all tokens + reset -->
<link rel="stylesheet" href="globals.css" />
```

Or import individual token files:

```css
@import 'tokens/colors.css';
@import 'tokens/typography.css';
@import 'tokens/spacing.css';
@import 'tokens/grid.css';
@import 'tokens/animation.css';
```

Component styles are standalone CSS Module-compatible files in `components/`.

---

## Brand Identity

| Property | Value |
|---|---|
| **Primary Font** | Montserrat (Google Fonts) |
| **Mono Font** | Geist Mono |
| **Surface Color** | `#f6f6f6` (light warm gray) |
| **Text Color** | `#171717` (near-black) |
| **Body Text** | `#6f655c` (warm muted brown) |
| **Accent Palette** | Khaki `#7A7060` · Steel `#6A7070` · Slate `#5A6068` |
| **Tone** | Minimalist, warm, premium, quiet confidence |
| **Casing** | lowercase for UI labels, sentence case for body copy |

---

## Design Principles

1. **Quiet luxury** — No loud gradients, no neon. Warm neutrals and generous whitespace.
2. **Typographic hierarchy** — Weight and size do the work. Rarely use color for emphasis.
3. **Pill-based UI** — Rounded pill shapes (`border-radius: 100px`) are the primary interactive element.
4. **Content-first grid** — `2fr 3fr 7fr` column system puts the image/content in the dominant position.
5. **Scroll-driven narrative** — Full-viewport sticky cards reveal content on scroll.
6. **Accessible by default** — Reduced-motion, focus-visible, semantic HTML.

---

## Color Palette

### Surfaces
| Token | Hex | Usage |
|---|---|---|
| `--surface-light` | `#f6f6f6` | Page background, card backgrounds |
| `--surface-dark` | `#2A2A2A` | Dark sections, premium cards |

### Text
| Token | Hex | Usage |
|---|---|---|
| `--color-text-dark` | `#171717` | Headlines, primary text |
| `--color-text-body` | `#6f655c` | Body paragraphs, descriptions |
| `--color-text-light` | `#FFFFFF` | Text on dark backgrounds |
| `--color-text-muted` | `#6B6763` | Captions, metadata |
| `--color-hero-muted` | `#8c857d` | Hero section muted text |

### UI Elements
| Token | Hex | Usage |
|---|---|---|
| `--color-pill-bg` | `#EFEFEF` | Pill backgrounds |
| `--color-border-light` | `#e5e5e5` | Light borders, dividers |
| `--color-border-dark` | `rgba(255,255,255,0.1)` | Borders on dark surfaces |
| `--color-card-dark` | `#2A2A2A` | Premium card background |
| `--color-card-light` | `#FFFFFF` | Standard card background |

### Accent
| Token | Hex | Usage |
|---|---|---|
| `--color-khaki` | `#7A7060` | Feature cards, accent |
| `--color-steel` | `#6A7070` | Feature cards, accent |
| `--color-slate` | `#5A6068` | Feature cards, accent |
| `--color-muted` | `#6f655c` | Muted accents (same as body) |

---

## Typography

| Role | Size | Weight | Tracking |
|---|---|---|---|
| Preloader logo | `48px` | 600 | `-0.04em` |
| Hero text (desktop) | `clamp(24px, 4vw, 40px)` | 400 / 500 | `-0.02em` |
| Hero text (mobile) | `clamp(16px, 4.5vw, 22px)` | 400 / 500 | `-0.02em` |
| Section title (desktop) | `clamp(24px, 2.2vw, 44px)` | 500 | `-0.02em` |
| Section title (mobile) | `clamp(22px, 5vw, 32px)` | 500 | `-0.02em` |
| Body text | `clamp(0.875rem, 1vw, 1.05rem)` | 300 | normal |
| Pill label (desktop) | `14px` | 500 | normal |
| Pill label (mobile) | `13px` | 500 | normal |
| Nav wordmark | `14px` | 300 | normal |
| Nav link | `13px` | 400 | normal |
| Footer header | `13px` | 500 | normal |
| Footer link | `13px` | 400 | normal |
| Copyright | `12px` | 400 | normal |
| Eyebrow | `11px` | 400 | `0.08em` |

---

## Spacing Scale (8px base)

| Token | Value |
|---|---|
| `--space-1` | `8px` |
| `--space-2` | `16px` |
| `--space-3` | `24px` |
| `--space-4` | `32px` |
| `--space-5` | `48px` |
| `--space-6` | `64px` |
| `--space-7` | `80px` |
| `--space-8` | `120px` |
| `--space-9` | `160px` |
| `--space-10` | `240px` |

---

## Grid System

| Property | Value |
|---|---|
| Content columns | `2fr 3fr 7fr` |
| Column gap | `5px` |
| Row gap | `24px` |
| Container max-width | `1800px` |
| Mobile gutter (`< 768px`) | `24px` |
| Tablet gutter (`768px – 1199px`) | `80px` |
| Desktop gutter (`≥ 1200px`) | `32px` |

### Breakpoints

| Name | Range |
|---|---|
| Mobile | `< 768px` |
| Tablet | `768px – 1199px` |
| Desktop | `≥ 1200px` |

---

## Component Specs

### Pills
- `border-radius: 100px`
- `box-shadow: 0 1px 4px rgba(0, 0, 0, 0.07)`
- Desktop: `padding: 14px 24px`, `font-size: 14px`
- Mobile: `padding: 12px 20px`, `font-size: 13px`
- `font-weight: 500`, lowercase text

### Navigation
- Two-pill layout: wordmark pill + CTA/nav pill
- Pill height: `44px`
- `z-index: 10`
- Padding: `24px` top, `16px` bottom
- Mobile: hamburger menu with drawer (`border-radius: 16px`)

### Cards (Image)
- `border-radius: 12px`
- `aspect-ratio: 16 / 9`
- `object-fit: cover`
- Background placeholder: `#f0f0f0`

### Footer
- `border-top: 1px solid var(--color-border-light)`
- 3-column grid (desktop), 2-column (tablet), 1-column (mobile)
- Copyright row: `margin-top: 80px`, centered, `12px` `#D4D4D4`

### Preloader
- Full-viewport overlay, `z-index: 9999`
- Background: `#f4f4f5`
- Logo: `48px` Montserrat weight 600, `letter-spacing: -0.04em`
- Progress bar: `160px × 1px`, black fill

---

## Animation

| Pattern | Value |
|---|---|
| Default transition | `0.2s ease` |
| Content fade in | `opacity 0.6s ease-out, transform 0.6s ease-out` |
| Accordion expand | `1.2s cubic-bezier(0.19, 1, 0.22, 1)` |
| Content reveal | `opacity 0.8s cubic-bezier(0.19, 1, 0.22, 1)` |
| Pill hover | `opacity: 0.8, transform: translateX(4px)` |
| Scroll indicator | `caretBounce 1.6s ease-in-out infinite` |
| Preloader exit | `transform 0.8s cubic-bezier(0.65, 0, 0.35, 1)` |

### Reduced Motion
All animations collapse to `0.01ms` duration. `.animate-in` elements render at full opacity with no transform.
