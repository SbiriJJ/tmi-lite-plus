# TMI Lite+ for AI agents

**Lightweight interface for AI agents**

TMI Lite+ is an independent Windows x64 interface for Codex CLI, designed to
keep AI-assisted development practical on computers with limited performance.
It provides projects and conversations, working directories, model/reasoning
controls, approvals, activity, files, diffs and token/usage information without
heavy conversation rendering.

Version **2.0** is a **stable release** (9 October 2026), with an independent
**2.x Rolling** channel for subsequent updates.
Stable Windows executable version: **2.0.9778.29506** (Release/Win64).
Current Rolling Windows executable version: **2.0.9781.32598**
(Release/Win64, 12 October 2026).
Tested with Codex CLI **0.162.0**. Compatibility with every later CLI build is
not guaranteed.

[![Release 2.x](https://img.shields.io/badge/Release%202.x-2.0-blue?style=flat-square)](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/v2.0)
[![Rolling 2.x](https://img.shields.io/badge/Rolling%202.x-2.0.9781.32598-orange?style=flat-square)](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/rolling-2)
[![Platform](https://img.shields.io/badge/platform-Windows%20x64-0078D4?style=flat-square)](#requirements)
[![Codex CLI](https://img.shields.io/badge/Codex%20CLI-tested%200.162.0-6f42c1?style=flat-square)](https://developers.openai.com/codex/)
[![Downloads](https://img.shields.io/github/downloads/SbiriJJ/tmi-lite-plus/total?style=flat-square&color=brightgreen)](https://github.com/SbiriJJ/tmi-lite-plus/releases)
[![License](https://img.shields.io/badge/license-Custom%20EULA-lightgrey?style=flat-square)](EULA.md)
[![Status](https://img.shields.io/badge/status-unofficial-orange?style=flat-square)](LEGAL_NOTICE.md)

## Latest Rolling 2.x changes

- **Worktime statistics:** the host sums resident Working time per 1% drop of
  the shared subscription limit. Click the text for Current / Past / All graphs;
  previous reset cycles persist in the registry. This is an approximate local
  statistic, not billing data.
- **Changed Files and diffs:** file access stays enabled without sending a prompt.
  Diff is the first menu command, Open the second, and double-click opens the
  file. Ctrl/Shift selects multiple diffs; opening the viewer selects the latest
  edit and all edits with the same timestamp to the second.
- **Agent snapshots while working:** inspect an agent's conversation through
  its parent's process without resuming it or changing the parent writer.

The rolling ZIP is `TMILitePlusBundle-2.0.9781.32598-Win64.zip` in the
[2.x Rolling release](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/rolling-2).
The stable v2.0 ZIP and legacy 1.x releases are unchanged.

## What's new in stable 2.0

- **Resident backend:** one tray host owns an independent Codex app-server
  process for each work. Closing or switching a frontend detaches it, without
  stopping the work. Reattach later or use multiple frontends side by side.
- **Sessions and tray:** inspect resident works, open the thread needing a
  reply, and distinguish working, waiting and error states. Approvals/questions
  are handled in the owning frontend, never silently answered by the host.
- **TMI Computer Use:** local `tmi_desktop` tools discover and read Windows
  windows, capture images and operate controls. Native window messages support
  background interactions without TMI changing focus; UIA and physical input
  remain explicit alternatives. Separate read/control permissions, visible
  blue/red warnings and physical ESC interruption are included.
- **On-demand maintenance:** CLI Setup runs in `TMIMaintenance.exe`, not a
  permanently loaded setup form. The host suspends idle Codex connections while
  maintenance is open and reconnects them afterwards; active work blocks it.
- **Version-family updates:** 2.x Release and Rolling download the complete
  matching binary/assets/documentation bundle through their own endpoints.
  Existing 1.x installations do not receive a 2.x package automatically.
- **Projects and threads:** server-backed search, shared Pinned entries and a
  permanent Archived group. Documents includes the current user's Windows-known
  Documents folder and all subfolders. Working directory and project are
  independent; selecting a folder no longer creates a project implicitly.
- **Compact controls and graphics:** colored toggle buttons; an optional
  WebView2 Visualize viewer with a local inline preview, zoom and image copy.
- **Existing tools retained:** bounded conversation loading, Full Load/search,
  diffs, Changed Files, Usage charts, Questions, attachments, native prompt
  queue, Goal, Unicode formulas and optional local LaTeX rendering.

See [Release Notes](RELEASE_NOTES.md) for changes, verification and limitations.

## Download and installation

1. Open the [TMI Lite+ 2.0 release](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/v2.0) and download its Win64 ZIP.
2. Check the archive against its published SHA-256 checksum.
3. Extract the **complete** archive into a writable directory.
4. Start `TMILitePlus.exe` and accept the versioned disclaimer/EULA.
5. Use **CLI Setup** if Codex CLI needs installing/updating, then complete Codex CLI authentication.

Keep `TMILitePlus.exe`, `TMIHost.exe`, `TMIDesktop.exe`,
`TMIMaintenance.exe`, the supplied documentation and viewer assets together.
Install matching binaries from one bundle; local IPC protocol **14** rejects
incompatible peers. Do not run from inside the ZIP or replace a running host by
copying only one executable.

The neutral INI contains no personal project/thread paths, credentials,
accepted EULA or internal switches. Local preferences remain beside the
frontend. Common defaults and desktop grants use the current user's registry.

When upgrading from 1.x, finish existing work, exit all old frontends and exit
any running TMI host before extracting the complete 2.0 bundle. Retain backups
of your settings and Codex data. Earlier published ZIPs remain available.

## Requirements

- 64-bit Windows.
- Separately installed and authenticated Codex CLI; this release was tested
  with **0.162.0**.
- An account/service supported by Codex CLI.
- For Visualize only: Microsoft Edge WebView2 Runtime, installed separately.
- For graphical LaTeX only: the optional renderer described in
  [MicroTeX Setup](MICROTEX_SETUP.md). MicroTeX binaries/fonts are not included.

No AI model, account, subscription, Codex CLI, WebView2 Runtime or cloud Work
service is supplied by TMI Lite+. Optional viewers initialize only when needed.
Mermaid and generic MCP Apps rendering are not included.

## Screenshot

![TMI Lite+ interface on Windows](assets/tmi-lite-plus-demo.png)

This screenshot shows the earlier 1.x interface; it is not a screenshot of the
2.0 backend, Computer Use or current thread browser.

## Responsibility and independence

TMI Lite+ is hobby-developed software provided free of charge and may be used
for any lawful personal or professional purpose. It is not designed, certified,
supported or warranted for professional, production, safety-critical or
business-continuity use. Such use is entirely at the user's discretion and risk.

Codex CLI performs agent operations. TMI also performs its own local client and
Computer Use operations under the permissions selected by the user. Review
commands, output, file changes and grants; maintain backups.

TMI Lite+ is not affiliated with, endorsed by, sponsored by or supported by
OpenAI. It can coexist with the **ChatGPT desktop app**, but **do not use both
applications on the same project at the same time**. TMI provides no
cross-client coordination with that app.

## Documentation

- [End User License Agreement](EULA.md)
- [Legal and Copyright Notice](LEGAL_NOTICE.md)
- [Privacy Notice](PRIVACY.md)
- [Third-Party Notices](THIRD_PARTY_NOTICES.md)
- [User Guide](USER_GUIDE.md)
- [Release Notes](RELEASE_NOTES.md)
- [Optional MicroTeX Setup](MICROTEX_SETUP.md)
- [Development Notes](DEVELOPMENT_NOTES.md)

The **2.0 ZIP is linked directly above**. GitHub's generic Latest release is
deliberately kept on 1.x for legacy installations; it is not the 2.x download.
The 2.x **Release** channel uses `stable-2`; **Rolling** uses `rolling-2`.
The [2.x Rolling release](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/rolling-2)
contains the current rolling bundle, separately from stable v2.0. Legacy 1.x
continues using its existing Latest/`rolling` channels and may receive separate
1.x updates.

The visible Release/Rolling choices and check schedules remain unchanged.
Remote 2.x packages stage all four executables plus supplied runtime assets
and documentation; personal INI settings are never overwritten. Package names
use `TMILitePlusBundle-2.0.G.T-Win64.zip`, which the legacy updater does not
recognize. Upgrade from 1.x remains a manual full-ZIP installation.
See the User Guide for coordinated local/remote updates. Official Codex documentation:
[developers.openai.com/codex](https://developers.openai.com/codex/).
