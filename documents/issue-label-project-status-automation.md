# Issue label → Project Status automation

This document describes the GitHub Actions workflow that keeps the **`learn-project`** GitHub Project status in sync with issue labels in this repository.

## Workflow

- File: `.github/workflows/issue-label-to-project-status.yml`
- Trigger: `issues` events
  - `opened`
  - `labeled`
  - `unlabeled`
  - `edited`
- Permissions:
  - `issues: read`
  - `projects: write`
  - `contents: read`

## Behavior

- The workflow reads the labels of the issue from the event payload.
- It looks for a label whose **name matches one of the Project Status options** (case-insensitive):
  - `Backlog`
  - `Ready`
  - `In progress`
  - `In review`
  - `Done`
- If no such label is present, the workflow does nothing.
- If a matching label is present:
  - It finds the `learn-project` Project (user project **#10** for `k3aworks`).
  - It ensures the issue is added as a Project item (creates the item if missing, otherwise reuses the existing one).
  - It sets the Project **`Status`** field for that item to the corresponding option.

## Requirements

- This repository must be pushed to GitHub so that Actions can run.
- The default `GITHUB_TOKEN` (provided automatically by GitHub Actions) needs `projects: write` permission (granted in the workflow).
- GitHub issue labels should exist with names matching the status options above (e.g. labels named `Backlog`, `Ready`, `In progress`, `In review`, `Done`).

## How to use

1. Create or update labels in this repository so their names match the Project status options.
2. Create or open an issue.
3. Add exactly one status label (e.g. `In progress`) to the issue.
4. Wait for the workflow run to complete.
5. Open the **`learn-project`** Project and verify that the corresponding item has its **Status** updated to the same value.

If you change the Project status options in GitHub, you must update both:
- The list of `statusNames` inside the workflow, and
- The labels in this repository to keep names aligned.
