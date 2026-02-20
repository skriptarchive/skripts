# Contributing to SkriptArchive Scripts

Thank you for contributing! Please follow these guidelines:

## 1. Fork & Branch
- Fork the repository.
- Create a new branch with a descriptive name, e.g., `fly-command` or `add-teleport`.

## 2. Script Format
Each script must include:
- Description
- Version
- Author
- Required Addons
- Tags (e.g., utility, admin, fun)
- Code

**Example header in Markdown:**

```md
# Fly Command
## Description
Allows players to toggle fly mode.
## Requirements
Skript 2.7+, Minecraft 1.20+
## Author
YourName
## Version
1.0
## Addons
None
## Tags
utility, admin
````

* File naming: `fly-command.sk`
* Optional subfolders: `/gui`, `/admin`, `/economy`

## 3. Pull Request

* PR target: `main` branch
* PR title format: `[Category] ScriptName` e.g., `[Utility] Fly Command`
* Include a short description in the PR body (what the script does, required addons, MC version).

## 4. Review & Merge

* Maintainers will review the PR for format, tags, and code quality.
* Do not push directly to `main`.

## 5. License

* Scripts must be compatible with **MIT or CC0**.
* If using a different license, mention it in the PR.
