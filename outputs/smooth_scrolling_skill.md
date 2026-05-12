# Skill: Smooth Scrolling Implementation

## Objective
Enable fluid, animated transitions when navigating between internal sections of a single-page application (SPA) using anchor links (`#section-id`). This prevents jarring "cuts" and enhances the premium feel (Vibe) of the interface.

## Implementation Methods

### 1. TailwindCSS (Recommended)
If using TailwindCSS, add the `scroll-smooth` class to the `html` element.
```html
<html class="scroll-smooth">
```

### 2. Vanilla CSS
Add the `scroll-behavior` property to the `html` or `body` selector.
```css
html {
  scroll-behavior: smooth;
}
```

## Why use this?
- **User Experience**: Provides visual context of where the user is moving within the page.
- **Solidity**: Standard CSS approach with wide browser support.
- **Aesthetics**: Aligns with modern, premium web design standards by eliminating instant jumps.

## Atomic Vibe Integration
- Ensure all sections have unique IDs.
- Ensure navbar links use the corresponding `#ID`.
- If the layout has a fixed header, consider adding `scroll-padding-top` to avoid the header overlapping the section content.

```css
html {
  scroll-behavior: smooth;
  scroll-padding-top: 80px; /* Adjust based on navbar height */
}
```
