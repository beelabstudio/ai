# Code Style Rules

## General Principles

- Write clear, readable code
- Prefer explicit over implicit
- Follow the principle of least surprise

## Naming Conventions

- Files: kebab-case (e.g., `my-component.tsx`)
- Components: PascalCase (e.g., `MyComponent`)
- Functions: camelCase (e.g., `myFunction`)
- Constants: UPPER_SNAKE_CASE (e.g., `API_BASE_URL`)

## TypeScript

- Use strict mode
- Prefer `interface` over `type` for object shapes
- Avoid `any` - use `unknown` if necessary
- Enable `noImplicitAny` and `strictNullChecks`

## Formatting

- Indent: 2 spaces
- Max line length: 100 characters
- Use trailing commas in multiline
- Semicolons: required

## Comments

- Explain why, not what
- Use JSDoc for public APIs
- Keep comments up-to-date with code
