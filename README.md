# browser-automation

[![License](https://img.shields.io/github/license/howar31/browser-automation?style=flat-square)](LICENSE)
[![Built for Claude Code](https://img.shields.io/badge/built%20for-Claude%20Code-D77655?style=flat-square)](https://claude.com/claude-code)
[![Puppeteer](https://img.shields.io/badge/Puppeteer-40B5A4?style=flat-square&logo=puppeteer&logoColor=white)](https://pptr.dev)
[![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)](https://playwright.dev)
[![Last Commit](https://img.shields.io/github/last-commit/howar31/browser-automation?style=flat-square)](https://github.com/howar31/browser-automation/commits/main)
[![Stars](https://img.shields.io/github/stars/howar31/browser-automation?style=flat-square)](https://github.com/howar31/browser-automation/stargazers)
[![Sponsor](https://img.shields.io/badge/Sponsor-donate.howar31.com-b4532c?style=flat-square&logo=data:image/svg%2Bxml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iI2ZmZiIgZD0iTTIwLjg0IDQuNjFhNS41IDUuNSAwIDAgMC03Ljc4IDBMMTIgNS42N2wtMS4wNi0xLjA2YTUuNSA1LjUgMCAwIDAtNy43OCA3Ljc4bDEuMDYgMS4wNkwxMiAyMS4yM2w3Ljc4LTcuNzggMS4wNi0xLjA2YTUuNSA1LjUgMCAwIDAgMC03Ljc4eiIvPjwvc3ZnPg==)](https://donate.howar31.com/)

A [Claude Code](https://claude.com/claude-code) skill for driving a **real headless browser** — anything `curl` or scripted HTTP can't do: click / type / scroll / upload, screenshots and PDFs, network intercept / mock / observe, raw Chrome DevTools Protocol (CDP) debugging, and cross-browser end-to-end flows.

The skill is **model-invoked**: Claude loads it automatically when a task needs the actual browser runtime — reproducing a UI bug, debugging client-side behavior (SPA routing, SameSite cookies, console errors), scraping JS-rendered content, or taking a screenshot of a page.

## What you get

A single skill, `browser-automation`, covering **two engines** with a decision guide for picking between them:

| | Puppeteer | Playwright |
|---|---|---|
| Browsers | Chrome / Chromium | Chromium, Firefox, **WebKit (Safari engine)** |
| Strengths | deepest Chrome DevTools (raw CDP, heap/CPU profiling, `chrome://tracing`) | auto-waiting, locators, isolated contexts, `codegen`, trace viewer, cross-browser |
| Reach for it when | a quick Chrome script or DevTools-deep debugging | new interactive / E2E / cross-browser work (auto-waiting removes most flake) |

The skill body has copy-ready patterns for interaction, network interception, DOM inspection, screenshots/PDF, device/geolocation emulation, performance profiling, raw CDP, wait strategies, a sandboxed launch for macOS scripts that download files or use the File System Access API, and troubleshooting.

## Requirements

The skill drives engines you install yourself (it does not bundle them). On an active LTS Node:

```bash
npm install -D puppeteer playwright            # or -g for a global install
npx puppeteer browsers install chrome          # if postinstall didn't fetch browsers
npx playwright install chromium firefox webkit # drop names you don't need
```

The skill documents this and the cross-platform browser cache locations in full.

## Install

The skill is **model-invoked**: once installed, Claude triggers it automatically from the `description`; the slash name below only matters when you invoke it by hand. Three ways to install. **Use one per agent**: installing the same skill twice exposes it under two names.

### setup.sh — symlink (recommended; shortest slash name)

```bash
git clone https://github.com/howar31/browser-automation
cd browser-automation && ./setup.sh
```

Symlinks `skills/browser-automation` into `~/.claude/skills/`, so the skill is invoked as **`/browser-automation`** (no namespace prefix) and the repo stays the single source of truth. Idempotent; backs up before any destructive action:

```bash
./setup.sh --dry-run   # preview without applying
./setup.sh --force     # replace an existing real directory at the skill path (backed up first)
```

### Plugin manager — marketplace (discoverable, auto-updating)

Installs the same skill, namespaced as **`/browser:browser-automation`**. Two registration paths — pick one:

```bash
# Self-hosted marketplace (this repo is its own marketplace)
claude plugin marketplace add howar31/browser-automation
claude plugin install browser@browser-automation

# Or the central marketplace (all of howar31's plugins; register once)
claude plugin marketplace add howar31/howar31-marketplace
claude plugin install browser@howar31
```

### npx skills: Claude Code or another agent

The skill uses no Claude Code-specific paths or tools, so it also works in other coding agents. The [`npx skills`](https://github.com/vercel-labs/skills) CLI installs it for the agents you pick in its wizard:

```bash
npx skills add https://github.com/howar31/browser-automation   # then follow the wizard
npx skills update                                              # update later
```

In Claude Code the skill is then invoked as **`/browser-automation`**.

### Which method?

| | setup.sh (symlink) | plugin manager | npx skills |
|---|---|---|---|
| Slash name in Claude Code | `/browser-automation` (shortest) | `/browser:browser-automation` | `/browser-automation` |
| Install / update | clone + re-run `setup.sh` | `claude plugin install` / `update` | `npx skills add` / `update` |
| Discoverable via marketplace | no | yes | no |
| Other agents | no | no | yes |

Either way, skills are picked up dynamically — run `/reload-plugins` in an active session if `browser-automation` does not appear immediately.

## Uninstall

```bash
# setup.sh install (the replaced real dir, if any, is in ~/.claude/skills/.bak/<timestamp>/)
rm ~/.claude/skills/browser-automation

# Plugin install
claude plugin uninstall browser@browser-automation   # or browser@howar31

# npx skills install
npx skills remove browser-automation
```

## License

MIT — see [LICENSE](LICENSE).
