# 🚀 GitHub Actions Workflows

![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088ff?style=flat&logo=github-actions&logoColor=white)
![Reusable Workflows](https://img.shields.io/badge/reusable-workflows-brightgreen?style=flat&logo=github&logoColor=white)
![Main Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/main.svg)
![Dev Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/dev.svg)

[![GitVersion Badge](https://github.com/owndev-public/workflows/actions/workflows/gitversion-badge.yml/badge.svg)](https://github.com/owndev-public/workflows/actions/workflows/gitversion-badge.yml)
[![GitHub Release](https://github.com/owndev-public/workflows/actions/workflows/github-release.yml/badge.svg)](https://github.com/owndev-public/workflows/actions/workflows/github-release.yml)

Reusable GitHub Actions workflows for personal and public repositories.
This repository is focused on two things:

- generating GitVersion-based version badges
- creating GitHub releases with optional AI-generated release notes

Everything is written to be configured through GitHub variables and secrets,
with explicit reusable workflow inputs available for cross-organization callers,
so the calling workflows can stay pleasantly boring.

## 📖 Table of Contents

- [Requirements](#️-requirements)
- [Overview](#-overview)
- [Features](#-features)
- [Repository Structure](#️-repository-structure)
- [Available Workflows](#-available-workflows)
- [GitHub Variables](#-github-variables)
- [Secrets](#-secrets)
- [Examples](#-examples)
- [GitVersion Setup](#️-gitversion-setup)
- [Versioning](#-versioning)
- [Contributing](#-contributing)
- [Security](#-security)
- [Support](#-support)

## ⚠️ Requirements

GitVersion is required for both reusable workflows.

Your repository should include:

- the GitVersion tool in your environment
- a `GitVersion.yml` file in the repository root

GitVersion project: [GitTools/GitVersion](https://github.com/GitTools/GitVersion)

## 📜 Overview

`owndev-public/workflows` provides a small, focused set of reusable GitHub
Actions workflows that can be called from other repositories.

This repository currently includes:

1. **GitVersion Badge Workflow** for generating SVG version badges
2. **GitHub Release Workflow** for publishing releases with GitVersion metadata

> [!IMPORTANT]
> Same-organization callers can often use `secrets: inherit`, but
> cross-organization callers should pass `with:` inputs and `secrets:`
> explicitly. Reusable workflows cannot reliably read the caller's `vars`
> across organizations unless the caller forwards them.
> [!WARNING]
> If `GitVersion.yml` is missing or invalid, the workflows that depend on
> GitVersion will fail.

## ✨ Features

- GitVersion-based semantic versioning
- Automatic SVG badge generation per GitFlow-style branch type
- Release creation with optional artifact uploads
- Optional AI-generated release notes through GitHub Models
- GitHub Variables-based configuration with explicit cross-org input support
- Environment-aware release behavior
- Reusable workflows that keep caller workflows short and maintainable

## 🏗️ Repository Structure

```text
workflows/
├── .github/
│   ├── ISSUE_TEMPLATE/          # Issue forms
│   ├── prompts/                 # Prompt files used for authoring and review
│   ├── workflows/               # Reusable workflows
│   │   ├── gitversion-badge.yml
│   │   └── github-release.yml
│   ├── CODE_OF_CONDUCT.md
│   └── CONTRIBUTING.md
├── examples/                    # Example caller workflows
│   ├── gitversion-badge.yml
│   └── github-release.yml
└── GitVersion.yml               # Example GitVersion configuration
```

## 🚀 Available Workflows

### 🏷️ GitVersion Badge

Workflow file:
[`/.github/workflows/gitversion-badge.yml`](.github/workflows/gitversion-badge.yml)

Highlights:

- uses GitVersion to calculate the current semantic version
- creates an SVG badge with `emibcn/badge-action`
- stores badges in a central badge repository
- applies automatic colors for `main`, `dev`, `release/*`, `hotfix/*`, and
  `feature/*`
- writes a rich workflow summary with ready-to-copy badge URLs

Example badge URLs for this repository:

| Branch | Badge |
| --- | --- |
| `main` | ![Main Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/main.svg) |
| `hotfix/*` | ![Hotfix Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/hotfix.svg) |
| `release/*` | ![Release Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/release.svg) |
| `dev` | ![Dev Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/dev.svg) |
| `feature/*` | ![Feature Version](https://owndev-public.github.io/badges/owndev-public/workflows/version/feature.svg) |

### 🏷️ GitHub Release

Workflow file:
[`/.github/workflows/github-release.yml`](.github/workflows/github-release.yml)

Highlights:

- calculates release versions with GitVersion
- supports stable and pre-release flows
- downloads artifacts from previous jobs
- creates GitHub releases with configurable names and tags
- can generate AI-assisted release notes with review gates
- supports release discussions and custom body content

## 🔧 GitHub Variables

> [!NOTE]
> For callers in a different organization or owner account, map variables to the
> reusable workflow `with:` inputs shown in the examples. The reusable workflow
> will use those inputs first and fall back to local `vars` when available.

### GitVersion Badge Variables

| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| `GITVERSION_BADGE_REPO` | string | required | Target repository for badge storage in `owner/repo` format |
| `GITVERSION_BADGE_LABEL` | string | branch name | Badge label text |
| `GITVERSION_BADGE_COLOR` | string | branch color | Override badge color with a hex value |
| `GITVERSION_BADGE_STYLE` | string | `classic` | Badge style (`flat` or `classic`) |
| `GITVERSION_VERSION_SPEC` | string | `6.4.x` | GitVersion setup version spec |

Recommended example:

```text
GITVERSION_BADGE_REPO=owndev-public/badges
```

Reusable workflow inputs for cross-org callers:

| Input | Maps to variable |
| --- | --- |
| `badge_repo` | `GITVERSION_BADGE_REPO` |
| `badge_label` | `GITVERSION_BADGE_LABEL` |
| `badge_color` | `GITVERSION_BADGE_COLOR` |
| `badge_style` | `GITVERSION_BADGE_STYLE` |
| `gitversion_version_spec` | `GITVERSION_VERSION_SPEC` |

### GitHub Release Variables

| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| `RELEASE_GITHUB_USE_GITVERSION` | string | `true` | Enable GitVersion for release versioning |
| `RELEASE_GITHUB_TAG_NAME` | string | `v{version}` | Tag name pattern |
| `RELEASE_GITHUB_NAME` | string | `Release v{version}` | Release title pattern |
| `RELEASE_GITHUB_DRAFT` | string | `false` | Create the release as a draft |
| `RELEASE_GITHUB_GENERATE_NOTES` | string | `true` | Enable GitHub automatic release notes |
| `RELEASE_GITHUB_APPEND_BODY` | string | `true` | Append custom body to generated notes |
| `RELEASE_GITHUB_ARTIFACT_PATTERN` | string | `*` | Artifact download glob |
| `RELEASE_GITHUB_ARTIFACT_PATH` | string | `./release-files` | Local artifact download directory |
| `RELEASE_GITHUB_FILES_PATTERN` | string | `*` | Files to attach to the release |
| `RELEASE_GITHUB_ENVIRONMENT_URL` | string | repository URL | Custom environment URL |
| `RELEASE_GITHUB_CUSTOM_BODY` | string | empty | Custom markdown body |
| `RELEASE_GITHUB_DISCUSSION_ENABLED` | string | `true` | Create a linked discussion |
| `RELEASE_GITHUB_DISCUSSION_CATEGORY` | string | `announcements` | Discussion category name |

### 🤖 AI Release Notes Variables

> [!NOTE]
> AI release notes are optional. If `RELEASE_AI_TOKEN` is not configured, the
> workflow falls back to `GITHUB_TOKEN`. In that case the calling workflow must
> grant `permissions: models: read`.

| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| `RELEASE_AI_ENABLED` | string | `false` | Enable AI-generated release notes |
| `RELEASE_AI_MODEL` | string | `openai/gpt-4.1` | GitHub Models model ID |
| `RELEASE_AI_LANGUAGE` | string | `en` | Output language |
| `RELEASE_AI_MAX_TOKENS` | string | `4096` | Maximum completion tokens |
| `RELEASE_AI_TEMPERATURE` | string | `0.4` | Generation temperature |
| `RELEASE_AI_CUSTOM_PROMPT` | string | empty | Override the built-in system prompt |
| `RELEASE_AI_PROJECT_DESCRIPTION` | string | auto-generated | Project description used as release note context |
| `RELEASE_AI_MAX_COMMITS` | string | `500` | Maximum commits analyzed by the AI step |
| `RELEASE_AI_REVIEW_ENVIRONMENT` | string | auto-generated | Environment name used for review approval |

Reusable workflow inputs for cross-org callers:

| Input | Maps to variable |
| --- | --- |
| `release_github_use_gitversion` | `RELEASE_GITHUB_USE_GITVERSION` |
| `gitversion_version_spec` | `GITVERSION_VERSION_SPEC` |
| `release_github_tag_name` | `RELEASE_GITHUB_TAG_NAME` |
| `release_github_name` | `RELEASE_GITHUB_NAME` |
| `release_github_draft` | `RELEASE_GITHUB_DRAFT` |
| `release_github_generate_notes` | `RELEASE_GITHUB_GENERATE_NOTES` |
| `release_github_append_body` | `RELEASE_GITHUB_APPEND_BODY` |
| `release_github_artifact_pattern` | `RELEASE_GITHUB_ARTIFACT_PATTERN` |
| `release_github_artifact_path` | `RELEASE_GITHUB_ARTIFACT_PATH` |
| `release_github_files_pattern` | `RELEASE_GITHUB_FILES_PATTERN` |
| `release_github_environment_url` | `RELEASE_GITHUB_ENVIRONMENT_URL` |
| `release_github_custom_body` | `RELEASE_GITHUB_CUSTOM_BODY` |
| `release_github_discussion_enabled` | `RELEASE_GITHUB_DISCUSSION_ENABLED` |
| `release_github_discussion_category` | `RELEASE_GITHUB_DISCUSSION_CATEGORY` |
| `release_ai_enabled` | `RELEASE_AI_ENABLED` |
| `release_ai_model` | `RELEASE_AI_MODEL` |
| `release_ai_language` | `RELEASE_AI_LANGUAGE` |
| `release_ai_max_tokens` | `RELEASE_AI_MAX_TOKENS` |
| `release_ai_temperature` | `RELEASE_AI_TEMPERATURE` |
| `release_ai_custom_prompt` | `RELEASE_AI_CUSTOM_PROMPT` |
| `release_ai_project_description` | `RELEASE_AI_PROJECT_DESCRIPTION` |
| `release_ai_max_commits` | `RELEASE_AI_MAX_COMMITS` |
| `release_ai_review_environment` | `RELEASE_AI_REVIEW_ENVIRONMENT` |

## 🔐 Secrets

> [!IMPORTANT]
> Reusable workflows do not automatically receive secrets. Use
> `secrets: inherit` only when GitHub allows it for your caller relationship.
> For cross-organization calls, pass secrets explicitly.

| Secret | Required | Used by | Description |
| --- | --- | --- | --- |
| `GITHUB_TOKEN` | automatic | all workflows | Built-in token for repository access |
| `GITVERSION_BADGE_REPO_TOKEN` | optional | GitVersion Badge | Token with push access to the badge repository |
| `RELEASE_AI_TOKEN` | optional | GitHub Release | Personal token with `models:read` for GitHub Models |

Reusable workflow secret mappings for cross-org callers:

| Secret mapping | Maps to secret |
| --- | --- |
| `badge_repo_token` | `GITVERSION_BADGE_REPO_TOKEN` |
| `release_ai_token` | `RELEASE_AI_TOKEN` |

## 🧪 Examples

Example caller workflows are available in [`examples/`](examples/):

- [`examples/gitversion-badge.yml`](examples/gitversion-badge.yml)
- [`examples/github-release.yml`](examples/github-release.yml)

The examples use explicit `with:` and `secrets:` mappings so they work for
cross-organization consumers as well as same-organization callers.

## 🏷️ GitVersion Setup

Install GitVersion:

```bash
dotnet tool install --global GitVersion.Tool
```

Or as a local tool:

```bash
dotnet new tool-manifest
dotnet tool install GitVersion.Tool
```

Then copy [`GitVersion.yml`](GitVersion.yml) to the root of the repository that
will call these workflows.

The provided configuration supports these common branch patterns:

- `main` or `master`
- `dev` or `develop`
- `release/*`
- `feature/*`
- `hotfix/*`

Useful commit suffixes for explicit version bumps:

```text
+semver:major
+semver:minor
+semver:patch
+semver:skip
```

## 🌿 Versioning

This repository uses [Semantic Versioning](https://semver.org/) through
GitVersion.

Releases and badges are derived from Git history, tags, and branch naming.
If you want predictable results, keep branch names and commit messages tidy.
Git is very forgiving, but release automation has a longer memory.

See the repository's [Releases](../../releases) page for published versions.

## 🤝 Contributing

Please read [`/.github/CONTRIBUTING.md`](.github/CONTRIBUTING.md) before opening
issues or pull requests.

## 🔒 Security

If you find a security issue, please avoid posting sensitive details in a public
issue. Open a private security advisory or contact the repository owner first.

General security guidance:

- keep dependencies up to date
- avoid committing secrets
- prefer least-privilege tokens and permissions
- review workflow changes carefully before merging

## 📞 Support

- **Maintainer:** [`@owndev`](https://github.com/owndev)
- **Issues:** [GitHub Issues](../../issues)
- **Repository:** [owndev-public/workflows](https://github.com/owndev-public/workflows)
