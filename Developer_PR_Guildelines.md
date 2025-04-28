# 🚀 Developer Team Guidelines for PRs and Main Branch Protection

## 📋 Purpose

To ensure the stability of the `main` branch, we are adopting a workflow where **only Pull Requests (PRs)** are allowed to update the `main` branch, and **PRs must originate from the `uat` branch**.

---

## 🧠 Key Rules

1. **Never push directly to the `main` branch.**
   - Always create a Pull Request (PR) to merge changes into `main`.

2. **Always create your working branches off `uat`.**
   - Example: `feature/awesome-update`, `bugfix/fix-typo`

3. **All PRs to `main` must come from `uat`.**
   - No feature branch should PR directly to `main`.
   - Merge your feature branches into `uat` first.

4. **Pull Request Checklist:**
   - ✅ Ensure your source branch is `uat`.
   - ✅ Pass all GitHub Action checks (e.g., "Enforce UAT PR").
   - ✅ Request a review if necessary.

5. **No direct pushes:**
   - GitHub Branch Protection rules are not enforced (due to free plan), but team discipline is expected.

---

## 🔥 Workflow Diagram

```plaintext
feature/* → merge into → uat → PR into → main
```

---

## 🛠 Example Process

1. Pull the latest changes:
   ```bash
   git checkout uat
   git pull origin uat
   ```

2. Create a feature branch:
   ```bash
   git checkout -b feature/awesome-update
   ```

3. Work, commit, push:
   ```bash
   git push origin feature/awesome-update
   ```

4. Open a PR: `feature/awesome-update` → `uat`

5. After merging to `uat`, open a new PR: `uat` → `main`

6. Wait for GitHub Action to validate your PR.

7. Merge when approved.

---

## 🚨 Important Reminders

- Direct pushes to `main` may corrupt production code.
- Always pull latest changes before creating a branch.
- Always keep `uat` updated and clean.
- Do not bypass PR validations manually.

---

## 💬 Questions or Help?

Please contact the DevOps team or your tech lead if you encounter issues.

---

# 🎯 Goal

Maintain a safe, consistent, and reliable release process.

Let's work together to protect our main branch! 🚀

