# browser-automation — repo guide

Public Claude Code plugin: one model-invoked skill that guides driving a real headless browser (Puppeteer + Playwright). Distributed two ways from this single repo — as a standalone plugin (the repo is its own marketplace) and via the central `howar31` marketplace.

## Layout
- `skills/browser-automation/SKILL.md` — the skill, and the deliverable. Reference guide: engine decision tree, detect/install/run, Puppeteer + Playwright patterns, wait strategies, troubleshooting.
- `.claude-plugin/plugin.json` — plugin manifest.
- `.claude-plugin/marketplace.json` — self-marketplace (`source: "./"`) so the repo is directly installable.
- `setup.sh` — symlink installer (recommended path): links the skill into `~/.claude/skills/` for the short `/browser-automation` name, repo stays source of truth. Idempotent; `--dry-run` / `--force`. The plugin manager is the alternative (namespaced `/browser:browser-automation`, discoverable). Use one, not both.
- `README.md` — human-facing overview + install / uninstall.

## Conventions
- The skill is model-invocable: triggering lives entirely in the SKILL.md `description` (no `disable-model-invocation` / `user-invocable` gating). Keep `description` (+ optional `when_to_use`) ≤ 1536 chars or the skill listing truncates.
- Skill content stays portable and public: no machine-specific assertions, no personal / locale data in examples.
- Conventional Commits. MIT licensed.

## Install
Recommended — `./setup.sh` (symlink → `/browser-automation`). Alternative — `claude plugin marketplace add howar31/browser-automation && claude plugin install browser@browser-automation` (→ `/browser:browser-automation`).
