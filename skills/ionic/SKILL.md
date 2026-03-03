---
name: ionic
description: Ionic framework usage guidelines including CLI generation, Ionicons, and styling conventions. Use when working with Ionic components, pages, or generating Ionic artifacts.
---

# Ionic

---

## Ionic Usage

- Generate components/pages/services with `@ionic/cli`
- Remove empty constructors and unused lifecycle hooks
- Import Ionicons via `addIcons` in the component constructor

---

## Styling

- No custom CSS without explicit approval
- Use default Ionic components with their built-in props
- Use Ionic CSS utility classes for layout and spacing
