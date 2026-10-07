# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed
- The privacy policy and terms now live on https://www.georgiosstavropoulos.com (`/privacy#ai-usage-limits-macos`, `/terms#ai-usage-limits-macos`); the old `docs/privacy.html` and `docs/terms.html` redirect there, and `PRIVACY.md` and `TERMS.md` point to the new pages.

## [0.3.1] - 2026-08-20

### Added
- **Settings pane** (menu → *Settings…*, or ⌘,). Shows each provider as
  detected / not signed in / disabled, with the credential source and the
  account it last saw. Lets you hide a provider you don't want in the menu bar,
  point at a **custom credential path** for non-default installs
  (`CLAUDE_CONFIG_DIR`, `CODEX_HOME`, …), and set the **poll interval** and
  **alert threshold**.
- The alert threshold now also drives the colour ramp, so a bar turns red at the
  same point the notification fires.

### Changed
- Poll interval and alert threshold moved from compile-time constants into
  preferences (defaults unchanged: 10 minutes, 90%). Both are clamped
  (1–120 minutes, 1–100%) so a mistyped value can't hammer the API or disable
  polling entirely.

## [0.3.0] - 2026-08-20

### Added
- **ChatGPT usage.** The app now also reports OpenAI rate-limit windows, read
  from the credential the ChatGPT desktop app / Codex CLI already store at
  `~/.codex/auth.json`. Windows are labelled by length (*Monthly*, *5-hour*, …),
  and code-review limits and credit balances appear when the account has them.
- Providers are **auto-detected**: whichever of Claude Code and ChatGPT/Codex is
  signed in on the Mac gets a section. A provider that isn't signed in is hidden
  rather than shown as an error.

### Changed
- **Renamed to AI Usage Monitor** (was "Usage Monitor for Claude"), since it is
  no longer Claude-specific. The Homebrew cask token is now `ai-usage-monitor`;
  the tap carries a `cask_renames.json` entry so existing installs migrate on
  `brew update`. The bundle identifier is deliberately unchanged, so preferences
  and notification permissions carry over.
- Menu bar shows both providers as `C 45% · G 7%`. With a single provider it
  keeps the previous compact form including the reset countdown.
- The dropdown groups rows under a per-provider header (only when more than one
  provider is present, so single-provider layout is unchanged).
- Polling, rate-limit backoff, and error state are now **per provider** — one
  being signed out, expired, or 429'd no longer affects the other.
- Threshold notifications are namespaced per provider, so identically-named
  buckets can't suppress each other.

### Security
- The ChatGPT credential is treated as **read-only**. The app never writes
  `~/.codex/auth.json` and never refreshes that token: those tools own it, and a
  bad write would sign the user out of Codex. An expired token is reported in the
  menu instead.

## [0.2.3] - 2026-08-20

### Changed
- Maintenance release: the 0.2.2 code republished as a freshly Developer
  ID-signed, notarized and stapled build. **No functional changes** — `main.swift`
  is identical to 0.2.2.
- `packaging/ai-usage-monitor.rb` (the canonical cask template) had been
  left at 0.2.1 when the tap cask was bumped to 0.2.2; template and shipped cask
  are back in sync.
- Fixed the changelog's comparison links, which were missing a `[0.2.2]` entry.

## [0.2.2] - 2026-07-29

### Added
- Menu items: **Star on GitHub**, **Privacy Policy**, and **Terms of Service**.
- The support window now also invites a GitHub star (alongside the tip jar).
- Privacy Policy and Terms of Service pages (GitHub Pages) + a README screenshot.

## [0.2.1] - 2026-07-17

### Fixed
- **Rate-limit (HTTP 429) resilience.** The usage endpoint sits behind an edge
  rate limiter that returns `429` with a `Retry-After` hint. The app now parses
  `Retry-After` (seconds or HTTP-date), keeps the last-good numbers on screen,
  and schedules a single backoff retry that waits **at least** as long as the
  server asks — so it no longer polls back into an open window and keeps it
  armed. Without a server hint it falls back to exponential backoff (30s → 30m
  cap) with jitter to decorrelate from other clients on the same account. A
  successful fetch clears the backoff. Previously any non-2xx just showed
  "Last refresh failed" until the next 10-minute poll, with no `Retry-After`
  handling.

## [0.2.0] - 2026-07-14

### Added
- **Colored gradient usage bars** in the dropdown. Each bucket (session,
  weekly-all, per-model weekly) now renders as a rounded progress bar that ramps
  green → amber → red with severity, instead of a plain text percentage row.
- **Pay-as-you-go credit balance.** A **Credits (monthly)** row shows what you've
  used, what's remaining, and the monthly cap in dollars, behind the same
  severity-colored bar. Parsed from the usage endpoint's `spend` block (with a
  fallback to the legacy `extra_usage` field). The API exposes no reset timestamp
  for credits, so the row is labeled monthly rather than showing a countdown.

## [0.1.0] - 2026-07-02

### Added
- Initial public release of the macOS menu bar app.
- Live **session** (5-hour) and **weekly** (7-day) usage in the menu bar with a
  local countdown to the next reset.
- Dropdown breakdown: session, weekly-all, and per-model scoped weekly buckets
  with reset times.
- One-time-per-window notification when a bucket crosses 90%.
- Skippable "support this app" tip jar shown on launch (opens donation links in
  the browser); reopenable from the menu and dismissible with "Don't show again".
- Reads the Claude Code login credential from the Keychain and refreshes the
  OAuth token silently when it expires.
- `build.sh` (Command Line Tools build), `tools/build_release.sh` (Developer ID
  sign + notarize + staple), launch-at-login template, and an original
  CoreGraphics app icon.

[Unreleased]: https://github.com/stavrop/ai-usage-monitor/compare/v0.3.1...HEAD
[0.3.1]: https://github.com/stavrop/ai-usage-monitor/compare/v0.3.0...v0.3.1
[0.3.0]: https://github.com/stavrop/ai-usage-monitor/compare/v0.2.3...v0.3.0
[0.2.3]: https://github.com/stavrop/ai-usage-monitor/compare/v0.2.2...v0.2.3
[0.2.2]: https://github.com/stavrop/ai-usage-monitor/compare/v0.2.1...v0.2.2
[0.2.1]: https://github.com/stavrop/ai-usage-monitor/compare/v0.2.0...v0.2.1
[0.2.0]: https://github.com/stavrop/ai-usage-monitor/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/stavrop/ai-usage-monitor/releases/tag/v0.1.0
