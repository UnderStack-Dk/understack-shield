# Team access setup (pending — do this once you have usernames)

No collaborators have been invited yet. When you are ready to add a
developer:

1. GitHub Organization → People → Invite member (invite them to the
   organization first, if not already a member).
2. Go to the **Developers** team (created empty, Write permission on
   this repository) → Add member.
   - If the **Developers** team does not exist yet, see the setup
     report for why, and create it manually: Org → Teams → New team →
     name it `Developers` → Repositories → add `REPO` with permission
     **Write**.
3. Never grant Admin or Owner to a developer.
4. Confirm the new member can: clone, branch, commit, push to their
   own branch, open a PR, comment, review — and cannot push directly
   to `main`.
