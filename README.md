# Enforce Pull Requests to Main Only from UAT Branch

This repository contains a reusable GitHub Action workflow that enforces the rule:
- Pull Requests (PRs) into the `main` branch must **only come from the `uat` branch**.
- Any PR to `main` from any other branch will be automatically **rejected**.

---

## How It Works

- The reusable workflow checks the source branch (`head_ref`) of every pull request targeting `main`.
- If the source branch is **NOT** `uat`, the workflow fails and blocks the merge.

✅ This enforces a strict flow:  
`uat` → `main`  
🚫 Other branches → `main` (blocked)

---

## Setup Instructions

### 1. Create `.github` Repository

- Inside your GitHub Organization, create a special repository called `.github`.
- This repository will host organization-wide workflows.

---

### 2. Add the Reusable Workflow

- In the `.github` repository, create the file:

  ```
  .github/workflows/enforce-uat-pr.yml
  ```

- Paste the following content:

  ```yaml
  name: Enforce UAT Pull Requests

  on:
    workflow_call:

  jobs:
    check-branch:
      runs-on: ubuntu-latest
      steps:
        - name: Check that PR is from 'uat' branch
          run: |
            echo "Checking PR source branch: ${{ github.head_ref }}"
            if [ "${{ github.head_ref }}" != "uat" ]; then
              echo "❌ Only pull requests from 'uat' branch are allowed into 'main'."
              exit 1
            fi
            echo "✅ PR is from 'uat' branch. Proceeding."
  ```

---

### 3. Enable Workflow Usage in Other Repositories

In every repository that should enforce this rule:

- Create a GitHub Actions workflow at:

  ```
  .github/workflows/enforce-uat-pr.yml
  ```

- With the following content:

  ```yaml
  name: Enforce UAT PR (reusable)

  on:
    pull_request:
      branches:
        - main

  jobs:
    enforce-uat-pr:
      uses: your-org-name/.github/.github/workflows/enforce-uat-pr.yml@main
  ```

- Replace `your-org-name` with your actual GitHub organization name.

This **calls** the central reusable workflow.

---

### 4. Protect the `main` Branch

To fully enforce the rule, configure branch protection for `main`:

- Go to **Repository Settings → Branches → Branch Protection Rules**.
- Add or edit a rule for `main`:
  - ✅ Require pull request before merging
  - ✅ Require status checks to pass
    - Add the check from the `enforce-uat-pr` workflow
  - ✅ Block direct pushes
  - (Optional) ✅ Require approvals

This ensures merges cannot bypass the workflow.

---

## Visual Architecture

```plaintext
 Organization Repo (.github)
   └── .github/workflows/enforce-uat-pr.yml
       (Reusable Workflow)

 Other Repositories
   └── Tiny file that calls the reusable workflow

 PR Flow:
   uat branch --> Pull Request --> main branch
                (Validated by Action)
```

---

## Notes

- The `uat` branch must exist before creating PRs.
- Only PRs from `uat` are allowed into `main`.
- This setup ensures stricter release management and better environment control.

---

## Updating the Workflow

- If updates are needed, modify the workflow **once** in the `.github` repo.
- All repositories using it will automatically pick the latest version if they point to `@main`.

---

## License

Internal use within the organization.

