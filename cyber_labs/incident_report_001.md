# Incident Response Report: Credential Leak & Remediation
**Date:** Jan 25, 2026
**Severity:** High (Remediated)
**Status:** Closed

## Executive Summary
During a routine code push, a sensitive authentication token (GitHub PAT) and internal system configuration files were accidentally committed to a public repository. The push was blocked by GitHub's "Secret Scanning" feature.

## Root Cause Analysis
- **User Error:** The `git add .` command was used without a proper `.gitignore` file, staging sensitive hidden folders (`.config`, `.ssh`) for upload.
- **Tool Configuration:** The `credential.helper store` saved the token in plain text, which was then tracked by Git.

## Remediation Steps Taken
1.  **Containment:** The push was blocked by server-side hooks (GitHub Push Protection), preventing public exposure.
2.  **Eradication:**
    - Ran `git rm -r --cached` to untrack sensitive folders from the repository index.
    - Updated `.gitignore` to explicitly exclude `.config/`, `.ssh/`, and `.termux/`.
    - Purged local Git history of the sensitive commit.
3.  **Credential Rotation:**
    - The exposed Personal Access Token was immediately revoked.
    - A new "Classic" token with least-privilege scopes (`repo`) was generated.
4.  **Verification:** Validated that only `cyber_labs` and `README.md` remain in the remote repository.

## Lessons Learned
- Always configure `.gitignore` *before* the first commit.
- Use `git status` to verify staged files before running `git commit`.
- Never use "All Access" tokens; stick to fine-grained or scoped permissions.
