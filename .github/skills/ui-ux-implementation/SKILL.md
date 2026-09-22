---
name: ui-ux-implementation
description: "Design, implement, review, and polish responsive web interfaces with strong visual hierarchy, accessibility, interaction quality, typography, color, motion, and screenshot-based verification. Use for UI styling, UI/UX reviews, responsive layouts, design-to-code work, visual matching, and frontend interaction fixes. Detect the project stack first and preserve existing tooling."
argument-hint: "[interface, component, or visual issue]"
user-invocable: true
---

# UI/UX Implementation

Create interfaces that are intentional, accessible, responsive, and visually verified. Treat supplied designs and screenshots as references to implement, not as a reason to replace the page with a flattened screenshot.

## Stack Guardrails

1. Inspect the repository before choosing implementation tools.
2. If the project is plain HTML/CSS/JavaScript, use semantic HTML and local CSS. Do not add Node.js, React, Tailwind, shadcn/ui, Radix, or build tooling unless explicitly requested.
3. If an existing framework or design system is present, follow its components, tokens, conventions, and build commands.
4. Use shadcn/ui or Tailwind only when the project already uses them or the user explicitly asks for them.
5. Preserve existing assets, public APIs, file structure, and unrelated user changes.

## Workflow

### 1. Understand the Target

Identify:

- The page, component, layer, or behavior being changed.
- The target audience and primary task.
- The reference source: Figma node, screenshot, existing implementation, or written requirements.
- The current rendering constraints: viewport sizes, content density, device behavior, and browser support.

State one local hypothesis about the controlling code path and one cheap check that can disconfirm it before editing.

### 2. Inspect the Existing Implementation

Read the owning HTML/component and its nearby styles. Check:

- Existing layout model and positioning context.
- Asset paths, intrinsic dimensions, and transparent/opaque backgrounds.
- Font loading and fallback behavior.
- Breakpoints, viewport metadata, overflow, and fixed dimensions.
- Existing animation, focus, hover, and reduced-motion behavior.

Prefer the smallest edit at the closest controlling abstraction.

### 3. Build the Visual System

Use deliberate tokens for:

- Background, surface, text, border, accent, and focus colors.
- Spacing, radius, shadows, and z-index layers.
- Typography family, weight, size, line-height, and letter spacing.
- Motion duration, easing, and reduced-motion fallback.

Avoid default-looking purple-on-white layouts, excessive rounded cards, decorative blobs, illegible low-contrast text, and layouts whose text or controls overlap at smaller widths.

Use expressive typography appropriate to the domain. Do not use viewport-scaled font sizes for general text; use responsive constraints that keep text readable and contained.

### 4. Implement Semantically

Use real HTML elements:

- `button` for actions.
- Links for navigation.
- Headings and landmarks for structure.
- Labels for form controls.
- `alt=""` only for genuinely decorative images.

For image-based references, reproduce the structure with editable layers whenever practical. Use an exported image only when the user explicitly requests a static image or the source is inherently a single artwork asset.

Keep touch targets at least 44x44 CSS pixels where practical, with clear spacing. Never remove visible keyboard focus without providing an equivalent focus treatment.

### 5. Add Motion Carefully

Use a small number of purposeful animations that communicate state or draw attention to an intended action. Prefer transform and opacity over layout properties. Define hover, focus-visible, active, and disabled states for interactive elements.

Always include a reduced-motion fallback:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### 6. Validate Visually and Behaviorally

After each substantive edit, run the narrowest available validation before further exploration.

Required checks when browser tooling is available:

1. Render the page at a representative mobile viewport.
2. Render it at a representative desktop viewport.
3. Capture screenshots of the full target surface and important controls.
4. Inspect computed dimensions, image `complete` state, `naturalWidth`, and relevant CSS properties.
5. Check that text does not overlap, overflow, or become unreadable.
6. Exercise keyboard focus and primary interactions.
7. Check `prefers-reduced-motion` behavior when animation is present.

Compare screenshots against the reference for alignment, scale, cropping, color, typography, and whitespace. Fix only the local defect exposed by the check, then repeat the focused check.

### 7. Accessibility and Quality Gate

Before delivery, confirm:

- Contrast is sufficient for text and controls.
- All meaningful images have useful alternative text.
- Decorative images are hidden from assistive technology.
- Interactive elements are keyboard reachable and visibly focused.
- Touch targets are usable on small screens.
- Motion has a reduced-motion fallback.
- No horizontal scroll appears at supported widths.
- Loading states reserve layout space.
- No console, HTML, CSS, type, or lint errors were introduced.
- The implementation remains editable rather than being a flattened screenshot.

## Design-to-Code Notes

For Figma work, use the exact node or frame named by the user. Confirm the node name, dimensions, and parent before implementing. Do not modify the Figma file unless explicitly requested. Use Figma screenshots and metadata as reference evidence, then adapt the result to the existing codebase and asset pipeline.

For asset issues, verify filenames and relative paths exactly, including spaces and case. Inspect the source image itself before applying blend modes or filters. Preserve texture when removing a tint: prefer desaturation, neutral color treatment, masking, or a properly isolated transparent asset over simply erasing the entire layer.

## Completion Report

Report briefly:

- Files changed and the behavior implemented.
- Visual and executable checks performed.
- Any remaining limitation, such as an unavailable browser, missing reference, or asset that is baked into a raster image.
