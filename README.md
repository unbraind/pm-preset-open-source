# pm-preset-open-source

An open-source project workspace preset for [pm-cli](https://github.com/unbraind/pm-cli).

Applies community-focused settings optimised for issue triage, milestone-driven releases, and contributor-friendly workflows. Installs three ready-to-use templates: bug reports, feature requests, and good-first-issues.

---

## Install

```sh
pm install github.com/unbraind/pm-preset-open-source
```

The `--project` flag scopes the extension to the current pm workspace only.
Omit it to install globally for all workspaces.

---

## Usage

```sh
pm oss-setup
```

Run this once after `pm init` to configure your workspace. The command writes
`settings.json` and creates the three templates inside `.agents/pm/templates/`.

### Options

| Flag | Short | Description |
|------|-------|-------------|
| `--force` | `-f` | Overwrite an existing `settings.json` without prompting. |
| `--dry-run` | `-n` | Preview all changes without writing any files. |
| `--prefix <str>` | `-p` | Override the `id_prefix` in `settings.json` (default: `oss-`). |

### Examples

```sh
# Standard setup
pm oss-setup

# Preview without writing
pm oss-setup --dry-run

# Re-apply and overwrite existing settings
pm oss-setup --force

# Use a custom item-ID prefix
pm oss-setup --prefix "gh-"
```

---

## What gets installed

### `settings.json`

Written to `.agents/pm/settings.json`:

```json
{
  "id_prefix": "oss-",
  "governance": {
    "preset": "default",
    "ownership_enforcement": "off",
    "create_mode_default": "progressive",
    "close_validation_default": "warn",
    "metadata_profile": "core"
  },
  "validation": {
    "sprint_release_format": "off",
    "parent_reference": "warn"
  },
  "item_types": {
    "definitions": [
      { "name": "Epic",    "description": "A major feature or milestone release" },
      { "name": "Feature", "description": "A user-facing improvement or addition" },
      { "name": "Issue",   "description": "A bug or regression report" },
      { "name": "Task",    "description": "A contributor task or good-first-issue" }
    ]
  },
  "search": { "mode": "keyword" },
  "calendar": { "default_view": "month", "first_day_of_week": 1 },
  "telemetry": { "enabled": false }
}
```

### Templates

All three templates are written to `.agents/pm/templates/`:

| File | Type | Priority | Tags |
|------|------|----------|------|
| `bug-report.json` | Issue | medium | `bug`, `community` |
| `feature-request.json` | Feature | medium | `feature-request`, `community` |
| `good-first-issue.json` | Task | low | `good-first-issue`, `help-wanted`, `contributor-friendly` |

---

## Contributor workflow — next steps

After running `pm oss-setup`, typical community workflows look like:

### Triage an incoming bug

```sh
pm create --template bug-report
# Fill in: version, OS, steps to reproduce, expected vs actual behaviour
pm list --type Issue --tag bug
```

### Track a feature request

```sh
pm create --template feature-request
# Fill in: use case, proposed solution, milestone target
pm board --group milestone
```

### Onboard a new contributor

```sh
pm create --template good-first-issue
# Fill in: estimated hours, relevant files, mentorship contact
pm list --tag good-first-issue
```

### Milestone release view

```sh
pm board --group milestone
```

### Full workflow tips

- Use Epics to represent major milestone releases (e.g. `v1.0`, `v2.0`).
- Attach Features and Issues as children of the relevant Epic.
- Tag Tasks as `good-first-issue` to surface them for new contributors.
- `close_validation_default: warn` means items can be closed without all
  metadata filled in — keep it as a nudge rather than a blocker for fast-moving
  community projects.
- `ownership_enforcement: off` allows any contributor to pick up any item.

---

## Requirements

- pm-cli `>=2026.5.0`
- Node.js `>=18`

---

## License

MIT

## Release Automation

This package is release-ready for GitHub, npm, and Bun-compatible installs. CI runs type checking, build, production dependency audit, package packing, Bun install verification, and pm-changelog validation. The daily release workflow publishes only when commits exist after the latest release tag and uses pm-changelog to generate CHANGELOG.md and GitHub release notes.
