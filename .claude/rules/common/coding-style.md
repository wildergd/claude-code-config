# Code Quality Standards

## Formatting and Linting
- Prettier handles formatting — don't adjust style manually
- ESLint errors must be resolved before committing (`yarn run lint`)

## Type Safety
- No `any` — use `unknown` and narrow, or define the type explicitly
- Prefer explicit return types on public functions and hooks

## Error Handling
- All async operations must have explicit error handling
- Surface typed errors — avoid throwing raw strings
- UI errors → error boundaries; async errors → typed error responses

## Code Organisation
- One clear responsibility per file (see SOLID rules [solid-principles.md](solid-principles.md))
- Extract shared logic to utilities or services — not into components
- No hardcoded configuration — use environment variables

## Documentation
- No inline comments for logic that can be self-explanatory

## Testing
- See testing rules [testing.md](testing.md)