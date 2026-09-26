# review-claude-skills

Personal Claude Code plugin marketplace. Add it once on any machine and it stays in sync via git — no manual copying of agent files between devices.

## Structure

```
review-claude-skills/
├── .claude-plugin/
│   └── marketplace.json                # lists every plugin in this repo
├── plugins/
│   ├── code-review/
│   │   ├── .claude-plugin/plugin.json
│   │   └── agents/code-reviewer.md
│   ├── security-auditor/
│   │   ├── .claude-plugin/plugin.json
│   │   └── agents/security-auditor.md
│   ├── accessibility-reviewer/
│   │   ├── .claude-plugin/plugin.json
│   │   └── agents/accessibility-reviewer.md
│   ├── performance-profiler/
│   │   ├── .claude-plugin/plugin.json
│   │   └── agents/performance-profiler.md
│   ├── api-contract-reviewer/
│   │   ├── .claude-plugin/plugin.json
│   │   └── agents/api-contract-reviewer.md
│   ├── angular-specialist/
│   │   ├── .claude-plugin/plugin.json
│   │   └── agents/angular-specialist.md
│   └── test-designer/
│       ├── .claude-plugin/plugin.json
│       └── agents/test-designer.md
└── README.md
```

## Agents in this repo

| Plugin | Agent | What it does |
|---|---|---|
| `code-review` | `code-reviewer` | Full-stack PR reviewer covering correctness, security, best practices, and performance across frontend and backend. |
| `security-auditor` | `security-auditor` | Traces authentication, authorization, cookies, input handling, and data exposure across the full request path. Finds security issues that span several files. |
| `accessibility-reviewer` | `accessibility-reviewer` | Checks keyboard use, focus, semantics, screen reader behavior, and RTL/LTR UI. |
| `performance-profiler` | `performance-profiler` | Investigates measured slow paths, bundle size, rendering, network requests, and database queries. Focuses optimization on actual bottlenecks, not speculation. |
| `api-contract-reviewer` | `api-contract-reviewer` | Checks request/response shapes, validation, errors, pagination, and frontend/backend compatibility. Prevents integration surprises. |
| `angular-specialist` | `angular-specialist` | Reviews signals, RxJS, change detection, SSR, hydration, forms, and component boundaries. |
| `test-designer` | `test-designer` | Finds behavior that needs unit/integration/e2e tests and writes tests for meaningful edge cases (has write access, unlike the read-only reviewers). |

## Add a new skill or agent later

1. Create a new folder under `plugins/`, e.g. `plugins/test-writer/`.
2. Give it its own `.claude-plugin/plugin.json` and an `agents/` and/or `skills/` folder.
3. Add an entry for it in the top-level `.claude-plugin/marketplace.json` `plugins` array.
4. Commit and push. Every device with this marketplace added will pick it up automatically.

## One-time setup (per machine)

```shell
/plugin marketplace add <github-owner>/review-claude-skills
/plugin install code-review@review-claude-skills
/plugin install security-auditor@review-claude-skills
/plugin install accessibility-reviewer@review-claude-skills
/plugin install performance-profiler@review-claude-skills
/plugin install api-contract-reviewer@review-claude-skills
/plugin install angular-specialist@review-claude-skills
/plugin install test-designer@review-claude-skills
```

Or, for local testing before you push to GitHub:

```shell
/plugin marketplace add E:\Projects\Claude\review-claude-skills
/plugin install code-review@review-claude-skills
```

Claude Code checks the marketplace's git remote periodically in the background and pulls new commits automatically, so as long as every device has this marketplace added (via the GitHub source, not the local path), edits pushed from one machine reach the others without reinstalling. You can also force a refresh with `/plugin marketplace update review-claude-skills`.

## Notes

- `plugin.json` intentionally omits `"version"`. Without it, Claude Code uses the resolved git commit SHA as the version, so *every* push counts as an update. If you'd rather control updates manually, add `"version": "1.0.0"` and bump it on each release.
- Keep the repo private on GitHub if these agents reference anything internal; private repos work fine as marketplace/plugin sources as long as your git credentials (e.g. `gh auth login`) are set up on each machine.
