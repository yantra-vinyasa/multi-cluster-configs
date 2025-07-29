# DRY and WET Principles

## DRY - Don't Repeat Yourself

The DRY principle aims to reduce repetition. It states: "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

**Benefits:**
- **Maintainability:** Change in one place.
- **Readability:** Cleaner code.
- **Reusability:** Encourages reusable components.
- **Fewer Bugs:** Less code, fewer bugs.

## WET - Write Everything Twice

WET describes code with repetitive patterns. It's an anti-pattern that can lead to:

- **Difficult maintenance:** Changes needed in multiple places.
- **More bugs:** Easy to miss a spot when updating.
- **Bloated code:** More to read and manage.

Sometimes, a little repetition is acceptable to avoid **overly complex abstractions**, but it should be refactored when a clear pattern emerges.