# Singulink C# Coding Standards and Conventions

Standard .NET conventions apply to anything not specified below. Conventions specified as `must` are required to be follow at all times. Conventions specified as `should` can be violated if there is good justification to do so.

## Comments

### Library Code

- Summary comments (`/// <summary></summary>`) must be provided on public types and members.
- Comments on method parameters are optional, but must be provided if their correct usage is not obvious.
- Comments on return values are optional, but must be provided if their correct usage is not obvious.
- Comments on `internal` or `private` types and members are optional, but must be provided if their correct usage is not obvious.

### Application Code

- Comments on types and members are optional, but must be provided if their correct usage is not obvious.

## Type Member Ordering

The following is the order that member should appear inside types. Each category of members below should be primarily ordered in descending order of accessibility, followed by the ordering specified in the parenthesis:

1. Constants and constant-like fields (logical grouping and order)
2. Static fields (logical grouping and order)
3. Instance fields (logical grouping and order)
4. Delegate types (logical grouping and order)
5. Events (logical grouping and order)
6. Static Properties, if any auto-properties are present (alphabetical order)
7. Instance Properties, if any auto-properties are present (alphabetical order)
8. Constructors (logical grouping and order)
   - Should flow in the order constructors are chained, and more specific constructors with more parameters should appear below less specific constructors.
9. Static Properties, if auto-properties are NOT present (alphabetical order)
10. Instance Properties, if auto-properties are NOT present (alphabetical order)
11. Non-private static methods (alphabetical order)
12. Non-private instance methods (alphabetical order)
13. Private instance methods (alphabetical order)
14. Private static methods (alphabetical order)
15. Explicit interface implementation properties (alphabetical order)
16. Explicit interface implementation methods (alphabetical order)
17. Explicit interface implementation properties that throw `NotSupportedException` (alphabetical order)
18. Explicit interface implementation methods that throw `NotSupportedException` (alphabetical order)
19. Nested types (logical order)
    - Move into separate `TypeName.NestedTypeName.cs` files if `TypeName.cs` content is long.

## Naming

### Fields

- Constants, `static readonly string` fields, and other "Constant-like" `static readonly` fields (i.e. `static readonly ImmutableArray<>`) must be PascalCased.
- Other `static` fields must be named `s_fieldName`.
- Instance fields must be named `_fieldName`.

### Native Interop Members

- Internal native interop enums and constants can violate naming rules to match how they are named in the native API, i.e. constants and enum names may be named `SOME_CONSTANT_VALUE` if that is how they are defined in the native API.

### Async Methods

- Methods that return `Task`, `ValueTask`, or other awaitable types must be named with an `Async` suffix.
- Async methods that return `void` must NOT be named with an `Async` suffix.
- Do not fire-and-forget methods that return awaitable types. Use the `AmbientTasks` library (https://github.com/jnm2/AmbientTasks) to facilitate fire-and-forget behavior with proper exception tracking/handling.
- Async methods that return `void` should be avoided. For event handlers or other `void` returning methods that contain async behavior, put the async behavior inside a nested async method that returns a `Task` and follow the guidance above regarding fire-and-forget behavior.

## Accessibility

### General

- Members must have the minimum amount of accessibility required to facilitate their functionality and proper usage, aside from the exceptions specified below for top-level application code.
- Do not add property setters if they are not required. Prefer get-only properties.
- Fields should be `private`. If `internal` access is required, it should be through a property.
- Nested methods should be used if they are intended to be only called from one other method.

### Top-level application code

Top-level application code is all the code in the top-level project that creates the executable file, i.e. the main ASP.NET, console or UI project.

- Members should not be marked as `internal`. Use `public` instead of `internal`.
- All types should be `public`.

## Nullable Reference Types

- NRTs must be enabled.
- Our `RuntimeNullables` package must be used to inject runtime parameter null checks based on NRT annotations. Settings should be left at their defaults.

## Miscellaneous

- Code should be written in the most concise form that does not negatively affect readability:
  - Prefer `condition ? x : y` over `if / else`.
  - Single line get-only properties should be writted as `public Type PropertyName => ...;`
  - Single line property accessors should be written as `get => ...;` and `set => ...;`
  - Single line methods should be written as `public ReturnType MethodName() => ...;`
  - Single line `if`/`else if`/`else` blocks do not need curly braces (`{ }`).
  - Use `someExpression switch { }` where appropriate
  - Use pattern matching where appropriate.
- Do not use `this.` to access type members.
- Do not use `private` auto-properties. Use fields instead.
- Instance auto-properties and fields should generally not be mixed. If a type has fields, then use full properties instead of auto-properties.
- Prefer accessing values through their backing fields instead of their properties, unless the property provides additional functionality that is needed.
