# Onli Brand — Website Architecture Guide

> How every Onli-branded site is built.
> This document is the structural blueprint. For design tokens and values, see [BRAND.md](./BRAND.md).

---

## Table of Contents

1. [Page Skeleton](#page-skeleton)
2. [Preloader](#1-preloader)
3. [Navigation](#2-navigation)
4. [Hero](#3-hero)
5. [Scroll Stack Sections](#4-scroll-stack-sections)
6. [Footer](#5-footer)
7. [The Grid System](#the-grid-system)
8. [Responsive Behavior](#responsive-behavior)
9. [Animation System](#animation-system)
10. [Branded Sites Reference](#branded-sites-reference)

---

## Page Skeleton

Every Onli site follows this vertical structure from top to bottom:

```
┌─────────────────────────────────────────────┐
│  PRELOADER (full-screen overlay, fades out) │
├─────────────────────────────────────────────┤
│  NAVIGATION (two-pill header)               │
├─────────────────────────────────────────────┤
│                                             │
│  HERO (generous whitespace + minimal text)  │
│                                             │
├─────────────────────────────────────────────┤
│  SCROLL STACK SECTION 1                     │
│  ┌─────────────────────────────────────────┐│
│  │  Pill Row (3 pills: label, name, CTA)  ││
│  │  Card 1 (title + body + image)         ││
│  │  Card 2 (sticky, reveals on scroll)    ││
│  │  Card N ...                            ││
│  └─────────────────────────────────────────┘│
├─────────────────────────────────────────────┤
│  SCROLL STACK SECTION 2                     │
│  ┌─────────────────────────────────────────┐│
│  │  Pill Row                              ││
│  │  Cards ...                             ││
│  └─────────────────────────────────────────┘│
├─────────────────────────────────────────────┤
│  ... more Scroll Stack Sections ...         │
├─────────────────────────────────────────────┤
│  FOOTER                                     │
│  (border-top, 3-column grid, copyright)     │
└─────────────────────────────────────────────┘
```

The key principle: **the page is a vertical narrative**. Each section occupies the full viewport on desktop and reveals its cards one at a time as the user scrolls, like turning pages. The user always sees exactly one card at a time.

---

## 1. Preloader

The preloader is a full-screen overlay that appears on initial page load.

**Structure:**
```
┌──────────────────────────────────┐
│                                  │
│           onli.cloud             │  ← Centered logo text
│          ━━━━━━━━━━              │  ← Thin progress bar (160px × 1px)
│                                  │
└──────────────────────────────────┘
```

**Behavior:**
- Covers the entire viewport (`z-index: 9999`)
- Background: `#f4f4f5` (near-white warm gray)
- Logo: `48px` Montserrat weight `600`, letter-spacing `-0.04em`
- Progress bar: `160px` wide, `1px` tall, black fill scales from `0` to `1` via `scaleX()`
- **Exit animation**: The entire overlay slides up (`translateY(-100%)`) over `0.8s` with `cubic-bezier(0.65, 0, 0.35, 1)`
- Page content begins rendering underneath while the preloader is active

**CSS file:** `components/preloader.css`

---

## 2. Navigation

The navigation is a non-sticky, two-pill header at the top of the page.

**Structure:**
```
┌──────────────────────────────────────────────────────────────────────────┐
│ ┌─────────────────────────┐  ┌──────────────────────────────────────┐   │
│ │  onli.cloud             │  │  Login                           →   │   │
│ └─────────────────────────┘  └──────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────────┘
```

**Specs:**
- Two pill elements side by side, separated by a small gap
- **Left pill**: Wordmark (site name), text-left aligned
- **Right pill**: CTA or nav links, with optional arrow `→` right-aligned
- Pill height: `44px`
- Pill background: `var(--color-pill-bg)` (`#EFEFEF`)
- Pill radius: `999px`
- Pill shadow: `0 1px 4px rgba(0, 0, 0, 0.07)`
- Padding: `24px` top, `16px` bottom
- `z-index: 10`
- **Not sticky** — scrolls with the page

**Mobile:**
- Left pill (wordmark) remains
- Right pill becomes a hamburger button (`44px × 44px` circle)
- Tapping opens a slide-down drawer with `border-radius: 16px`

**CSS file:** `components/navigation.css`

---

## 3. Hero

The hero section is defined by **deliberate emptiness**. It is mostly whitespace with two small text elements anchored to the edges.

**Structure:**
```
┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│                                                                          │
│                                                                          │
│  let's build something new                              onli.cloud       │
│                                                                          │
│                                                                          │
│                                                                          │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

**Specs:**
- Vertical padding: `30svh` top and bottom (viewport-relative, ~30% of screen height)
- **Left text**: Muted warm gray (`#8c857d`), weight `400`, lowercase tagline
- **Right text**: Near-black (`#171717`), weight `500`, the site name
- Font size: `clamp(24px, 4vw, 40px)` — responsive fluid scaling
- Letter-spacing: `-0.02em`
- Both text elements are on the same horizontal line, pushed to opposite edges via `justify-content: space-between`
- `user-select: none` — text is not selectable (decorative, not content)

**The whitespace is intentional.** The hero should feel like a breath — a calm pause before the content begins. It signals premium quality and quiet confidence.

**Mobile:**
- Padding shrinks to `20svh` top, `10svh` bottom
- Font size scales down: `clamp(16px, 4.5vw, 22px)`
- Text remains on one line but wraps if needed with a `<br class="mobileBr">`

**CSS file:** `components/hero.css`

---

## 4. Scroll Stack Sections

This is the primary content delivery pattern. Each section contains a **pill row** (3 labels) followed by a series of **cards** that stack on top of each other and reveal one at a time on scroll.

### Pill Row

The pill row is the section header. It consists of three pills arranged in the `2fr 3fr 7fr` grid:

```
┌──────────┐  ┌───────────────┐  ┌──────────────────────────────────────────────────┐
│  learn   │  │  onli.cloud   │  │  what is onli                              →     │
└──────────┘  └───────────────┘  └──────────────────────────────────────────────────┘
  Column 1       Column 2                        Column 3
   (2fr)          (3fr)                            (7fr)
```

- **Pill 1** (2fr): Category label (e.g., "learn", "products", "with.onli")
- **Pill 2** (3fr): Platform/product name
- **Pill 3** (7fr): Descriptive label + optional CTA arrow `→`. This pill may be a link.
- Gap between pills: `5px`
- Each pill has the same styling as navigation pills

### Card

Each card fills the viewport height on desktop and uses the same `2fr 3fr 7fr` grid:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│              ┌─────────────────┐  ┌────────────────────────────────────┐ │
│              │  Section Title  │  │                                    │ │
│              │                 │  │                                    │ │
│              │  Body text...   │  │        [  Image 16:9  ]           │ │
│              │  Body text...   │  │                                    │ │
│              │  Body text...   │  │                                    │ │
│              │                 │  │                                    │ │
│              │  ∨ scroll       │  │                                    │ │
│              └─────────────────┘  └────────────────────────────────────┘ │
│                                                                          │
│   (col 1 is empty)                                                       │
└──────────────────────────────────────────────────────────────────────────┘
```

- **Column 1** (2fr): Empty on cards (only used in pill row)
- **Column 2** (3fr): Title + body text
  - Title: `clamp(24px, 2.2vw, 44px)`, weight `500`
  - Body: `clamp(0.875rem, 1vw, 1.05rem)`, weight `300`, color `#6f655c`
  - If text overflows, it becomes scrollable with a thin scrollbar + gradient fade indicator
- **Column 3** (7fr): Image
  - Aspect ratio: `16 / 9`
  - Border radius: `12px`
  - Object fit: `cover`
  - Background placeholder: `#f0f0f0`
- Card height: `100svh` (full viewport) on desktop
- Cards are separated by a `1px solid #e5e5e5` border

### Scroll Behavior (Desktop)

On desktop, cards use GSAP-powered sticky positioning:

1. The first card is visible at `100svh` height
2. As the user scrolls, the next card slides up from below and covers the previous card (curtain-style transition)
3. Each card pins to the viewport for its scroll duration
4. The final card in the section releases the pin, and the next section's pill row scrolls into view
5. This repeats for each section

The scroll distance per card is proportional — typically `100vh` of scroll travel per card.

### Mobile

On mobile, cards stack vertically as standard scrollable content:
- Grid collapses to a single column
- Pills stack in order: Pill 1, Pill 2, Pill 3 → then title → then image
- Cards use `auto` height instead of `100svh`
- No sticky behavior — standard scroll

**CSS file:** `components/scroll-stack.css`

---

## 5. Footer

The footer is separated from the content by a horizontal border.

**Structure:**
```
─────────────────────────────────────────────────────────────
  Contact              Legal              onli.ai
  hello@...            Privacy Policy     onli.cloud
                                          onli.one
                                          onli.you
                                          withonli.com


              © 2025 The Onli Corporation. All rights reserved.
```

**Specs:**
- `border-top: 1px solid #e5e5e5`
- `margin-top: 35px` (small breathing room after last section)
- 3-column grid on desktop, 2-column on tablet, 1-column on mobile
- Column headers: `13px`, weight `500`, near-black
- Links: `13px`, weight `400`, muted gray (`#A3A3A3`), hover → near-black
- Copyright row: Centered, `margin-top: 80px`, `12px` text, `#D4D4D4`
- Contact email is a `mailto:` link
- Links to other Onli properties are external links (`target="_blank"`)

**CSS file:** `components/footer.css`

---

## The Grid System

The entire site uses a consistent **`2fr 3fr 7fr`** column architecture:

```
│◄── 2fr ──►│◄─── 3fr ───►│◄──────────── 7fr ──────────────►│
│            │              │                                  │
│  Label     │  Title/Text  │  Image / Content                │
│  Pill 1    │  Pill 2      │  Pill 3 (CTA)                   │
```

**Proportions:**
- Column 1 (`2fr`): ~16.7% — Labels, category pills, small metadata
- Column 2 (`3fr`): ~25% — Headlines, body text, descriptions
- Column 3 (`7fr`): ~58.3% — Hero images, feature visuals, primary content

**Gap:** `5px` between columns, `24px` between rows.

**Container:** Max-width `1800px`, centered with `auto` margins.

**Gutters (page edge padding):**
- Mobile (`< 768px`): `24px`
- Tablet (`768px – 1199px`): `80px`
- Desktop (`≥ 1200px`): `32px`

**CSS file:** `tokens/grid.css`

---

## Responsive Behavior

### Breakpoints

| Name | Range | Layout |
|---|---|---|
| Mobile | `< 768px` | Single column, stacked content, hamburger nav |
| Tablet | `768px – 1199px` | 3-column grid, auto-height cards, no sticky |
| Desktop | `≥ 1200px` | 3-column grid, `100svh` cards, sticky scroll |

### What Changes at Each Breakpoint

**Mobile:**
- Nav: Wordmark pill + hamburger button
- Hero: Reduced padding (`20svh` / `10svh`), smaller text
- Cards: Stacked vertically — pills first (all 3), then title, then image
- Grid: Collapses to single column
- Footer: Single column

**Tablet:**
- Nav: Full two-pill layout
- Hero: Same as desktop
- Cards: 3-column grid but `auto` height (no sticky)
- Footer: 2-column grid

**Desktop:**
- Nav: Full two-pill layout
- Hero: `30svh` padding, fluid text
- Cards: Full-viewport sticky cards with scroll-driven transitions
- Footer: 3-column grid

---

## Animation System

### Entry Animations

Elements with the `.animate-in` class start invisible and offset:
```css
opacity: 0;
transform: translateY(20px);
```
GSAP or IntersectionObserver triggers them to `opacity: 1; transform: translateY(0)` on scroll-into-view.

### Scroll Stack Transitions

Cards transition using a **curtain slide-up** effect:
- The incoming card slides up from the bottom of the viewport to cover the current card
- No opacity fade — the transition is rigid and clean
- Driven by GSAP ScrollTrigger with `pin: true` on each card
- Transition easing: `cubic-bezier(0.19, 1, 0.22, 1)` (expressive ease-out)

### Micro-interactions

| Element | Hover State |
|---|---|
| Pill links | `opacity: 0.8` + `translateX(4px)` shift |
| Image links | `opacity: 0.92` |
| Nav links | `opacity: 0.6` |
| Footer links | Color change to `--color-text-dark` |
| Scroll indicator | `caretBounce` animation — chevron bounces `5px` every `1.6s` |

### Reduced Motion

All animations respect `prefers-reduced-motion: reduce`:
- Durations collapse to `0.01ms`
- `.animate-in` elements render immediately at full opacity
- Scroll indicator stops bouncing

---

## Branded Sites Reference

| Site | URL | Primary Use | Unique Features |
|---|---|---|---|
| **onli.cloud** | [onli.cloud](https://onli.cloud) | Developer platform / API console | Login CTA, product-focused cards |
| **onli.one** | [onli.one](https://onli.one) | Protocol & network docs | Scroll-stack with GSAP mask transitions |
| **withonli.com** | [withonli.com](https://withonli.com) | Marketing / brand storytelling | Blog entries, extended card content |

All three follow the same structural pattern: **Preloader → Nav → Hero → Scroll Stack Sections → Footer**. Content varies, but the architecture is identical.

---

## For Developers / Agents

To build a new Onli-branded site:

1. **Import `globals.css`** — gets you all tokens, the reset, and Safari fixes
2. **Copy the page skeleton** from above — Preloader → Nav → Hero → Sections → Footer
3. **Use the component CSS files** as your starting point for each section
4. **Follow the grid** — `2fr 3fr 7fr` for all content, `1800px` max-width
5. **Typography is Montserrat** — load via Google Fonts or `next/font/google`
6. **Use the spacing scale** — `--space-1` through `--space-10`, 8px base
7. **Keep it minimal** — warm neutrals, generous whitespace, no loud colors
8. **Scroll stack is the hero feature** — invest time in the sticky card experience
9. **Test on Safari/iOS** — viewport units, sticky positioning, and `-webkit-fill-available` all need attention
10. **Respect reduced motion** — all animations must degrade gracefully
