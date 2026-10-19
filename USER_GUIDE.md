# TMI Lite+ for AI agents — User Guide

Version 2.2  
Stable release: 19 October 2026  
Tested Codex CLI: 0.163.0

TMI Lite+ is a lightweight independent Windows client for Codex CLI. It does
not provide an AI model, account or subscription. It is hobby-developed and
free of charge; lawful personal or professional use is allowed entirely at
the user's discretion and risk. Read the supplied EULA and notices.

## 1. Install and start

Extract the complete Win64 package into a writable directory. Keep
`TMILitePlus.exe`, `TMIHost.exe`, `TMIDesktop.exe`,
`TMIMaintenance.exe`, documentation and viewer assets together.
All four executables must be from the same bundle (local IPC protocol 16).
Start `TMILitePlus.exe`; it starts/connects to the resident host as needed.
Accept the versioned EULA when requested. Acceptance stores its version and
SHA-256 fingerprint; a changed EULA version or text asks for acceptance again.

Codex CLI and its authentication are separate. If it is missing, the host opens
CLI Setup before enabling Codex connections. Use the displayed install/update
commands or run them manually, then authenticate using Codex CLI.

For **1.x to 2.x**, upgrade manually: finish work, close old frontend instances,
exit any running host, back up settings/Codex data and extract the full 2.2 ZIP.
The old single-executable updater is not a four-binary 2.x installer. Do not
overwrite an executable while it is running.

You may also start `TMIHost.exe` directly. It stays in the tray without requiring
a frontend and can open Maintenance for initial CLI setup. A disconnected
frontend waits for host availability and cannot submit prompts.

## 2. Projects, threads and working directories

The **Show Threads** toggle switches the grouped browser over the left sidebar;
its crossed-out state shows the current thread's agents and changed files.
**New**, **Sessions** and the search-icon button remain visible in the same
command row. Click a project to expand/collapse it and a thread to open it. Project,
thread title and working directory are shown separately. Project assignment
does not move files or change a thread's working directory.

The browser order is **Pinned**, projects in their normal recent-activity
order, then **Unassigned**, **Documents** and **Archived**.

- **Pinned** opens automatically once per frontend startup. A pinned thread
  remains under its normal project too; both rows open the same thread, not
  another writer. Manual expansion/collapse survives refreshes.
- **Unassigned** contains active threads without a project, except those
  presented in Documents.
- **Documents** includes the current user's Windows-known Documents folder
  and every descendant, including redirected paths. It takes presentation
  precedence over an old project ID but preserves the real working directory
  and does not clear that ID.
- **Archived** is always available at the bottom. Expand it to load archived
  metadata. Selecting an archived row shows information without resuming it;
  its context menu offers **Unarchive** and **Delete**, with confirmation.

The search icon opens a modeless **Search** window. Enter a query and press Enter
or Search: title matches arrive first, followed by single-line message excerpts.
All projects and Unassigned are searched; Include archived also searches archives.
Title matching is case-insensitive. Stop keeps results already received.
There is one excerpt per matching thread. Click a title to open its thread; click
an excerpt to open that thread and a separate historical view at the match.
Only the selected historical turn is fetched. If the original text is unavailable
or ambiguous, the exact server excerpt is shown with an explicit context note.
Full Load remains available and unchanged. Thread rows show titles without pin/date
prefixes; last-activity date and time are in their tooltip, formatted by Windows.

Right-click actions apply to the row actually clicked. **New**
chooses a directory and waits for the first prompt before creating a thread.
Global/special-group New prepares an unassigned thread; New in a real project
keeps that project ID. Choosing a directory does not create a project.
**Duplicate (fork)** retains the source project independently of the chosen
working directory and requires an initial prompt. Rename and project assignment
are server-backed operations. Named project creation is explicit.

**Project Roots**, opened with the working-directory **Project Roots** button, lists roots
and refreshes them from project threads. Add a root using the edit/folder dialog,
remove unused roots, or assign the selected root to the current thread.
A used root cannot be removed. Changing cwd does not invent a replacement
project or change the thread's project ID.

**Spawn agents** shows saved descendants of the open thread, including nested
agents, with nickname and role when supplied by Codex. It is independent of the
browser's selected row. **Subagents** shows live agent activity and clears when
the current turn finishes; an empty live list does not mean there are no saved
agent threads. Agent metadata is loaded only for the open thread, without polling
or a full-history scan.

Double-click an agent in either list to open its conversation snapshot in a
separate read-only window with text search. TMI sends `thread/read` through the
parent's running Codex process; it does not resume the agent, detach the parent
thread, or switch the parent writer.
Inspection is also available while the parent is working. The window shows the
history received for that request, not a continuously updated live transcript.
Stop or close the window to cancel local preparation; late replies are ignored.

Only after an explicit expansion of a real project returns zero active threads
does TMI scan unassigned threads for automatic assignment, at most once per
frontend startup. It assigns unique existing-root matches only, excluding
Documents and descendants; it never creates or deletes projects. Ordinary
refreshes, automatic expansion and display filtering do not trigger the scan.
The separate manual **Assign unassigned threads to projects...** command shows
the complete proposal before accepting/cancelling it. Only acceptance permits
the proposed new projects; shared paths reuse the same project.

## 3. Send, steer, queue and settings

Write in the prompt editor. **Ctrl+Enter** uses its normal send/steer action
there; Enter in another editor does not send a prompt. Append queues ordinary
follow-up input; Steer sends input into the current work. Stop interrupts the
turn. TMI does not expose/override Codex's experimental `instant_interrupt`.
In CLI 0.163.0 that feature is enabled by default but remains experimental;
Code Mode interruption is stable and enabled. Effectiveness is determined by
Codex and its current operation, not by an extra TMI interrupt command.

**Enqueue** and **Queue (n)** use the persistent server-owned prompt queue.
Its panel reads, edits, removes, reorders or starts queued prompts. The next
prompt has a compact preview. The native queue has no pause control; local
Schedules, described below, are separate.

Attachments may be selected with Files or dropped on the prompt.

**Goal** submits the text currently in the prompt editor as a new active Goal;
it does not change Append, Steer or Ctrl+Enter into a Goal command. Those actions
always send ordinary prompts. Goal uses the normal button color and is available
when no Goal exists or the previous Goal is complete, subject to the usual
connection and thread requirements. After Codex confirms the Goal, its text
starts a turn when idle or steers the current turn when work is already running.
Goal is disabled while a Goal is active, paused or otherwise unresolved. Failed
Goal submission retains the draft.

The Goal row shows its native state and provides edit, pause/resume and delete
actions independently of the new-Goal button. Its background is green while
active, yellow while paused, blue when complete, red when blocked, and orange
when usage- or budget-limited. Changing the objective of a completed Goal
reactivates it through Codex; accepting the identical text does nothing and does
not reset its usage. Goal management is also available during a running turn
when the server and the requested operation are ready.

Agent follow-up links paste text only after confirming any draft overwrite;
they never send it automatically. Question links open the Questions window.

Model/reasoning and confirmed settings come from native thread metadata.
Manual changes send only their relevant settings, preserving unrelated
permissions. Rejected changes restore confirmed values. An acknowledgement is
not a promise that a model/reasoning change has affected an already-running step.

Fast, Auto-review, Network access and Poll use compact toggle buttons.
On uses the active color; Off uses the normal background with a thin diagonal
red slash and readable text. Poll is idle limit polling; its hint shows the
configured interval. Fast reflects Codex's available service tier, not a
guarantee of speed or model availability.

**Schedules**, beside Goal, manages local scheduled prompts owned by TMI Host.
It turns green when at least one schedule is active. It opens the schedules
panel in the left-hand thread-list area, or opens **New schedule** directly
when no schedules exist. The panel lists upcoming activations separately for
repeating schedules. **Enable/Disable** and **Delete** on a row affect the
entire schedule, not only that occurrence; deleting asks for confirmation.
Disabling prevents future activations; it does not withdraw a prompt already
claimed for delivery or accepted by Codex.

Choose **New...** or double-click a row to edit its name, prompt, start date/time,
recurrence and delivery mode. A schedule normally targets the current thread;
choose another known thread or enter a thread ID to target it explicitly.
Alternatively, choose a new thread at each activation, its project and working
directory. Delivery is **append** by default (queue), **steer** (guide current
work), or **absolute** (stop current work and send). Local time and UTC are
available. The simple repeat selector supports minutes, hours, days and weeks;
the recurrence-rule field also supports multiple times and weekdays.
Supported rules use SECONDLY, MINUTELY, HOURLY, DAILY or WEEKLY; monthly/yearly
rules are not supported. Unsupported rule fields are reported rather than ignored.

Host must be running for local schedules to execute, but no frontend has to stay
open. After startup or CLI Maintenance, scheduled delivery waits for CLI readiness
and initial connection/loading requests to finish, then for 20 seconds of quiet.
Definitions remain available during this wait; occurrences due during the wait
are retained. **Send missed now** reports that startup is still settling if used
before delivery is enabled. Ordinary work after this startup gate opens does not
restart the delay. Its tray menu pauses/resumes all scheduling. One-shot deadlines missed
while unavailable or because delivery failed remain marked **missed**. Open
their editor to **Send missed now**, or choose a future start and Save to
reschedule. Missed recurring occurrences appear only in that schedule's
execution log; future activations continue normally. This local scheduler is
separate from Codex's persistent prompt queue and from cloud automations.
Definitions and logs are stored per Windows user in
`%LOCALAPPDATA%\TMILitePlus\schedules.json`. Interrupted unconfirmed deliveries
are marked missed after restart, never replayed automatically. Save failures
stop scheduling and are reported in the schedules panel.

Prompts cannot be submitted while disconnected. Drafts are not discarded merely
because a native connection is unavailable.

## 4. Resident work, Sessions and tray

The resident `TMIHost.exe` owns one independent Codex app-server process per work.
Switching a thread or closing its frontend detaches it; work and pending native
requests may continue. Closing a frontend does not exit the host.

On startup/reconnect, a frontend can reattach to an available resident work,
preferring the last-used thread. It does not take over a work attached elsewhere
and does not create a second native resume/writer. Threads attached to another
TMI frontend cannot be opened for conflicting work. Selecting one activates its
existing TMI window instead, restoring it if minimized and preserving the
current frontend's conversation and draft. If Codex reports that another
application owns the writer, TMI explains that the thread is unavailable and
asks you to close it there before trying again.

The frontend **Connect / Disconnect** button affects only its selected
conversation. Disconnect closes its resident work and releases the Codex writer,
then shows thread selection. An idle work closes directly; a running task requires
confirmation because closing interrupts it. The frontend stays connected to the
host: projects, search and Sessions remain available. The unsent prompt and its
attachments are preserved. The closed conversation can be archived or deleted;
selecting it again opens a new resident work. Switching threads or closing a
frontend still only detaches its view and leaves resident work running.
The button does not restart the host or other works; while the host is in
maintenance the frontend waits for its authoritative ready state.

**Sessions** shows resident title, project, cwd, state, ownership and pending
requests. Closing Sessions does not stop work. Explicit **Close work** closes
a detached resident work and requires confirmation. For an attached work, use
**Disconnect** in its owning frontend.

Projects containing resident works expand automatically. Idle, working,
waiting and error states use soft colors and text. Pinned is not expanded or
collapsed by this work-state logic. Taskbar text identifies the active thread;
idle omits the former `[I]` prefix.

A tray left-click opens/raises a frontend or reattaches a detached work needing
attention. The tray **Open TMI Lite+** command always opens a new unselected
frontend. Pending-request menu entries and notifications open their exact
owning thread. Working/error indicators remain visible even with a frontend
in the foreground; duplicate attention notifications are avoided for that
frontend.

Use **Exit TMI host** to stop the backend and its Codex processes. Idle exit
does not require confirmation; active work may require it. Shutdown has a
shared five-second deadline. Applications merely observed/controlled by TMI
are not closed. Windows shutdown has been tested.

## 5. Approvals and Questions

Thread approval/question forms belong to the owning frontend, not the host.
If no frontend is attached, a pending native request stays pending; it is not
silently accepted. Tray selection opens the appropriate frontend.

Choices are shown only when offered by Codex. Empty MCP forms do not need
hand-written JSON. Suggested answers and free text are readable/selectable.
Closing a frontend does not make a decision. Only one correlated response can
be sent; uncertain submissions are not resent automatically.

Conversation Questions can be answered from their window or dismissed when
already answered by an ordinary prompt. Dismissal is session-local. When all
answers have been submitted, the Questions window closes. A question does not
always mean the agent is waiting or that the task must be interrupted.
Press Ctrl+Enter in the Questions window to send the selected or written reply.
The shortcut does not insert a newline; ordinary Enter keeps its normal behavior.

## 6. Computer Use and permissions

TMI's local `tmi_desktop` MCP tools are independent of the ChatGPT desktop app's
private Computer Use service. They discover windows, read bounded text or PNG
images, launch applications and interact with controls.

Read and control consent are separate. Grant access only to applications/data
you are authorized to use. Permanent grants are scoped to executable paths and
revocable in Computer Use. Codex approval/auto-review settings do not grant
desktop permissions. The explicit global-read switch is off by default,
persists until revoked and grants reads/captures, not input or launch.
TMI windows can be read for debugging but are not agent control targets.

A visible warning precedes authorized content reads/captures and control.
Each monitor gets a click-through pulsing border, 1% of its width and 50%
opacity: **blue for passive reads, red for control**. Red remains priority
through later reads in that control session. The opaque matching banner says
**Codex is using your computer (ESC to interrupt)**. It does not blink, take
focus or appear in the taskbar; it moves away from the pointer and remains
visible for at least two seconds. There is no idle screen capture/animation.

**Physical ESC interrupts the current Computer Use operation**, releases held
synthetic input and returns `user_interrupted` to the agent. It does not
permanently disable Computer Use; a new request is possible. Do not repeat the
interrupted action automatically. Agent-sent Escape to a target is different.
Revoking permissions is separate and does not undo completed actions.

For debugging/minor operations, native mode reads Win32 controls and uses
direct window messages for supported clicks, scroll and text replacement.
TMI does not activate the window or move the physical pointer. Covered controls
can be targeted; minimized/disabled/stale controls remain constrained by Windows.
The target application itself may change focus in its handler.

For application workflows, semantic mode uses UIA elements and actions.
UIA mutations require compatible foreground. Physical mouse/typing/keys/drag
are explicit last alternatives and require a usable foreground. There is no
silent mode fallback, implicit activation or retry after uncertain delivery.
Activation is a separate explicit tool action.

Ask for a relevant window/control rather than a full tree or full desktop.
Discovery can filter title/application; reads can target a named control,
focused element or subtree. Text/output limits and cropped/resized images
bound returned data. Truncation is explicit. Fresh observations identify
targets; native handles remain private. Unsupported actions/capture and
cancelled calls return explicit outcomes.

Password values are excluded from accessible reads, but screenshots may show
confidential data. Text/images returned to Codex may be sent to its AI service
and retained in history. Use these tools only with informed consent. No
hidden-access mode is offered, and warnings are not protection against modified
executables or other software.

`TMIHost.exe --mcp-stdio` instances are adapters, not duplicate tray hosts.
Temporary TMIDesktop workers isolate UIA/capture; they do not own target apps.

## 7. History, files, tokens and Usage

Normal opening loads a bounded recent history, not months of tool output/diffs.
The rolling conversation limit and resume-turn count are configurable. A valid
optional cache does not replace server readiness. The full-history **Full Load**
window is retained, including search, speaker/timestamp data and partial output
when its loading is stopped.

With **Use conversation history cache** disabled, persisted history is requested
from Codex, without reading or writing the frontend history cache. The Host's
in-memory live replay is separate and still restores the running turn on reattach.
Reload date/token-baseline lines wait until restoration finishes and are omitted
when the turn is still active; normal completed-task summaries are unchanged.

The live conversation follows new output until manual scrolling/navigation
disables autoscroll. Its down-arrow returns to the end and re-enables it.

MCP tools that send progress messages update the current Activity status in
place, without adding a log row for each message. **Retrying...** means Codex
reported a recoverable error and intends to retry; **Error** identifies an error
without that recovery indication. A displayed retry count is not a guarantee
that Codex has finished recovering. Retry notices stay in Activity/status,
without interrupting a streaming message. TMI does not add its own retry loop.

Changed Files remains enabled without sending a prompt, including while
disconnected. Its context menu starts with Diff, then Open; double-click opens
the file normally. Changed Files and compact Activity diff markers open the
same nonmodal diff viewer, scoped to the conversation and ordered by
task/session/day. Opening it selects the latest edit automatically, including
all edits with the same timestamp to the second. Select several diffs with
Ctrl/Shift to view them together. File links support default open, Show in
Explorer and Download; Download remembers its chosen location.

Context and account limits are native readings. Limits+ keeps additional
buckets in the hint. Reset-credit count/earliest expiry appear only when supplied;
the Token warning highlights relevant expiry or exhausted limits. Missing data
is not invented. Turn-end summaries distinguish consumed tokens from input/output
and conversation totals.

TMI does not enable experimental background compaction. Manual Compact remains
available; no compaction test switch or automatic activation is added here.
Native read/unread result metadata is separate from conversation retention:
this update adds no read-state control or automatic mark-as-read operation.
Normal history and Full Load keep the fast summary view for turns with a final
answer. When a loaded turn has no final answer, TMI additionally pages that
turn's persisted messages, including commentary and partial answers. It keeps
their order and does not retain historical tool/diff payloads from this recovery.
Normal line/turn limits and Full Load's Stop/memory limit remain in effect.
A complete live replay replaces the same turn during reattachment, without
duplicating its messages. Old summary-only caches are invalidated. Text never
persisted before an abrupt process loss cannot be reconstructed by this feature.

**Usage** is an on-demand statistics window, separate from live context/limits:
7-day and 30-day bars, weekday min/mean/max comparisons, and an extended
Monday-first calendar heatmap. Missing days count as zero in weekday averages.
The source date range, selectable start date, navigation arrows and hover
token values support visual analysis. Statistics are not proof of model quality
or exact billing cost.

**Worktime last (1%) / current**, above Refresh and Usage beside Poll, is a local
approximate statistic calculated by the host. Working time is summed across all
resident threads, including detached threads; approval/input waits do not count.
Two threads working for one minute therefore contribute two minutes. Each whole
percentage decrease of the shared main subscription limit records a time sample.
A multi-point decrease remains one sample; its time is divided by the number
of percentage points, so every displayed value is an average per 1%. This is not a token
price or billing estimate and does not measure use on other computers or online.

Click the Worktime text to open a nonmodal graph, created only on request.
Current, Past and All select the retained quota-reset cycles. The last 200
observed decreases are retained across cycles, one row per sample, newest first.
Measured zero time remains a real observation. Older per-point data is retained
as one-point samples when the saved format is upgraded.
The next automatic-reset timestamp identifies a quota cycle approximately:
differences below 60 seconds are treated as estimation/rounding unless the
remaining quota increases to 99% or 100%, indicating a reset. An unchanged
deadline or a stable/decreasing quota does not trigger that small-change reset.
A different deadline can start a cycle in either direction; a shorter
new deadline is valid. Snapshots matching a known earlier cycle are ignored.
Within a cycle the host keeps the lowest observed
remaining percentage, so an outdated value from another thread cannot create
a reset or count the same percentage decrease twice. Each resident thread has
its own already-counted Working total; only new time is added to the shared
accumulator. A missing reset timestamp does not prove a new cycle.
After restart the host restores the known limit identity from saved statistics.
If an update omits unchanged metadata, only already-known identity/window
metadata is reused; a new usage percentage is still required.
Pending time at a reset stays separate from completed samples. Reset metadata
and these residues are bounded to 201 retained cycles; completed samples keep
their independent 200-value allowance even if many empty resets occur.
The current cycle and earlier cycles are saved by the host in the shared Windows
registry, separately from UI preferences. Existing Usage and Thread usage data
remain unchanged. Click the Worktime text again to read a fresh graph snapshot.

## 8. Optional graphics

Unicode formula text works without a renderer. Graphical LaTeX requires the
external renderer in [MicroTeX Setup](MICROTEX_SETUP.md); MicroTeX binaries/fonts
are not distributed. Once available, completed formula blocks render locally.
Open formula opens the cached image with fit/original size, zoom and Copy image.
The image viewer uses the same clipboard action. Full Load remains text-only.

Reloading a conversation automatically renders only its newest 3 graphic
occurrences in total (available LaTeX, Mermaid and Visualize). Older graphics
stay textual and can be opened on request, without starting their renderers
during loading. Resize keeps this selection, even for repeated identical
diagrams. New live messages retain normal automatic previews.

Change **Graphic previews at reload** in Configuration, or set
`HistoryGraphicPreviewLimit` under `[Conversation]` in the local INI. The default
is 3; 0 disables automatic graphics on reload. Changes apply at the next reload.

A completed Visualize reference receives an automatic inline bitmap preview.
Click **Open visualization** (or **View visualization** before capture) to open
the nonmodal interactive WebView2 viewer. Resizing reuses the cached bitmap,
not the running widget. Update preview copies the current viewport; Copy image
places it on the clipboard. Closing the viewer does not rebuild history.
Preview/widget state are memory-only.

Visualize requires the separately installed Microsoft Edge WebView2 Runtime
and the supplied loader/assets. Its UTF-8 fragment must exist locally and be
at most 1 MB. Missing dependencies are reported on use; the browser initializes
only when there is graphic content to render, including retained rolling history.

Widgets run in an opaque-origin sandbox. Native objects/commands, arbitrary
navigation, downloads, browser permissions, arbitrary file resources and
fetch/XHR are blocked. Allowed static CDN resources may make HTTPS requests.
Follow-ups ask before replacing a draft and never submit it automatically.
Basic styles/icons/tabs/tooltips/state/follow-up APIs are supported; host-only
Tweak/calendar and generic MCP Apps are not included.

Completed assistant code blocks marked `mermaid` render automatically as inline
diagrams. Incomplete streaming blocks and ordinary code examples remain text.
**Open diagram** opens the separate viewer for zoom, scrolling and Copy image;
opening it does not replace the complete inline preview with a cropped viewport.
Resize uses the cached bitmap, while Full Load keeps the original source text.
If rendering fails, the fenced source remains visible with an explanatory link.

Mermaid uses the supplied local library and adapter, plus WebView2. It does not
contact an online renderer or load CDN resources. Diagrams run in strict mode
without native commands, network access or graph click actions. Source is
limited to 50,000 characters and 500 edges, with a 15-second automatic rendering
deadline. See [Third-Party Notices](THIRD_PARTY_NOTICES.md) for component licenses.

## 9. Preferences, maintenance and updates

UI preferences are local in `TMILitePlus.ini`: rolling/activity/resume limits,
cache, conversation colors, update channel/schedule, polling, last selection,
download destination and EULA acceptance. Defaults initialize first; missing
INI entries use their documented defaults, while read errors are reported.
Configuration separates conversation/interface preferences from application
updates and their commands. Descriptive selectors control cached-history use
and whether to hide the thread list after selection. It also exposes the graphic
preview limit at reload, alongside the rolling-line, activity-row and resume-turn
limits, color presets and update-channel/check-frequency selectors.

The host writes common defaults in `HKCU\Software\TMILitePlus`, including
Codex path and common permission defaults. Frontends submit changes rather than
rewriting stale values. Model/reasoning and native thread metadata remain
Codex-owned, not duplicated common defaults.

The host owns Codex detection and startup/six-hour version checks, even without
a frontend. Frontends use its shared cached status; Recheck requests a fresh
check. **CLI Setup** asks the host to open on-demand `TMIMaintenance.exe`.
While Maintenance is open, idle catalog/work processes are disconnected and
frontend drafts remain intact. After closing it, the host restores only thread
sessions currently attached to frontends, without replaying prompts. Detached
idle work and catalog connections remain suspended until actually requested.
Each restoration concludes success/error with a bounded wait; one failed
thread does not leave other threads offline. Working/opening/waiting work
prevents maintenance. A failed selected connection can be retried with Connect.

Maintenance can also run alone, without host/frontend. It enforces a single
maintenance window and refuses unsafe connected-operation conditions. Internal
Thread Transfer is not part of the normal public UI.

Release and Rolling are independent opt-in channels in Configuration.
Checks use the selected channel/schedule and run in the background; installing
an available update requires confirmation. Their visible names and frequency
choices are unchanged, but **2.x Release uses `stable-2` and 2.x Rolling uses
`rolling-2`**, separate from legacy 1.x Latest/`rolling`. The
[2.x Rolling release](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/rolling-2)
contains the independent rolling bundle and is not replaced by stable 2.2.
GitHub's Latest numbered release is v2.2; the direct
[2.2 release link](https://github.com/SbiriJJ/tmi-lite-plus/releases/tag/v2.2)
also provides the full bundle for initial installation. Earlier versioned ZIPs,
legacy 1.x assets and the legacy `rolling` endpoint remain unchanged. The old
single-EXE updater does not recognize 2.x bundle filenames.

This release uses `TMILitePlusBundle-2.2.G.T-Win64.zip`; remote 2.x ZIPs stage the four
matching executables, supplied runtime assets and documentation together.
They are not packages for the 1.x updater. Personal INI settings are never
overwritten. For local 2.x updates, place matching
staged files in `update\\`: newer numeric Windows file versions are required
for binaries, changed data/docs are compared by content. Personal INI settings
are not replaced. Restart and Update is coordinated with the host and refuses
active work/requests or running maintenance; idle frontends are allowed.

Never copy a lone incompatible binary over a running bundle. Keep backups.
Do not use TMI Lite+ and the **ChatGPT desktop app on the same project at the
same time**; TMI provides no cross-client coordination.
