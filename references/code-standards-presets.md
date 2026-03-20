# Code Standards Presets

When generating `docs/L1/code-standards.md` for a fresh project, use these presets
based on the project's tech stack. These are starting points — the user will customize.

## Detection and selection

Ask about the tech stack in Step 2. Match to the closest preset. If the stack spans
multiple presets (e.g., TypeScript frontend + Python backend), merge relevant sections.

## TypeScript / JavaScript

```markdown
## Naming conventions
| Object | Rule | Example |
|--------|------|---------|
| Files | kebab-case | user-service.ts |
| Functions | camelCase | getUserById |
| Classes | PascalCase | UserService |
| Constants | UPPER_SNAKE_CASE | MAX_RETRY_COUNT |
| Components | PascalCase | UserProfileCard |
| Types/Interfaces | PascalCase with I- prefix for interfaces (optional) | UserData, IUserService |

## File organization
- One exported class/component per file
- Index files for module public API
- Types in dedicated types.ts or types/ directory
- Utils in shared/utils/

## Error handling
- All async operations wrapped in try/catch
- Use custom error classes extending Error
- Never swallow errors silently
- User-facing error messages must not expose internals

## Forbidden patterns
- No `any` type (use `unknown` if type is truly unknown)
- No direct DOM manipulation in framework components
- No business logic in API route handlers (use service layer)
- No relative imports crossing module boundaries (use path aliases)
```

## Python

```markdown
## Naming conventions
| Object | Rule | Example |
|--------|------|---------|
| Files | snake_case | user_service.py |
| Functions | snake_case | get_user_by_id |
| Classes | PascalCase | UserService |
| Constants | UPPER_SNAKE_CASE | MAX_RETRY_COUNT |
| Private methods | _leading_underscore | _validate_input |

## File organization
- One class per file for large classes
- __init__.py for package public API
- Type hints on all public functions
- Separate models/, services/, routes/ directories

## Error handling
- Use custom exception hierarchy
- Always log before re-raising
- Context managers for resource cleanup
- Never bare except:

## Forbidden patterns
- No mutable default arguments
- No star imports (from x import *)
- No global mutable state
- No print() for logging (use logging module)
```

## Go

```markdown
## Naming conventions
| Object | Rule | Example |
|--------|------|---------|
| Files | snake_case | user_service.go |
| Exported functions | PascalCase | GetUserByID |
| Unexported functions | camelCase | validateInput |
| Packages | lowercase, single word | auth, user |
| Interfaces | -er suffix when sensible | Reader, UserFinder |

## File organization
- Group by domain, not by layer
- One package per directory
- _test.go files alongside source
- internal/ for unexported packages

## Error handling
- Always check returned errors
- Wrap errors with context: fmt.Errorf("doing X: %w", err)
- Use sentinel errors for expected conditions
- Never panic in library code

## Forbidden patterns
- No init() functions (except for driver registration)
- No package-level mutable variables
- No empty interface{} without documentation of expected types
```

## Rust

```markdown
## Naming conventions
| Object | Rule | Example |
|--------|------|---------|
| Files | snake_case | user_service.rs |
| Functions | snake_case | get_user_by_id |
| Types/Structs | PascalCase | UserService |
| Constants | UPPER_SNAKE_CASE | MAX_RETRY_COUNT |
| Modules | snake_case | auth, user_management |

## Error handling
- Use thiserror for library errors, anyhow for application errors
- Never unwrap() in production code (use expect() with message or proper error handling)
- Result<T, E> as return type, not panics

## Forbidden patterns
- No unwrap() without justification comment
- No unsafe blocks without safety documentation
- No clone() to satisfy borrow checker without performance consideration
```

## Fallback (unknown stack)

If the tech stack doesn't match any preset, generate a minimal code-standards.md with:
- Generic naming conventions (ask user for preference: camelCase vs snake_case)
- Basic file organization principles
- Error handling philosophy (fail explicitly, log context, don't swallow)
- Leave "Forbidden patterns" section for the user to fill in
