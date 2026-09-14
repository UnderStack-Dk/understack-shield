# Development Workflow

```
feature/fix/docs branch
        |
        v
   Pull Request
        |
        v
     develop
        |
   integration / review
        |
        v
   Pull Request
        |
        v
       main
        |
        v
final owner/admin decision
```

- Branch naming: `feature/*`, `fix/*`, `refactor/*`, `chore/*`,
  `developer-name/*`.
- `main` requires a Pull Request, is protected against force-push and
  deletion, and requires the repository owner's review before merge.
- No individual developer branches or feature work exist yet — this
  document only defines the convention for when they start.
