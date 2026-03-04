---
name: unit-testing
description: Write focused unit tests for complex logic, pure functions, and business rules. Use for testing functions with clear inputs and outputs. Does NOT cover UI, templates, or component rendering — those are out of scope.
---

# Unit Testing

Write fast, lightweight tests that catch logic bugs. Tests should be easy to read, easy to maintain, and run in milliseconds.

## Scope

**Test:**
- Pure functions and utility methods
- Business logic with non-trivial branching or computation
- Transformations, mappings, and derivations
- Edge cases, boundary conditions, and error paths
- Computed/derived state that encapsulates logic

**Do not test:**
- Whether a UI element is rendered or visible
- Template control flow (`@if`, `@for`, `@switch`)
- Whether an array of configuration items contains a specific entry
- Component inputs/outputs triggering DOM changes
- CSS classes, styles, or layout

If a test needs `TestBed`, `ComponentFixture`, or DOM queries — stop. That is a UI test, not a logic test.

## Principles

- One test, one behavior. Name it after the behavior: `should return 0 when list is empty`.
- No shared mutable state between tests. Use `beforeEach` to reset if needed.
- Avoid mocking unless the dependency is I/O (HTTP, storage). Logic-only dependencies should be called as-is.
- If a function is hard to test, the function needs to be simplified — but don't change production code; flag it.

## Structure

```typescript
describe('functionName', () => {
  it('should [expected behavior] when [condition]', () => {
    // Arrange
    const input = ...;

    // Act
    const result = functionName(input);

    // Assert
    expect(result).toEqual(...);
  });
});
```

## Good test examples

```typescript
// Pure function
describe('calculateDiscount', () => {
  it('should return 10% discount for orders above 100', () => {
    expect(calculateDiscount(120)).toBe(12);
  });

  it('should return 0 for orders at or below 100', () => {
    expect(calculateDiscount(100)).toBe(0);
    expect(calculateDiscount(50)).toBe(0);
  });

  it('should return 0 for negative input', () => {
    expect(calculateDiscount(-10)).toBe(0);
  });
});
```

```typescript
// Derived/computed logic without UI
describe('filterActiveItems', () => {
  it('should return only active items', () => {
    const items = [
      { id: 1, active: true },
      { id: 2, active: false },
      { id: 3, active: true },
    ];
    expect(filterActiveItems(items)).toEqual([{ id: 1, active: true }, { id: 3, active: true }]);
  });

  it('should return empty array when no items are active', () => {
    expect(filterActiveItems([{ id: 1, active: false }])).toEqual([]);
  });

  it('should return empty array for empty input', () => {
    expect(filterActiveItems([])).toEqual([]);
  });
});
```

## What not to test

```typescript
// BAD — tests UI rendering, not logic
it('should show the discount label', () => {
  fixture.detectChanges();
  expect(fixture.nativeElement.querySelector('.discount')).toBeTruthy();
});

// BAD — tests configuration data, not logic
it('should contain the settings menu item', () => {
  expect(menuItems).toContain(jasmine.objectContaining({ label: 'Settings' }));
});

// BAD — tests template @if, not logic
it('should hide the form when loading', () => {
  component.loading.set(true);
  fixture.detectChanges();
  expect(fixture.nativeElement.querySelector('form')).toBeNull();
});
```
