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

- Inside your GitHub Organization, create a special repository called:

