# Contributing

This repository is in the planning/pre-development stage. These rules
apply once development begins.

1. Never work directly on `main`.
2. Create a branch from the correct base branch (`main` or `develop`).
3. Use a clear branch name: `feature/*`, `fix/*`, `refactor/*`, `docs/*`.
4. Make focused commits.
5. Push the branch.
6. Open a Pull Request and keep it focused and reviewable.
7. Wait for review.
8. The repository owner/maintainer has final authority to merge into `main`.

```bash
git checkout main
git pull
git checkout -b feature/my-feature
# work
git add .
git commit -m "feat: add my feature"
git push -u origin feature/my-feature
```

Then open a Pull Request targeting `main` (or `develop`, once in use).

Do not commit secrets, `.env` files, private keys or credentials. Do not
merge unrelated work together.
