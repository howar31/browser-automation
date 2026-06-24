# browser-automation — SPEC

## Purpose
A public Claude Code plugin that ships one model-invoked skill, `browser-automation`. The skill is a reference guide that teaches an AI coding agent how to drive a real (headless) browser with **Puppeteer** or **Playwright** — for tasks the request/HTTP layer can't do: clicks/typing, screenshots and PDFs, network intercept/mock/observe, DevTools-level debugging (console/errors, DOM inspection, performance/coverage, heap snapshots, raw CDP), device/geolocation emulation, and cross-browser E2E. It contains no runtime code of its own; it guides the agent to install and use engines the host machine provides.

## Architecture
- **Skill, not a service.** The deliverable is `skills/browser-automation/SKILL.md` — Markdown with YAML frontmatter (`name`, `description`). Claude reads the `description` to decide when to auto-invoke; the body loads on invocation.
- **Two distribution channels from one repo:**
  - *Standalone plugin* — `.claude-plugin/plugin.json` defines the plugin; `.claude-plugin/marketplace.json` (`source: "./"`) makes the repo its own single-plugin marketplace, directly installable.
  - *Central marketplace* — the sibling `howar31-marketplace` repo lists this repo by `source: { github, repo: howar31/browser-automation }`. Same content, zero duplication.
- **Two install mechanisms, one source:** `setup.sh` (recommended) symlinks the skill directory into `~/.claude/skills/`, giving the short un-namespaced name `/browser-automation`; the Claude Code plugin manager (`claude plugin …`) installs the same skill namespaced as `/browser-automation:browser-automation`, in exchange for marketplace discoverability and auto-update. Use one, not both (both would expose the skill under two names).

## Layout
```
.
├── .claude-plugin/
│   ├── plugin.json          # plugin manifest (name, version, author, keywords)
│   └── marketplace.json     # self-marketplace: source "./" → repo installable directly
├── skills/
│   └── browser-automation/
│       └── SKILL.md         # the skill: decision tree, detect/install/run, engine patterns
├── setup.sh                 # idempotent symlink installer (--dry-run / --force)
├── README.md                # human-facing overview + install/uninstall
├── CLAUDE.md                # repo guide for agents working in this repo
├── LICENSE                  # MIT
└── .gitignore
```

## Conventions
- **Model-invocation is on by default.** No `disable-model-invocation` or `user-invocable` frontmatter — both the user and Claude can invoke. The `description` is the sole trigger surface; combined `description` + `when_to_use` must stay ≤ 1536 chars or the listing truncates (currently ~984).
- **Portable content.** No machine-specific assertions (no "already installed", no pinned Node version) and no personal/locale data in examples; the Runtime section teaches detect → install → run, with nvm guidance framed conditionally.
- **Single-file skill.** All skill content lives in one SKILL.md (no split reference files) — proportionate to its size.
- Conventional Commits; MIT license.

## Verification
- `setup.sh` self-verifies after install: confirms the skill path is a symlink to the repo and that `SKILL.md` frontmatter declares `name: browser-automation`. Re-runnable; reports all-green or pinpoints the failure.
- Manifests are plain JSON — validate with `jq empty <file>`.
- `setup.sh` syntax: `bash -n setup.sh`.

## Deploy
- **Publish:** push to `github.com/howar31/browser-automation` (public). The repo is consumed live by the plugin manager — no build step.
- **Install (recommended, symlink):** `./setup.sh` symlinks the skill into `~/.claude/skills/` → `/browser-automation`. Idempotent; `--dry-run` / `--force`.
- **Install (plugin):** `claude plugin marketplace add howar31/browser-automation` then `claude plugin install browser-automation@browser-automation`; or via the central marketplace, `…add howar31/howar31-marketplace` then `install browser-automation@howar31`. → `/browser-automation:browser-automation`.
- Run `/reload-plugins` if the skill does not appear immediately.

## Key Decisions
- **Genericized from a private original.** The skill began as a personal copy carrying machine-specific setup notes and locale/project-specific example strings; those were removed so the public version is portable. Reusable knowledge (e.g. nvm's per-Node-version global installs) was kept but reframed as conditional guidance.
- **Dual-channel, single source.** Shipping `plugin.json` + a self `marketplace.json` in the same repo, and also listing the repo in the central marketplace, lets one source serve both install paths without copying skill content.
- **No model-invocation gating.** The skill has no side effects, so it is left fully model- and user-invocable.
- **setup.sh is the recommended install.** It is a single, model-invoked skill, so the plugin namespace (`/browser-automation:browser-automation`) only adds length without benefit; the symlink install keeps the short `/browser-automation` for manual invocation. The plugin path stays available for users who want marketplace discoverability and auto-update.
