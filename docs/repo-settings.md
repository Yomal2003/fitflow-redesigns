# Repository Settings (to configure on GitHub after push)

These are configured in the GitHub UI (Settings tab) after the repository is pushed, since they
are account/repo settings rather than files:

## Branch protection (Settings → Branches → Add rule for `main`)
- Require a pull request before merging (no direct pushes to `main`)
- Require status checks to pass before merging (the `docs-lint` CI job in
  `.github/workflows/ci.yml`)
- Require branches to be up to date before merging

## General settings
- Default branch: `main`
- Issues: enabled (for tracking follow-up implementation tasks per component)
- `.gitignore`: already included at the repo root, covering Node/Python build artifacts,
  environment files, and editor/OS files

## Repository description (for the GitHub "About" panel)
> Technology stack research, architecture, and design documentation for the FitFlow fitness app
> redesign — IT3060 Human Computer Interaction, SLIIT.
