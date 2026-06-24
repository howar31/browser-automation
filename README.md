# browser-automation

[![License](https://img.shields.io/github/license/howar31/browser-automation?style=flat-square)](LICENSE)
[![Built for Claude Code](https://img.shields.io/badge/built%20for-Claude%20Code-D77655?style=flat-square)](https://claude.com/claude-code)
[![Puppeteer](https://img.shields.io/badge/Puppeteer-40B5A4?style=flat-square&logo=puppeteer&logoColor=white)](https://pptr.dev)
[![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)](https://playwright.dev)
[![Last Commit](https://img.shields.io/github/last-commit/howar31/browser-automation?style=flat-square)](https://github.com/howar31/browser-automation/commits/main)
[![Stars](https://img.shields.io/github/stars/howar31/browser-automation?style=flat-square)](https://github.com/howar31/browser-automation/stargazers)
[![Sponsor on Ko-fi](https://img.shields.io/badge/sponsor-Ko--fi-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/howar31)

A [Claude Code](https://claude.com/claude-code) skill for driving a **real headless browser** — anything `curl` or scripted HTTP can't do: click / type / scroll / upload, screenshots and PDFs, network intercept / mock / observe, raw Chrome DevTools Protocol (CDP) debugging, and cross-browser end-to-end flows.

The skill is **model-invoked**: Claude loads it automatically when a task needs the actual browser runtime — reproducing a UI bug, debugging client-side behavior (SPA routing, SameSite cookies, console errors), scraping JS-rendered content, or taking a screenshot of a page.

## What you get

A single skill, `browser-automation`, covering **two engines** with a decision guide for picking between them:

| | Puppeteer | Playwright |
|---|---|---|
| Browsers | Chrome / Chromium | Chromium, Firefox, **WebKit (Safari engine)** |
| Strengths | deepest Chrome DevTools (raw CDP, heap/CPU profiling, `chrome://tracing`) | auto-waiting, locators, isolated contexts, `codegen`, trace viewer, cross-browser |
| Reach for it when | a quick Chrome script or DevTools-deep debugging | new interactive / E2E / cross-browser work (auto-waiting removes most flake) |

The skill body has copy-ready patterns for interaction, network interception, DOM inspection, screenshots/PDF, device/geolocation emulation, performance profiling, raw CDP, wait strategies, and troubleshooting.

## Requirements

The skill drives engines you install yourself (it does not bundle them). On an active LTS Node:

```bash
npm install -D puppeteer playwright            # or -g for a global install
npx puppeteer browsers install chrome          # if postinstall didn't fetch browsers
npx playwright install chromium firefox webkit # drop names you don't need
```

The skill documents this and the cross-platform browser cache locations in full.

## Install

### As a plugin (recommended)

Two registration paths install the same plugin — pick one:

**Self-hosted marketplace** (this repo is its own marketplace):

```bash
claude plugin marketplace add howar31/browser-automation
claude plugin install browser-automation@browser-automation
```

**Central marketplace** (all of howar31's plugins; register once, future plugins appear automatically):

```bash
claude plugin marketplace add howar31/howar31-marketplace
claude plugin install browser-automation@howar31
```

### Manual (symlink installer)

If you prefer a plain skill directory over the plugin manager, clone and run the installer:

```bash
git clone https://github.com/howar31/browser-automation
cd browser-automation && ./setup.sh
```

`setup.sh` symlinks `skills/browser-automation` into `~/.claude/skills/` (the repo stays the single source of truth). It is idempotent and backs up before any destructive action:

```bash
./setup.sh --dry-run   # preview without applying
./setup.sh --force     # replace an existing real directory at the skill path (backed up first)
```

Skills are picked up dynamically — run `/reload-plugins` in an active session if `browser-automation` does not appear immediately.

## Uninstall

```bash
# Plugin install
claude plugin uninstall browser-automation@browser-automation   # or @howar31

# Manual install (the replaced real dir, if any, is in ~/.claude/skills/.bak/<timestamp>/)
rm ~/.claude/skills/browser-automation
```

## License

MIT — see [LICENSE](LICENSE).
