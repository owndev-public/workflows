---
description: "Workspace guidance for reusable GitHub Actions workflows, GitVersion automation, release notes, and repository documentation."
---

## Repository focus

- This repository is the source of reusable GitHub Actions workflows.
- Optimize changes for reuse by calling repositories, not just for this repository.
- Keep caller workflows simple and configuration-driven through GitHub variables
  and secrets.
- Preserve compatibility with the documented workflows for GitVersion badges and
  GitHub releases.

## Workflow authoring rules

- Prefer reusable workflow patterns that work well with `workflow_call`.
- Keep permissions minimal and explicit for each workflow.
- Pin third-party actions to full commit SHAs instead of floating tags.
- Favor clear step names, readable summaries, and maintainable job structure.
- Prefer `bash` on `ubuntu-latest` unless a different shell or runner is required.
- Avoid introducing unnecessary required inputs when repository or organization
  variables already fit the design.

## GitVersion and branching

- Treat `GitVersion.yml` as the authoritative versioning contract.
- Preserve support for `main`/`master`, `dev`/`develop`, `release/*`,
  `hotfix/*`, and `feature/*` branch patterns.
- Do not replace GitVersion-driven behavior with custom version logic unless it
  is a clearly documented fallback.
- When touching version logic, keep release tags, pre-release handling, and
  branch-derived behavior predictable.

## Release automation expectations

- Keep release behavior safe for production and pre-release scenarios.
- Maintain support for artifact download, release metadata, optional discussion
  creation, and generated release bodies.
- Keep AI-generated release notes optional and resilient when tokens,
  permissions, or review environments are missing.
- Never invent release note content; summaries must stay grounded in actual
  commits, PRs, and repository documentation.
- Preserve reviewability through workflow summaries and environment approval
  gates where applicable.

## Security and secret handling

- Never hardcode secrets, tokens, repository URLs with embedded credentials, or
  sensitive values.
- Prefer least-privilege permissions for `GITHUB_TOKEN` and any PAT usage.
- Assume all user-controlled strings are untrusted when used in shell scripts.
- Quote shell variables, avoid unsafe interpolation, and do not expose secrets in
  logs or step summaries.
- Keep cross-repository operations explicit, especially when pushing badges or
  publishing releases.

## Documentation sync

- When behavior changes, update `README.md` and the matching files in
  `examples/` in the same change whenever practical.
- Keep repository-facing documentation and workflow comments in English for
  consistency.
- Document new variables, secrets, permissions, and operational caveats.
- Keep examples aligned with the real reusable workflows so they remain copyable
  and trustworthy.

## Prompt and instruction assets

- Preserve the purpose of files under `.github/instructions/`,
  `.github/prompts/`, and `.github/chatmodes/`.
- Avoid broad or conflicting guidance that makes the repository harder to use as
  a shared automation source.
- Prefer concise, specific instructions over vague global rules.

## Change quality bar

- Prefer small, reviewable changes.
- Preserve backward compatibility unless a breaking change is intentional and
  documented.
- Call out hidden operational impacts such as permissions, environment names,
  required variables, or token scope changes.
- If a workflow change affects consumers, reflect that impact in documentation
  and examples in the same update.
