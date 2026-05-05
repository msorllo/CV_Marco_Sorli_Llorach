---
name: The Design System
colors:
  surface: '#0b141c'
  surface-dim: '#0b141c'
  surface-bright: '#313a43'
  surface-container-lowest: '#060f16'
  surface-container-low: '#141c24'
  surface-container: '#182028'
  surface-container-high: '#222b33'
  surface-container-highest: '#2d363e'
  on-surface: '#dae3ee'
  on-surface-variant: '#b9caca'
  inverse-surface: '#dae3ee'
  inverse-on-surface: '#29313a'
  outline: '#849494'
  outline-variant: '#3a4a4a'
  surface-tint: '#00dce3'
  primary: '#efffff'
  on-primary: '#003739'
  primary-container: '#00f7ff'
  on-primary-container: '#006d71'
  inverse-primary: '#00696d'
  secondary: '#d1bcff'
  on-secondary: '#3c0090'
  secondary-container: '#7000ff'
  on-secondary-container: '#ddcdff'
  tertiary: '#fcfbff'
  on-tertiary: '#2d3137'
  tertiary-container: '#dcdfe8'
  on-tertiary-container: '#5f626a'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#54f8ff'
  primary-fixed-dim: '#00dce3'
  on-primary-fixed: '#002021'
  on-primary-fixed-variant: '#004f52'
  secondary-fixed: '#e9ddff'
  secondary-fixed-dim: '#d1bcff'
  on-secondary-fixed: '#23005b'
  on-secondary-fixed-variant: '#5700c9'
  tertiary-fixed: '#dfe2eb'
  tertiary-fixed-dim: '#c3c6cf'
  on-tertiary-fixed: '#181c22'
  on-tertiary-fixed-variant: '#43474e'
  background: '#0b141c'
  on-background: '#dae3ee'
  surface-variant: '#2d363e'
typography:
  headline-xl:
    fontFamily: Space Grotesk
    fontSize: 48px
    fontWeight: '700'
    lineHeight: '1.1'
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Space Grotesk
    fontSize: 24px
    fontWeight: '600'
    lineHeight: '1.2'
  body-lg:
    fontFamily: Inter
    fontSize: 18px
    fontWeight: '400'
    lineHeight: '1.6'
  body-sm:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.5'
  terminal-code:
    fontFamily: JetBrains Mono, Fira Code, monospace
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.4'
    letterSpacing: 0.02em
  label-caps:
    fontFamily: JetBrains Mono, monospace
    fontSize: 12px
    fontWeight: '700'
    lineHeight: '1'
    letterSpacing: 0.1em
spacing:
  unit: 4px
  gutter: 24px
  margin: 32px
  container-max: 1440px
---

## Brand & Style

The design system is engineered to evoke a sense of clandestine authority and high-performance precision. It draws from the **Cyber-Brutalist** and **Glassmorphic** movements, prioritizing information density and technical sophistication. The aesthetic is tailored for an audience that values digital sovereignty—developers, security researchers, and tech enthusiasts.

The visual language communicates a "Command and Control" atmosphere. It balances the raw, unrefined energy of a terminal interface with the premium finish of modern high-tech software. The emotional response is one of immersion: the user should feel as though they are interfacing directly with a secure, powerful core.

## Colors

This design system utilizes a high-contrast, dark-mode-first palette. The foundation is built upon a deep "void" background to minimize eye strain and maximize the "bloom" effect of the neon accents.

- **Background:** The primary canvas is `#0d1117`, a sophisticated deep grey-black that provides more depth than pure black.
- **Neon Cyan (#00f7ff):** Used for primary actions, success states, and data visualizations. It represents the "active pulse" of the system.
- **Dark Purple (#7000ff):** Used for secondary highlights, decorative gradients, and rare "ultra-secure" states.
- **Accents:** Neon glows are achieved using semi-transparent versions of the primary and secondary colors (10–20% opacity) layered as box shadows or background blurs.

## Typography

The typography strategy leverages a dual-font approach to balance readability with a "hacker" aesthetic.

1. **Functional/Body:** **Inter** is the workhorse font, used for all long-form text, descriptions, and settings to ensure clarity and professional utility.
2. **Technical/Accents:** **Space Grotesk** is used for headlines to provide a geometric, futuristic edge. For terminal windows, data readouts, and small labels, a **Monospace** stack (ideally JetBrains Mono) is mandatory to reinforce the cybersecurity theme.
3. **Styling:** Use uppercase transformations for labels and small navigational elements to mimic military-grade hardware interfaces.

## Layout & Spacing

The layout philosophy is based on a **Fixed Grid** with high density. The system utilizes an 8px spatial rhythm, but allows for 4px increments for fine-tuned technical components like terminal inputs.

- **Grid:** A standard 12-column grid is used for marketing and dashboard pages. 
- **Density:** High. Information should be packed efficiently, separated by thin borders rather than excessive whitespace.
- **Layout Model:** Use "Shell-based" layouts. The primary navigation is often a slim vertical sidebar on the left, with a terminal-style breadcrumb/status bar at the top or bottom of the viewport.

## Elevation & Depth

In this design system, depth is not conveyed through shadows, but through **Tonal Layering** and **Luminescence**.

- **Layers:** Surface elevations are created by lightening the background hex slightly (e.g., from `#0d1117` to `#161b22`).
- **Glow Effects:** Interactive elements use an "Outer Glow" (box-shadow) utilizing the primary cyan or secondary purple. The blur should be soft (15px-30px) and the spread should be minimal.
- **Glassmorphism:** Use `backdrop-filter: blur(12px)` for terminal headers and floating modals, creating a "frosted glass over a dark server room" effect.
- **Borders:** Use 1px solid borders in a slightly lighter grey or low-opacity Cyan to define boundaries.

## Shapes

The shape language is strictly **Sharp (0px)** for primary containers to maintain a brutalist, technical feel.

- **Terminal Windows:** Must have 0px border-radius.
- **Buttons:** 0px radius, or a maximum of 2px for subtle "softening" on smaller mobile touch targets.
- **Exceptions:** Status pips and specific circular data visualizations are the only rounded elements permitted.
- **Angled Cuts:** Decorative corners may use a 45-degree chamfer (clipped corner) to emphasize the "stealth" military aesthetic.

## Components

### Terminal Windows
The centerpiece of the design system. Headers feature a distinct background color (Dark Purple or Medium Grey), "Window Control" pips (Close/Min/Max) in a monochrome style, and a Monospace title. The body uses a black background with a subtle "scanline" overlay pattern.

### Glowing Cards
Cards feature a 1px border. On hover, the border transitions to Neon Cyan, and a subtle Cyan outer glow is applied. The card content may slightly "glitch" (a quick 2px horizontal offset) during the hover transition.

### Neon Vertical Timelines
Vertical lines are 1px wide, using a Cyan-to-Purple gradient. Timeline nodes are small 8px squares. Active nodes have a "pulse" animation (a scaling glow effect).

### Buttons
- **Primary:** Solid Cyan background, black text (Inter Bold), no radius.
- **Secondary/Ghost:** 1px Cyan border, Cyan text, no background. 
- **Glitch Effect:** On hover, primary buttons should have a "split" color effect where a magenta and cyan shadow appear on opposite sides briefly.

### Input Fields
Inputs are styled as "Console Prompts." They feature a leading `>` character in Cyan. There is no bottom border or box; only a blinking underscore cursor at the end of the active text string.

### Progress Bars
Segmented bars (divided into blocks) rather than a smooth continuous line, evoking retro hardware loading sequences.