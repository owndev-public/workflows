# Contributing to `owndev-public/workflows`

Thanks for taking the time to contribute.

This repository contains reusable GitHub Actions workflows and supporting
documentation. Good contributions here are usually one of these:

- documentation fixes and clarifications
- workflow improvements or bug fixes
- better examples for calling repositories
- issue templates and repository hygiene improvements

Please read the [Code of Conduct](./CODE_OF_CONDUCT.md) before contributing.

## Getting Started

Before you open a pull request:

1. Read the [README](../README.md) for the current workflow behavior.
2. Check existing [issues](../../issues) and pull requests.
3. If your change is large, open an issue first to discuss the approach.

## Development Notes

When changing workflow behavior:

- keep reusable workflows generic and safe to call from other repositories
- prefer configuration through variables and secrets over hard-coded values
- preserve least-privilege permissions where possible
- update examples when caller behavior changes
- update the README if inputs, outputs, secrets, or setup steps change

When changing documentation:

- keep it concise and accurate
- prefer English for all user-facing text in this repository
- avoid references to repositories or organizations that are not part of this
  project

## Submitting Changes

1. Fork the repository.
2. Create a branch for your change.
3. Make your changes.
4. Test or validate the affected workflow or documentation.
5. Open a pull request with a clear description.

Helpful pull requests usually include:

- what changed
- why it changed
- how it was tested or validated
- whether documentation or examples were updated

## Issues and Feature Requests

Use the issue templates in this repository whenever possible:

- bugs for broken behavior
- documentation updates for docs fixes
- feature requests for new functionality
- support questions for usage help

## Review Expectations

Please keep changes focused.

Small pull requests are easier to review, easier to test, and much less likely
to summon merge-conflict goblins.

## Security

Do not post secrets, tokens, or sensitive configuration in issues or pull
requests.

If you find a security problem, report it privately instead of opening a public
issue.
