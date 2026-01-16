# TypeScript Coding Standards

## Type Safety

- Enable `strict` mode in tsconfig.json
- Avoid `any` - use `unknown` and type guards instead
- Prefer interfaces for object shapes, types for unions/intersections
- Use generics to maintain type information through transformations

## Naming Conventions

- PascalCase: Types, interfaces, classes, enums
- camelCase: Variables, functions, methods, properties
- UPPER_SNAKE_CASE: Constants and enum values
- Prefix interfaces with `I` only if needed to distinguish from classes

## Best Practices

- Use `const` by default, `let` when reassignment needed
- Prefer readonly properties and ReadonlyArray when mutation isn't needed
- Use optional chaining (`?.`) and nullish coalescing (`??`)
- Destructure objects and arrays for cleaner code
- Use async/await over raw promises

## Functions

- Use arrow functions for callbacks and simple expressions
- Use function declarations for hoisting and recursion
- Always specify return types for public APIs
- Use parameter destructuring for options objects

## Imports

- Use named exports over default exports
- Group imports: external deps, internal modules, relative imports
- Use type-only imports when importing only types: `import type { Foo }`

## Error Handling

- Create custom error classes for domain errors
- Never swallow errors silently
- Use Result types for expected failures
- Reserve exceptions for unexpected failures
