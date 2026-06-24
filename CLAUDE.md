# browser-automation — repo guide

Public Claude Code plugin: one model-invoked skill that guides driving a real headless browser (Puppeteer + Playwright). Distributed two ways from this single repo — as a standalone plugin (the repo is its own marketplace) and via the central `howar31` marketplace.

## Layout
- `skills/browser-automation/SKILL.md` — the skill, and the deliverable. Reference guide: engine decision tree, detect/install/run, Puppeteer + Playwright patterns, wait strategies, troubleshooting.
- `.claude-plugin/plugin.json` — plugin manifest.
- `.claude-plugin/marketplace.json` — self-marketplace (`source: "./"`) so the repo is directly installable.
- `setup.sh` — manual installer; symlinks the skill into `~/.claude/skills/` (alternative to the plugin manager). Idempotent; `--dry-run` / `--force`.
- `README.md` — human-facing overview + install / uninstall.

## Conventions
- The skill is model-invocable: triggering lives entirely in the SKILL.md `description` (no `disable-model-invocation` / `user-invocable` gating). Keep `description` (+ optional `when_to_use`) ≤ 1536 chars or the skill listing truncates.
- Skill content stays portable and public: no machine-specific assertions, no personal / locale data in examples.
- Conventional Commits. MIT licensed.

## Install (local test)
`./setup.sh` (symlink), or `claude plugin marketplace add howar31/browser-automation && claude plugin install browser-automation@browser-automation`.
