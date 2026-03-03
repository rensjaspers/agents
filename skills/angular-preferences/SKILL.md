---
name: angular-preferences
description: Personal Angular development preferences and guidelines. Use alongside other Angular skills to apply opinionated patterns for MCP server workflow, naming conventions, signals/RxJS usage, forms handling, and data loading strategy selection.
---

# Angular Preferences

Personal preferences and constraints to apply when working with Angular.

## Angular MCP Server

Always use the Angular MCP server instead of direct shell commands.

**Workflow:**

1. `get_best_practices` with `workspacePath` — before writing code
2. `search_documentation` — for conceptual questions

## Modern Angular Constraints

- Prefer signals for inputs, outputs, `viewChild` — avoid legacy decorators
- Avoid lifecycle hooks (`ngOnInit`, `ngOnDestroy`) — signals and field initializers usually suffice
- Subscribe in constructor with cleanup:

  ```ts
  obs$.pipe(takeUntilDestroyed()).subscribe();
  ```

## Naming Conventions

**No type suffixes** — describe responsibility, not technical type:

- ❌ `UserService` → ✅ `UserRepository`, `UserStore`, `Authentication`
- ❌ `ModalComponent` → ✅ `Modal`, `Dialog`

## Signals & RxJS Interop

- **Never use `effect()`** without explicit approval
- Don't convert RxJS to signals too early — convert at the end for templates
- `computed()` is synchronous only — not for async streams
- `toSignal()` only in injection context (causes immediate subscription)
- Check latest Angular docs before working with signals/RxJS interop

## Forms

Additional patterns for form handling (see `angular-forms` skill for complete API):

### Validation Rules

- Use Angular's built-in form validation — do not implement validation logic outside of Angular forms
- Never disable the submit button based on `form.invalid`
- The submit button must always be enabled
- Never manually track validation state with custom signals or variables — derive it from the form

### Submit Handling Pattern

On submit:

1. Call `form.markAllAsTouched()` so validation messages appear in the UI
2. Check `form.invalid` — if invalid, return early
3. Proceed with the actual submit logic

```ts
onSubmit() {
  this.form.markAllAsTouched();
  if (this.form.invalid) {
    return;
  }
  // submit logic
}
```

## Data Loading

Strategy selection for data fetching (see `angular-http` skill for implementation):

### Use TanStack Query when:
- You need advanced caching strategies
- You want automatic background refetching
- You need complex query dependencies
- You want optimistic updates
- You need request deduplication

### Use `rxResource` when:
- You're already using RxJS observables
- You need custom retry/error handling logic
- You want fine-grained control over the request lifecycle
- You're integrating with existing RxJS streams

### Use `resource` when:
- You have simple data fetching needs
- You want the simplest possible API
- You're using signals throughout your component
- You don't need advanced caching or retries

**General rule:** For simple cases, prefer `resource` for its simplicity. For complex state management, TanStack Query is most powerful.

## Before Finishing

- Successful production (AOT) build without warnings
