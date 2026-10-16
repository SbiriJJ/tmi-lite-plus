# TMI Lite+ Privacy Notice

Version 2.1  
Last updated: 16 October 2026

## Summary

TMI Lite+ is a local Windows desktop client. It does not operate a TMI Lite+
cloud service, advertising system, analytics service, user-account service, or
developer telemetry endpoint.

TMI Lite+ starts and communicates with a separately installed Codex CLI
`app-server`. Prompts, conversation data, selected file references, approvals,
tool interactions, and agent events are therefore processed by Codex CLI and,
where applicable, OpenAI services under the User’s Codex/OpenAI account and
their applicable terms and privacy policies.

## Data stored locally by TMI Lite+

Depending on use, TMI Lite+ stores:

- UI preferences and the last selected project/thread in the INI file beside
  the executable;
- common Codex-related settings in
  `HKEY_CURRENT_USER\Software\TMILitePlus`;
- the accepted EULA version and SHA-256 fingerprint in the local INI file;
- the latest session diagnostic information in
  `TMILitePlus.startup.log`;
- a bounded local rolling-history cache when that preference is enabled;
- working-directory overrides;
- permanent desktop read/control grants, keyed by executable path, in
  `HKCU\Software\TMILitePlus\DesktopAccess` and its `Control` subkey;
- the explicit revocable global-read setting (`ReadAllWindows`) in DesktopAccess;
- host-owned Worktime measurements and quota-reset metadata in the current
  user's TMI registry settings;
- scheduled prompt text, target thread/project and directory, timing rules,
  enabled state, delivery state and bounded execution logs in
  `%LOCALAPPDATA%\TMILitePlus\schedules.json`;
- user-selected downloaded files at paths chosen by the User.

The session log is overwritten when the first TMI Lite+ instance starts.
Additional concurrently running instances append synchronized entries to that
same current-session log.

Pending asynchronous questions and unsent reply drafts are held in application
memory for the current session. Unsent drafts are not written to the INI file.
When the User sends a reply, it is forwarded to Codex CLI as conversation input.
Questions included in conversation history may also be present in TMI Lite+'s
local rolling-history cache and in Codex CLI's own persisted history.

## Resident backend and local desktop tools

CLI Setup runs in the on-demand TMIMaintenance application. It uses the same
local settings and, when the host is running, its secured local connection.
Maintenance can also run alone; it introduces no separate account, telemetry
or remote service.

The local TMIHost backend uses secured named pipes for the current Windows
user/session. It has no public network listener. It keeps live work metadata,
pending native Codex requests and a bounded event replay in memory. These
remain available while the host runs, even after a frontend closes. Unsent
request reply drafts are not written to local preferences.

Local schedules can continue while the host is running with no frontend open.
At activation their prompt and selected target/settings are sent to Codex CLI.
They are local host schedules, not a TMI cloud service or ChatGPT cloud Work.

TMI's tmi_desktop tools can enumerate windows and, after separate read/control
consent, read accessible text, capture a screenshot, launch an executable and
send supported control/window messages or keyboard/mouse input. Text and PNG
observations are returned to Codex;
they can subsequently be sent to the configured AI service and retained in
Codex history. Native password values are excluded from accessible-text reads,
but a screenshot may still contain confidential information visible on screen.

Window identities, snapshots and temporary work consent are kept in host
memory, not a new TMI screenshot archive. Permanent app grants are stored in
the registry and can be revoked through Computer Use. Authorized content reads,
captures and control require an on-screen warning; no hidden-access mode is
provided. Use these capabilities only with informed consent. A warning is not
a guarantee against third-party modifications to the software. Revocation does not
delete previous tool responses or undo actions. Physical ESC interrupts
Computer Use while active. Exit TMI host from the tray to end owned resident
work; applications that were opened or controlled are not terminated merely
because the host exits.

## Data handled by Codex CLI and other tools

Codex CLI controls authentication, conversation persistence, OpenAI requests,
model use, tools, commands, MCP servers, and its own logs or local storage.
TMI Lite+ displays and forwards protocol data but does not replace Codex CLI’s
terms or privacy behavior.

Commands, MCP servers, network tools, browser tools, package managers, and
other processes started through Codex CLI may independently read, transmit, or
store data according to their configuration. The User must review those tools
before approving them.

## Network activity initiated by TMI Lite+

TMI Lite+ may initiate network activity when:

- the Codex CLI maintenance feature checks package/version information or
  performs an installation/update selected by the User;
- the User chooses Download for an HTTP or HTTPS transcript link;
- the User opens a Visualize fragment whose static scripts, styles, images or
  fonts use allowed external CDN providers;
- scheduled or manual Release/Rolling checks contact the public GitHub
  repository and download an available application update.

Application update checks send standard HTTPS request information (including
IP address and an application/version User-Agent) to GitHub and its download
infrastructure. They do not send conversation text, project paths or prompts.
The selected channel, frequency and last-check timestamps are stored in the
local INI. Packages are verified and staged in `update`; personal INI settings
are not replaced. The 2.x Release/Rolling endpoints (`stable-2`/`rolling-2`)
are separate from legacy 1.x Latest/`rolling`. A 2.x package includes matching
executables, supplied runtime assets and documentation. Local staged executable
checks remain supported.

Visualize fragments are read and executed locally only on request in WebView2.
Approved static CDNs can receive ordinary HTTPS request information, including
the requested resource URL and IP address. Fragment authors control resource
URLs; review untrusted content before opening it. No native commands, browser
permissions, arbitrary file resources or fetch/XHR are granted to a widget.
WebView2 browser cache/profile data is stored under
%LOCALAPPDATA%\TMILitePlus\Visualization. Preview bitmaps and widget state are
held in frontend memory only. Copy image writes the Windows clipboard; a
follow-up changes the prompt only after confirmation and is not submitted.

## Personal and confidential data

Do not place personal, confidential, regulated, or third-party data in prompts,
working directories, attached files, logs, or tools unless authorized and
appropriate for the configured Codex/OpenAI account and every involved service.

Startup logs may contain paths, thread identifiers, titles, operational errors,
or conversation-related metadata. Protect them as you would protect the
underlying development project.

## Retention and deletion

TMI Lite+ does not control the retention of conversations or account data
managed by Codex CLI or OpenAI.

To remove TMI Lite+ local preferences, close all TMI Lite+ frontend instances, exit TMI host from the tray, and
remove the local INI file and the
`HKEY_CURRENT_USER\Software\TMILitePlus` registry key. Remove downloads and
logs separately if required. Remove any local rolling-history cache files and
the WebView2 profile at %LOCALAPPDATA%\TMILitePlus\Visualization separately
when removing the corresponding local data. This does not delete Codex/OpenAI
conversation history or previous tool responses.

Delete schedules in their editor, or remove
`%LOCALAPPDATA%\TMILitePlus\schedules.json` after exiting the host to remove
all locally saved schedules and their logs. Removing Worktime registry data
does not change native Codex/account statistics.

## Changes

This notice may be updated when TMI Lite+ storage, networking, or public
update behavior changes. A change to the EULA version or text causes the
disclaimer to be displayed again.
