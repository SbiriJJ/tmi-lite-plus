# TMI Lite+ for AI agents — User Guide

Version 1.6

Lightweight interface for AI agents.

The application was formerly named Prompt Lite+. Install this renamed package
manually. The new EXE is `TMILitePlus.exe`; the INI beside it is `TMILitePlus.ini`.
Only the new INI and registry names are used. There is no automatic migration
or lookup of previous application names.
Do not run Prompt Lite+ and TMI Lite+ together: cross-name instance coordination
is not provided. Old caches and historical release ZIPs are not renamed.

Version 1.6 includes a non-modal **Usage** statistics window (daily bars,
weekly comparisons and a calendar heatmap), Unicode formulas and a
local image viewer. See [development notes](DEVELOPMENT_NOTES.md) for the exact
scope and [optional MicroTeX setup](MICROTEX_SETUP.md) for graphical formulas.
Existing release ZIPs are unchanged.

## 1. What TMI Lite+ is

TMI Lite+ is a lightweight Windows interface for Codex CLI `app-server`. It
does not provide its own AI model or OpenAI account. Codex CLI performs agent
operations and preserves compatible conversations.

TMI Lite+ is independent from OpenAI. Read and accept the versioned disclaimer
at first startup. Use the **Disclaimer** button at the top left to read it
again.

TMI Lite+ is hobby-developed software provided free of charge and may be
used for any lawful personal or professional purpose. It is not designed,
certified, supported, or warranted for professional, production,
safety-critical, or business-continuity use. Any such use is at the user’s
discretion and risk.

Its lightweight interface is designed for computers where the ChatGPT desktop
app causes excessive system load.

## 2. First startup

1. Start `TMILitePlus.exe`.
2. Read the disclaimer and select **Accept and Continue**. Selecting **Exit**
   closes the application without recording acceptance.
3. Open **CLI Setup** if Codex CLI is not detected.
4. Install or update Codex CLI, or copy the displayed command and run it
   manually.
5. Authenticate using the Codex CLI login procedure when required.
6. In the browser shown at startup, expand a project and click a conversation,
   or use the context menu's **New** command to create one.

TMI Lite+ stores the accepted EULA version and its SHA-256 fingerprint. When
the version or text of `EULA.md` changes, the disclaimer is displayed again.

## 3. Projects and conversations

Version 1.6 uses the project IDs, names and roots supplied by Codex
app-server. A project's roots and a conversation's working directory are
different data: assigning a project does not move files or change the working
directory. Unassigned conversations remain unassigned; browsing does not
automatically rewrite their project membership. **Documents** is a convenient
group for unassigned threads whose effective working directory is Documents,
not an invented server project or a missing directory.

- **Show Threads / Show Current** shows/hides the grouped browser over the left sidebar.
  Project headers expand/collapse with one click; a single click on a thread
  opens it. At startup the last-used project expands once the list is loaded,
  enabled and visible. No thread opens automatically.
- Project and thread ordering is by recent activity, retaining pinned markers
  and compact dates. Expanding a project requests indexed metadata, not history.
- **Find** searches titles across all projects and expands only matching groups.
  Its integrated button shows a magnifier while editing, then **×** after searching.
  With the field focused, Enter always searches and Esc always clears the text
  and filter. The magnifier returns after clearing. These keys never submit a prompt.
- **Show archived** switches to archived conversations only. Clicking one selects
  it without resuming or restoring it. Its context
  menu contains **Unarchive** and **Delete**.
- The main header shows the active thread, project and effective working
  directory. Only the project/thread names are bold, not the browser rows.
  Selecting a project does not change the active work. Full paths are available in hints.
- The **...** button beside **Working directory** opens **Project roots**.
  Opening it merges the working directories of associated active and archived
  threads into the server's root list. Add a folder by path or Browse; Remove
  is refused when a root is used by a thread, identifying that thread.
  **Assign to thread** changes only the current idle thread to the selected root.
  The server persists the change for subsequent turns; no files are moved and
  historical paths are not rewritten. Each thread has one working directory.
- Right-click the relevant project or thread for management commands. Actions
  use that row's identity, not whichever conversation happens to be active.
- **New conversation** chooses a working directory and prepares the first prompt.
  From a real project header it uses that project; from the background/virtual
  groups it finds or creates a project for the chosen folder. The thread is
  created when its first prompt is sent.
- **Assign to project** provides a selection list identified internally by IDs,
  including Unassigned; duplicate project names do not select the wrong project.
  **New** creates a named project in this dialog. Assignment always preserves
  the thread's working directory.
- **Duplicate** requires its initial prompt and a target working directory.
  **Rename**, **Pin/Unpin**, and **Archive** operate on the clicked thread.
- **Delete conversation** is permanent and includes spawned descendants.
  Confirmation shows the target identity and metadata. **Delete project** removes
  only the project container: its threads are kept as unassigned, and working
  files are untouched. Both require explicit confirmation.
- **Refresh projects**, **Search contents**, and **Repair server index** are
  available from the context menu, including the empty browser background.
  Repair is an explicit scan/repair; ordinary browsing uses the server index.
- **Assign unassigned threads to projects...** asks for confirmation, then scans
  active and archived main threads. It assigns only threads without a project
  whose working directory matches exactly one existing project root. Agents and
  already assigned threads are skipped; unmatched or ambiguous threads remain
  unassigned. No project is created and no working directory or file is changed.
  A final report lists successful assignments, unmatched/ambiguous counts and
  any server errors. Repair never performs these assignments.
- If opening a project returns no active threads, the browser automatically
  checks unassigned active threads and applies the same unique-root matching
  across all projects. This automatic check runs at most once per application
  launch, without a confirmation or modal report; results and errors appear in
  Activity. Search results, archived-only views and virtual groups do not trigger
  it. Empty projects are never deleted. The manual command also covers archives.
- Opening reloads current server data. The same conversation cannot be opened
  by two TMI Lite+ instances at the same time.

Several TMI Lite+ instances may run concurrently. Each instance starts its
own Codex `app-server`, allowing independent work on different conversations.

TMI Lite+ can be used on the same computer as the ChatGPT desktop app, but
do not use both applications on the same project at the same time.

Spawn-agent conversations are available for review under their parent
conversation when the server exposes the required metadata.

Large conversations are read through summarized paginated server history. The
normal conversation window caches only the recent messages needed for its
configured rolling line limit. Only a valid cache is reused, and the conversation
is displayed after the server completes resume. The normal history request loads
30 recent turns by default; Config supports 1–100 turns (`Conversation.ResumeTurnLimit`
in the INI). The line limit still controls the visible transcript. Historical Activity, tool
output, and diffs are deliberately
excluded from the normal reload path. **Full Load** opens a separate non-modal
search window, uses the existing message-summary pages, shows page/turn progress, and
provides **Stop loading**. Stopping keeps and displays the history pages already
received, marked as partial. Existing turn timestamps and speaker information
remain visible. The 1 GB memory bound remains; no historical per-item timing or
complete tool-payload download is added.

**Use conversation history cache** in Config enables/disables local history-cache
reuse and writes (`Conversation.CacheEnabled`, enabled by default), including
Full Load. Disabling it does not remove existing cache files.

**Search contents...** opens a separate non-modal server-search window. Search all
conversations or the current conversation. Select a matching conversation to list
its occurrences, then select a message to see its highlighted excerpt and turn
context. **More conversations** and **More matches** page results; archived results
are a separate phase when included. **Stop** retains downloaded results and ignores
the outstanding response, but does not cancel work already running on the server.
Only **Open conversation** acquires the normal conversation writer/instance lock.
Server content search covers user and final assistant messages, not all tool/diff
output. **Full Load** provides local search through the full conversation text;
it is not a complete tool/diff archive.

## 4. Model and reasoning

Model choices and supported reasoning levels are loaded from the server catalog.
The selected values belong to the current thread. On opening a conversation,
available thread metadata updates the selectors before the complete settings
snapshot arrives. Missing values are left unknown until confirmed by the server.
Metadata describes the configured model, not per-call execution telemetry.

A manual change is sent to the server immediately; the current server notification
remains authoritative. Refreshing the conversation list does not overwrite a
manual selection. Changing the model refreshes account limits and clears the old
model's context indicator until Codex supplies context usage for the new model.

Different threads may use different models and reasoning levels.

## 5. Permissions and safety

The settings panel exposes the protocol settings directly.

- **Approval** controls when Codex CLI asks before operations.
- **Sandbox** controls filesystem/process isolation.
- **Network access** applies where supported by the selected sandbox.
- **Auto-review** allows the configured reviewer to handle applicable
  approvals.

When Codex CLI explicitly permits a persistent MCP tool approval, the MCP
request window also shows **Always allow**. This approves the current request
and stores the matching tool policy across future Codex sessions. The option
is not inferred by TMI Lite+ and is absent when the server does not offer
it.

Use **MCP...** beside the approval setting to open the runtime inspection window.
It lists MCP servers, plugin ownership, connection/authentication state and discovery
errors, plus installed connector enabled/callable state. **Details** loads the full
catalog on demand. Cached/configured tools alone do not prove a live connection;
desktop-only browser integrations need not appear as a separate Chrome MCP server.
**Background** lists server-reported terminals for the window's thread snapshot;
termination requires selecting one and confirming. Nothing is polled automatically.

**Permanent approvals** reviews stored permanent MCP and connected-app tool
approvals. The list includes per-tool approvals and an
app-wide default approval when one is configured. Selecting an entry asks for
confirmation, changes that policy back to `prompt`, reloads the configuration,
and verifies that the effective policy was revoked. A policy from a
higher-priority configuration layer is reported if it remains effective.

`dangerFullAccess` is displayed in red. It can permit operations outside the
normal workspace restrictions. Use it only when you understand and accept the
consequences.

Approvals, sandbox, and other safety settings do not make generated commands or
changes automatically correct. Review important work and maintain backups.

## 6. Sending prompts

- **Steer** is the default while a turn is active and redirects current work.
- **Append** queues or sends a normal follow-up according to the current
  server state.
- **Stop** interrupts the current turn.
- **Files** attaches file mentions to the next prompt.
- **Goal** creates, edits, pauses, resumes, or removes the persistent thread
  goal.
- **Compact** asks the server to compact the conversation context.

The Send button changes color while the agent is working. Activity status and
elapsed time are shown beside the Activity title.

`Ctrl+Enter` sends the prompt. TMI Lite+ processes this shortcut without
altering the prompt text or leaving a key logically pressed. After sending, the
empty prompt editor returns to its first line.

### Asynchronous agent questions

When the model sends structured questions, they appear in the conversation with
their suggested answers. **Questions (n)** beside the prompt opens a non-modal
window for questions received during the current application session. Choose a
question, select a suggestion or type freely, then press **Send reply**.

During an active task the reply is sent as steering input, without interrupting
the task. If the task has ended, the window states that sending starts a new
turn in the same conversation. The normal prompt draft and its file attachments
are not consumed by a question reply.

A question is marked answered only after the server accepts the submission.
Errors retain the draft and are shown in the main window. There is no automatic
retry or automatic answer. Closing the question window retains drafts in memory;
select the original conversation to send them later. Unsent drafts are not saved
across application restarts. Historical questions remain readable in the normal
transcript and Full Load, but are not all re-opened as pending questions.

## 7. Conversation and activity

The main conversation uses a configurable rolling line limit. The complete
conversation remains available through **Full Load**, with timestamps,
speaker information, and text search.

Conversation rendering is recalculated when the window is resized so block
backgrounds continue to match the current text width.

Manual scrolling disables automatic transcript scrolling. Use the down-arrow
button to return to the bottom and re-enable auto-scroll.

Activity is intentionally compact and has a configurable row limit. Tool
progress replaces the current status text instead of creating heavy animated
output. Double-click an Activity row marked `[diff]` to open its stored diff.
Activity and diffs received during the current session remain available;
historical tool details are not downloaded automatically when a thread opens.

## 8. Files, links, and diffs

Changed files are listed for the current conversation. The section title and
hint identify lists that support double-click. Double-click a file to open it.
The context menu can open it, show it in Explorer, copy its path, or display
available diffs.

Transcript paths and supported URLs are clickable. Their context menu provides:

- **Open**;
- **Show in Explorer** for local paths;
- **Download** for HTTP/HTTPS links or a copy of a local file;
- **Copy link / path**.

Download remembers its own last selected directory in the local INI file and
runs without blocking the UI.

The diff form lists available file-change snapshots for the current
conversation, grouped by recent task, current TMI Lite+ session, and date.

## 9. Token and rate information

TMI Lite+ displays context-window use, turn/thread token totals, and the rate
information supplied by the server. Context is shown as used tokens over total
capacity. Available account-limit percentages are shown as compact remaining
capacity bars; a bar is omitted when the server does not supply that window.
When the server supplies more than the usual account-limit buckets, the title
changes to **Limits+**. Click the limits to open a persistent information menu;
the hint reads **Click for info**. When supplied by Codex, reset credits appear
on one line with their count and earliest reported expiry. **Use one reset credit...**
opens a confirmation showing at most 10 credit details and the number remaining.
No credit is consumed merely by opening the menu or confirmation.

**Idle polling** requests rate information at a low frequency while no turn is
running. Token events received during work update the displayed counters
without an additional high-frequency polling loop.

**Conversation total** is the cumulative token count reported for that thread,
not a daily account total. It includes input tokens reused from cache. **Task**
is the increment during the current turn; neither value is a monetary cost.
Changing the conversation title or selecting another model does not intentionally
reset the conversation total.

## 10. Codex CLI maintenance

**CLI Setup** shows the detected executable, installed version, latest version,
and commands used for installation or update.

Only one TMI Lite+ instance performs the startup CLI version check. A later
check may occur after the shared six-hour interval. Updating Codex CLI requires
all TMI Lite+ instances/app-server processes to be disconnected.

CLI maintenance output is streamed into its dialog so progress remains visible.

Development builds require Codex CLI 0.157.1 or later. Older versions cannot start
the app-server from TMI Lite+. Version 1.6 preview requires Codex CLI 0.157.1 or later and was checked against 0.157.1.

## 11. TMI Lite+ application updates

Open **Config** using the gear button in the top toolbar. Choose one channel:

- **Release**: numbered public releases. Check every startup, daily (default),
  or weekly.
- **Rolling**: the latest published working build, for users who need current
  changes. Check every startup (default for this channel), hourly, or daily.

Only the selected channel is checked. Daily checks become due at local
midnight, including while the application stays open. Weekly checks become due
seven calendar days after the last attempt. Hourly checks use elapsed time.
Missed checks run on the next eligible startup/timer tick; the scheduler wakes
once per minute. Check timestamps are stored in the local INI, not the registry.
Failed attempts follow the same selected schedule and do not cause retry loops.

Checks and downloads run in the background, only while this is the sole
TMI Lite+ instance. A check already in progress can finish if another
instance opens, but installation still requires a single instance. **Save and
check now** performs a manual check. GitHub receives normal HTTPS request data
and the application version, not conversation contents.

An available build is downloaded to `update`, its SHA-256 and fixed EXE version
are checked, and the gear button is highlighted. Existing equal/newer staged
builds are not downloaded again. An older build never replaces a newer one.
Open Config when idle and use **Restart and Update...** to review the version
and confirm. An active task is never automatically interrupted.

The local staging mechanism remains available:

```text
update\TMILitePlus.exe
```

At startup, when TMI Lite+ is the only running instance, it compares the
staged executable version with the running version.

If a newer version is available, the dialog displays both versions:

- **Restart and Update** closes TMI Lite+, replaces the executable, keeps a
  `.previous.exe` backup, and starts the updated version;
- **Don't Update** continues using the current version without modifying the
  executable.

An update is never applied merely because it was detected. Remote packages
include documentation for that build; installation updates those documents but
never replaces the user's INI. An updated EULA triggers the existing acceptance
check. Packages use the readable name `TMILitePlus-M.m.G.T-Win64.zip`, with
the full fixed EXE version. GitHub supplies the download URL and SHA-256; no
separate manifest is required. If multiple matching ZIPs exist, the newest
numeric version wins. Old ZIP names without the full version are reported and
not downloaded automatically. The Rolling channel is available after its first publication.

## 12. Local configuration

**Config** exposes the conversation line limit (minimum 100), resume turn count
(1–100), conversation cache toggle, Activity row limit (minimum 1), conversation
color presets, update channel and check
frequencies. The three presets are Classic charcoal, Cool slate and Warm
graphite; all keep the existing dark application theme. **Keep current colors**
preserves any colors edited manually in the INI. Smaller limits trim the visible
content; raising a limit does not restore already discarded rows until history
is loaded again. Colors apply to the visible conversation, deferred until the
current streaming block is complete if needed.

The Download directory is remembered automatically by the link Download command
and is intentionally not exposed in Config. Internal options and Codex server
settings are not exposed there either.

TMI Lite+ uses:

- the INI beside `TMILitePlus.exe` for UI preferences, selected
  project/thread, accepted EULA version and fingerprint, rolling limits,
  conversation colors, download directory, update channel/frequencies and
  last-check timestamps;
- `HKEY_CURRENT_USER\Software\TMILitePlus` for common Codex-related settings;
- shared memory only for lightweight multi-instance coordination;
- `TMILitePlus.startup.log` for the current diagnostic session.

The first instance overwrites the startup log. Concurrent instances append to
the same log with their process IDs.

## 13. Troubleshooting

- If **Send** is disabled, connect the server first.
- If Codex CLI is missing or older than 0.157.1, open **CLI Setup**.
- If a conversation reports that it is already open, close it in the other
  TMI Lite+ instance.
- If a project is absent, refresh after Codex thread discovery completes.
- If a thread is very large, the rolling conversation loads first; use
  **Full Load** only when the complete history is needed.
- For startup or protocol problems, copy `TMILitePlus.startup.log` before the
  next first-instance startup overwrites it.
- For protocol incompatibility, regenerate and review the app-server schemas.

## 14. Legal and privacy

Read:

- [EULA](EULA.md)
- [Legal notice](LEGAL_NOTICE.md)
- [Privacy notice](PRIVACY.md)
- [Third-party notices](THIRD_PARTY_NOTICES.md)
