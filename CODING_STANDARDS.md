# Coding standards

## Correct by construction

- **Make illegal states unrepresentable**: shape your types so that only valid
  combinations of data can be constructed.
- **Parse, don't validate**: a check that returns nothing forces every later
  caller to re-handle a case that's already been ruled out, so have it return a
  narrower type carrying what it learned, and do it once at the system boundary.

## Functional core, imperative shell

Decisions live in pure functions; I/O, time, randomness and UI live in a thin
layer that calls them.
