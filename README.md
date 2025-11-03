# RAIO Infra DevSecOps Workflows

This repository contains **centrally managed, reusable GitHub Actions workflows** for automated security scanning (SAST) using [Gitleaks](https://github.com/gitleaks/gitleaks) and [Semgrep](https://semgrep.dev/). These workflows are designed for use across all RAIO-Dev organization repositories, enabling consistent and easy security enforcement with minimal setup.

## Contents

- `.github/workflows/sast-gitleaks.yml` — Reusable workflow for secret scanning with Gitleaks
- `.github/workflows/sast-semgrep.yml` — Reusable workflow for SAST/code analysis with Semgrep
- `.github/workflows/sast.yml` — Orchestrator workflow to run both Gitleaks and Semgrep together

## How to Use in Other Repositories

To apply these scans in any repo of your organization, create a workflow file (for example: `.github/workflows/devsecops.yml`) containing:

```
name: devsecops

on:
  push:
  pull_request:
  workflow_dispatch:

jobs:
  sast:
    uses: RAIO-Dev/raio-infra-devsecops/.github/workflows/sast.yml@main
```

- No need to copy/paste security jobs – always use the latest version from this repo via the `uses:` field.
- For branch protection, add the `sast` workflow as a required status check in repository settings.

## How It Works

- The orchestrator (`sast.yml`) invokes both `sast-gitleaks.yml` and `sast-semgrep.yml` as reusable jobs via `workflow_call`.
- Each workflow runs independently and fails the build if any issues are found.
- No tokens or extra secrets required for OSS mode (Gitleaks, Semgrep).

## Updating Central Workflows

Any updates pushed to these workflows in this repo are immediately available for all repositories that use them on the current branch (e.g., `main`).  
To pin a specific version/commit for reproducibility, change the `@main` in your caller workflow to a commit SHA.

## References

- [GitHub Docs: Reusing workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [Gitleaks](https://github.com/gitleaks/gitleaks)
- [Semgrep](https://semgrep.dev/)

---

**Tip:** This approach makes it easy to maintain compliance and update security logic centrally—update once, all consuming repos benefit instantly!
```
