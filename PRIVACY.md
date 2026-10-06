> The current privacy policy is at https://www.georgiosstavropoulos.com/privacy#ai-usage-limits-macos

# Privacy Policy

**AI Usage Monitor** — last updated 2026-08-20

**Short version: this app has no servers and collects nothing.** Everything happens
on your Mac, and the only services it talks to are Anthropic and OpenAI, reusing the
Claude Code and ChatGPT/Codex logins you already have.

## Scope

This policy covers the AI Usage Monitor macOS app ("the app"), distributed
at <https://github.com/stavrop/ai-usage-monitor> and via the `stavrop/tap`
Homebrew tap.

## What the app collects

**Nothing — for the developer.** There is no server, no analytics, no telemetry,
no crash reporting, no tracking, and no account with the developer. The developer
never receives your credentials or your usage data. The app runs entirely on your
Mac and communicates **only with Anthropic's and OpenAI's servers** to read your
usage.

## Credentials it uses

The app uses only logins that are already on your Mac. It never asks you to sign
in, and only ever contacts each provider with that provider's own token.

**Claude (Anthropic)**

- The app reads the OAuth credential that **Claude Code** already stores in your
  login **Keychain** (item `Claude Code-credentials`). It refreshes the access
  token when it expires and writes the refreshed token back to that same item.
- That token is sent **only to Anthropic's own hosts** (`api.anthropic.com`,
  `platform.claude.com`) to read your usage.

**ChatGPT (OpenAI)**

- The app reads the OAuth credential that the **ChatGPT desktop app / Codex CLI**
  already stores at `~/.codex/auth.json`. It is **read-only**: the app never
  writes to that file and never refreshes that token, because those tools own it
  and a bad write would sign you out of Codex. An expired token is reported in the
  menu instead.
- That token is sent **only to OpenAI's own host** (`chatgpt.com`) to read your
  usage.

Neither token is ever sent anywhere else, and never to the developer. If a
provider isn't signed in on your Mac, the app simply doesn't show it and makes no
requests to it.

## Data stored locally on your Mac

- The Claude Code credential lives in your login Keychain and belongs to Claude
  Code; the app reads and refreshes it but keeps no separate copy.
- The ChatGPT/Codex credential lives in `~/.codex/auth.json` and belongs to those
  tools; the app only reads it and keeps no copy.
- **App preferences** (macOS user defaults): non-personal settings only, such as
  whether the tip-jar item is hidden.
- Your usage numbers are fetched live from each provider and shown in the menu;
  the app keeps no database of them.

## Third parties

- **Anthropic.** Claude usage data is processed by Anthropic under
  [Anthropic's Privacy Policy](https://www.anthropic.com/legal/privacy).
- **OpenAI.** ChatGPT usage data is processed by OpenAI under
  [OpenAI's Privacy Policy](https://openai.com/policies/privacy-policy).
- This app is independent and is **not affiliated with, endorsed by, or sponsored
  by Anthropic or OpenAI**.
- **Optional links.** If you click a tip-jar link or open the project on GitHub,
  your browser opens that site (e.g. Buy Me a Coffee, GitHub), each with its own
  privacy policy. The app shares no data with them.

## Children

The app is not directed at children and collects no personal information from
anyone.

## Changes

This policy may change; updates are published here with a new "last updated" date.

## Contact

Questions or concerns: open an issue at
<https://github.com/stavrop/ai-usage-monitor/issues>.
