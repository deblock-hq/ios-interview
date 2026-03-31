Review the current staged changes (run `git diff --cached`). If nothing is staged, review all uncommitted changes (`git diff`) and stage everything before committing.

Analyze the changes and generate a commit message following the **Conventional Commits 1.0.0** specification (https://www.conventionalcommits.org/en/v1.0.0/).

**User-provided description:** $ARGUMENTS

If the user provided a description above, use it as the `<description>` portion of the title line verbatim (just ensure it's lowercase and has no trailing period). You still determine the `<type>` and `<scope>` from the diff. If no description was provided, generate one from the diff as usual.

## Commit message format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

## Rules

### Scope strategy

Determine whether to include a scope by inspecting the repository:

1. **Check the repo name** (`basename $(git rev-parse --show-toplevel)`)
2. If the repo name already represents a specific service or domain (e.g. `auth-service`, `transfers-api`, `payments-worker`, `kyc-service`), **omit the scope** — the repo is the scope:
   ```
   fix: prevent duplicate SEPA credits under concurrent requests
   ```
3. If the repo is broad or multi-module (e.g. `app-ios`, `backend-monolith`, `infrastructure`, `shared-libs`), **include a scope** targeting the module, feature area, or package affected:
   ```
   fix(payments): prevent duplicate SEPA credits under concurrent requests
   ```
4. When in doubt, include the scope — redundancy is a smaller sin than ambiguity.

### Title line
- **type** (REQUIRED): one of `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
- **scope** (CONTEXTUAL): see scope strategy above
- **`!`** after type/scope if the change is a BREAKING CHANGE, e.g. `feat(api)!: ...` or `feat!: ...`
- **description** (REQUIRED): imperative mood, lowercase, no period at the end, max 72 characters total for the title line
  - ✅ "add webhook handler for reverification"
  - ❌ "Added webhook handler for reverification."

### Body (include if changes are non-trivial)
- Separate from title with one blank line
- Explain **what** changed and **why**, not how
- Wrap lines at 72 characters
- May use multiple paragraphs separated by blank lines

### Footer (include when applicable)
- Separate from body with one blank line
- Use `BREAKING CHANGE: <description>` for breaking changes (MUST be uppercase)
- Use git trailer format for references: `Refs: #123`, `Closes: #456`
- Multi-word tokens use `-` instead of spaces: `Reviewed-by:`, `Acked-by:`

## Choosing the type

| Type       | When to use                                                  | SemVer  |
|------------|--------------------------------------------------------------|---------|
| `feat`     | A new feature or capability for the user                     | MINOR   |
| `fix`      | A bug fix                                                    | PATCH   |
| `docs`     | Documentation only changes                                   | -       |
| `style`    | Formatting, semicolons, whitespace (no logic change)         | -       |
| `refactor` | Code restructuring that neither fixes a bug nor adds feature | -       |
| `perf`     | Performance improvement                                      | PATCH   |
| `test`     | Adding or updating tests                                     | -       |
| `build`    | Build system or external dependencies (gradle, docker)       | -       |
| `ci`       | CI/CD configuration (GitHub Actions, pipelines)              | -       |
| `chore`    | Maintenance tasks that don't modify src or test files         | -       |
| `revert`   | Reverting a previous commit                                  | -       |

## Process

1. Read the diff carefully
2. Check the repo name to decide the scope strategy
3. Determine the single most appropriate type
4. If scope is needed, identify it from the primary module/package affected
5. Write a concise imperative description
6. If the change is non-trivial, write a body explaining what and why
7. Add footers if there are breaking changes or issue references
8. If changes span multiple unrelated concerns, suggest splitting into separate commits instead
9. Run `git commit -m "<message>"` with the generated message