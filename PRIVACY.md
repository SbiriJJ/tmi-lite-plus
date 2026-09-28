# Prompt Lite+ Privacy Notice

Version 1.6  
Last updated: 28 September 2026

## Summary

Prompt Lite+ is a local Windows desktop client. It does not operate a Prompt Lite+
cloud service, advertising system, analytics service, user-account service, or
developer telemetry endpoint.

Prompt Lite+ starts and communicates with a separately installed Codex CLI
`app-server`. Prompts, conversation data, selected file references, approvals,
tool interactions, and agent events are therefore processed by Codex CLI and,
where applicable, OpenAI services under the User’s Codex/OpenAI account and
their applicable terms and privacy policies.

## Data stored locally by Prompt Lite+

Depending on use, Prompt Lite+ stores:

- UI preferences and the last selected project/thread in the INI file beside
  the executable;
- common Codex-related settings in
  `HKEY_CURRENT_USER\Software\PromptLitePlus`;
- the accepted EULA version and SHA-256 fingerprint in the local INI file;
- the latest session diagnostic information in
  `PromptLitePlus.startup.log`;
- working-directory overrides;
- user-selected downloaded files at paths chosen by the User.

The session log is overwritten when the first Prompt Lite+ instance starts.
Additional concurrently running instances append synchronized entries to that
same current-session log.

Pending asynchronous questions and unsent reply drafts are held in application
memory for the current session. Unsent drafts are not written to the INI file.
When the User sends a reply, it is forwarded to Codex CLI as conversation input.
Questions included in conversation history may also be present in Prompt Lite+'s
local rolling-history cache and in Codex CLI's own persisted history.

## Data handled by Codex CLI and other tools

Codex CLI controls authentication, conversation persistence, OpenAI requests,
model use, tools, commands, MCP servers, and its own logs or local storage.
Prompt Lite+ displays and forwards protocol data but does not replace Codex CLI’s
terms or privacy behavior.

Commands, MCP servers, network tools, browser tools, package managers, and
other processes started through Codex CLI may independently read, transmit, or
store data according to their configuration. The User must review those tools
before approving them.

## Network activity initiated by Prompt Lite+

Prompt Lite+ may initiate network activity when:

- the Codex CLI maintenance feature checks package/version information or
  performs an installation/update selected by the User;
- the User chooses Download for an HTTP or HTTPS transcript link;
- scheduled or manual Release/Rolling checks contact the public GitHub
  repository and download an available application update.

Application update checks send standard HTTPS request information (including
IP address and an application/version User-Agent) to GitHub and its download
infrastructure. They do not send conversation text, project paths or prompts.
The selected channel, frequency and last-check timestamps are stored in the
local INI. Packages are verified and staged in `update`; personal INI settings
are not replaced. Local staged executable checks remain supported.

## Personal and confidential data

Do not place personal, confidential, regulated, or third-party data in prompts,
working directories, attached files, logs, or tools unless authorized and
appropriate for the configured Codex/OpenAI account and every involved service.

Startup logs may contain paths, thread identifiers, titles, operational errors,
or conversation-related metadata. Protect them as you would protect the
underlying development project.

## Retention and deletion

Prompt Lite+ does not control the retention of conversations or account data
managed by Codex CLI or OpenAI.

To remove Prompt Lite+ local preferences, close all Prompt Lite+ instances and
remove the local INI file and the
`HKEY_CURRENT_USER\Software\PromptLitePlus` registry key. Remove downloads and
logs separately if required.

## Changes

This notice may be updated when Prompt Lite+ storage, networking, or public
update behavior changes. A change to the EULA version or text causes the
disclaimer to be displayed again.
