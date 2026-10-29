# TMI Lite+ for AI agents — Development Notes

Document version 2.3  
Current 2.x Rolling build: 2.3.9798.40375 — 29 October 2026  
Stable 2.3: 2.3.9797.42811 — 28 October 2026  
Tested Codex CLI: 0.166.0; earlier installed-version and focused checks also covered 0.164.0 and 0.165.0  
Local IPC protocol: 16

## Version 2.3 architecture

The host captures the selected executable/version pair before publishing local
readiness; an obsolete frontend path hint cannot select another CLI after
Maintenance. Local version discovery is bounded to three seconds and an unknown
version does not prevent app-server admission. Help, network and
startup-configuration inspection remain advisory. Probes and pipe reads are bounded;
a descendant holding stdout open cannot defeat the probe deadline. Optional
inspection failures remain advisory. Only an actual app-server launch or
initialization failure blocks that native connection. Native startup inspection
runs once per host; its structured `appServerVersion`/`configRequirements`
reply is retained in CLI status with a 15-second cancelled-process deadline.

Installed-version selection reads package metadata, validates only the chosen
executable, and commits only its shared path. Successful installation selects
installer/current automatically. Update availability compares the remote release
with the newest installed version, not the runtime selection. Configurable
previous-minor/age cleanup uses publication dates supplied in memory; unknown
dates keep the affected package and produce a warning. No per-install ledger is
created and no unrelated settings, processes or Codex data are deleted.
Install and Update resolve availability through the official release channel,
capture the displayed version, and call the official installer with `-Release`
set to that exact version. The resulting executable must match the captured
target before selection persistence or retention cleanup.

CLI Setup's designer-owned Network diagnostics action requires the selected
CLI version to be at least 0.166.0 and an idle Maintenance reservation. Its
existing worker launches only `doctor --network-only --no-color --ascii`,
streams stdout/stderr into the existing output memo, and enforces a 60-second
deadline. The diagnostics branch exits before update lookup, installed-package
enumeration, selection persistence or retention. It never runs automatically.

Host-ready executable notifications update both the observed frontend value and
that one shared baseline field, rather than fabricating an unsaved frontend edit.
Real conflicts on other changed fields remain guarded. An obsolete thread and
obsolete configured model resolve only against advertised models; numeric
versions rank newest first and equal versions retain catalog order. A valid
manual choice can resume the failed target, not create an unintended new thread.
Configuration and `config.toml` defaults are not rewritten by this recovery.

`feedback/save` runs on the catalog connection, with explicit log/thread consent
and an unused absolute ZIP destination. Codex owns archive creation. A disconnect
does not imply the archive was not written; no automatic resend is made.

`thread/sample` runs on the selected work's connection. Its nonvisual request state
first reads only the latest turn metadata (descending, limit 1, no items), then
supplies `expectedTurnId`. Cancellation translates the UI request ID to the native
ID on that same connection, including cancellation requested before acceptance.
Results are consumed before normal turn handling; source change, Maintenance,
disconnect or frontend closure cancels/invalidates them. Activity shows its phases
and independent elapsed time; the main conversation renderer receives its local
question/answer/usage blocks. These survive resize but not a history reload.
Cache serialization and Full Load read server pages, never those display blocks.
Normal token baselines and the existing global Working-time counter are unchanged.
The prompt byte counter is display-only outside the Sample limit guard.

Native startup output and Sample inference were checked with installed Codex
0.164; selecting installed 0.164/0.165 executables and focused native contracts
were also checked. The 29 October rolling adds targeted Codex 0.166 integration
checks and filtered Win64 Release builds. The diagnostics button, guards and
command were tested offline; the network doctor was not run against the real
account. This is not exhaustive certification of every Codex feature.
Fresh-install authentication is still a separate `codex login` step; TMI does
not yet provide a native login screen.

The resident TMIHost owns independent Codex app-server processes for catalog
and conversation work. Frontend detachment does not stop work; reattachment
reuses its writer. Native approvals/questions are routed to the owning GUI,
not shown or automatically answered by the host. All four matching binaries
must be installed together. Version components G.T remain compiler-generated.

Remote 2.x update routing is separate from legacy 1.x: Release reads `stable-2`,
Rolling reads `rolling-2`. Stable 2.3 uses `stable-2` and remains GitHub's Latest
numbered release. This build advances only the independent 2.x Rolling package;
Stable 2.3, legacy 1.x assets and the legacy `rolling` endpoint stay unchanged. The direct versioned
release links to the full bundle ZIP, named
`TMILitePlusBundle-2.3.G.T-Win64.zip`. The old single-EXE package recognizer does
not accept this bundle. The 2.x extractor stages all four matching EXEs and
the package's approved runtime/documentation files; personal INI is excluded.
Fixture validation of extraction/staging is distinct from a live network update.

Local scheduled prompts are persisted separately in
`%LOCALAPPDATA%\TMILitePlus\schedules.json` and dispatched by the host's
event/deadline worker, without a frontend dependency. Startup/maintenance
delivery waits for readiness and loading to settle, then 20 seconds of quiet;
intentional waiting does not mark due occurrences missed. Lists stay readable.
The current protocol validates sequenced `host/schedules` events; an unexpected
stream-sender failure closes its pipe so a subscriber cannot wait indefinitely.
Disabling a schedule also prevents dispatch of its queued jobs while retaining
coherent completion and log state.

TMIMaintenance is loaded only on demand and can run independently. The host
suspends idle Codex connections while maintenance is open and afterwards restores
only thread sessions currently owned by frontends, without prompt replay. Other
idle work and catalogs restore on demand. Restoration is bounded and reports a
terminal error per work, rather than leaving the host globally offline.
Frontend Connect/Disconnect attaches/detaches only the selected conversation.
Working/opening/waiting sessions block maintenance. Initial CLI setup is
host-owned and precedes native connections.

tmi_desktop uses native control observations/window messages by default for
minor/background operations. Semantic UIA mutations and physical input are
explicit alternatives with foreground requirements; uncertain input is not
silently retried. Read/control consent, bounded observations, visible blue/red
indicators and per-operation physical ESC cancellation are separate from Codex
approval policy. ESC does not permanently disable the next request.

Visualize and Mermaid initialize WebView2 only when a preview or viewer is
needed. Reload budgets are computed before rendering: the newest 3 graphic
occurrences share one configurable budget across LaTeX, Mermaid and Visualize;
0 skips automatic history graphics. Older occurrences remain text with manual
links, including repeated sources that share a cached bitmap. Resize retains
the same selection; live messages remain unrestricted. A shared source reader
keeps counting, formatting and preview queue positions aligned.

The sandboxed HTML viewer hosts interactive inline Visualize content with
DOM-height notifications, rather than a static screenshot. Each occurrence
retains separate state; offscreen views suspend and resume. Mermaid provides
local zoom/pan/reset with a pinned runtime and network access disabled.
Independent viewers retain Reload, zoom and clipboard copying without
rebuilding the conversation on close. WebView2 Runtime is separate; MicroTeX
remains external.
Full Load, Usage, diffs, attachments, Questions and native queues remain available.

Configuration groups preferences separately from update settings/commands and
uses descriptive selectors. Cross-frontend thread events update local recency
ordering without extra history reads. Questions handles Ctrl+Enter through the
same reply action while suppressing the generated newline. Frontend startup
presents its visible window before beginning host connection/startup.

Focused tests and installed-bundle use covered reattachment, multi-frontend
operation, desktop actions and dual-monitor warnings, maintenance boundaries,
Windows shutdown, Full Load, Usage and attachments. Pinned/Archived and shared
list refresh have passed practical multi-frontend use. The one-off tray AV has
not recurred, but its original occurrence was not reproduced. This is not an
exhaustive certification of every Windows app or future CLI build.

### Added local/state flows

- Notes keep their own Markdown files by working directory. The project-wide
  directory discovery loads metadata, not transcript history, and changes no
  Codex project assignments. Save validates external file changes; no polling.
- Auto switch is host-owned and temporary until host restart. It selects existing
  frontends with explicit priorities and excludes minimized/error candidates.
  Native taskbar attention and independent tray badges share normal work events.
- Working-time statistics consume active-thread Limits notifications only;
  idle account polling remains indicator-only. Deadline changes below 60 seconds
  do not start a cycle even when remaining quota rises to 99/100%.
- Memory reads are native shared-summary reads, not context dumps. Configuration
  uses native version-guarded config writes and verifies effective values. No
  TMI memory-generation loop, separate memory store or assumed hot reload exists.
  The adjacent CLI label uses the executable/version pair of the selected active
  work, not the newest installed or remotely available release.
- Live IDE Bridge `fileChanges` reports feed the existing Changed Files model.
  Artifacts are immutable/lazy-read and size/hash checked. Save-only reports
  preserve content edits; no history import, source reread or agent readback.
- Codex capacity/retry warnings are retained as response state, distinct from
  local worker failures. TMI does not add a retry loop or force-stop the turn.
  `policyCapacityUnavailable` maps to **Capacity unavailable**; an explicit native
  retry indication still takes precedence. Closing-thread resume rejection is
  handled without a client retry loop or message-string matching.
- Startup marketplace recommendation prefetch is disabled using the supported
  CLI launch setting. This is independent of Browser Tools and does not remove
  installed browser/plugin functionality or rewrite the saved browser policy.
- Process/output-reader failure invalidates the dead session's turn and pending
  requests, preserving diagnostics and permitting explicit reconnect without
  prompt replay. A failed New-work opening retains its first frontend draft.
- Downloads have explicit cancellation and callback ownership. The context-menu
  action becomes **Cancel download** while busy; frontend closure cancels/joins
  under its existing five-second deadline. Failed/cancelled downloads preserve
  the destination and clean up temporary files. Tests used local/loopback input,
  not external network access.

## Internal Thread Transfer — ZIP format 2

Thread Transfer remains an internal, experimental maintenance operation. It
uses short-lived local app-server connections for native project/thread
metadata; it never sends a prompt or starts inference. TMI work connections
remain suspended while Maintenance is open. File replacement remains offline.

The new `tmi-thread-transfer` format, version 2, contains a manifest and one
complete rollout. Older ZIP formats are rejected, without migration. The
manifest records the current native title, working directory, source dates,
model/reasoning and informational project name/roots. It does not transfer
machine-specific project IDs or global permission settings, source files,
local attachments, or agent conversations.

Only the normalized first user prompt identifies an existing conversation.
IDs and titles are not identity keys. Exact matches and overlapping names or
prompts produce a warning with source/target dates; an overlap alone never
selects an update target. New imports allow an edited name, an existing/new
destination project and an existing destination working directory. An update
defaults to the target's current values, including its model, reasoning and
archived state, with name/project/directory editable explicitly.

Confirmed updates may replace a divergent or newer local history. An exact
backup is retained and replacement is atomic; there is no automatic merge.
Native metadata is then restored through app-server. If that separate step
fails, the UI reports possible partial completion and retains the backup,
original ZIP and staged history. This is not a transactional cross-computer
sync mechanism.

New imports are durably staged for the next frontend connection. Project
creation uses a stable idempotency key; the host records the new thread ID
before completion. Retrying a partially completed import resumes that ID
instead of forking again. Native project/settings/name confirmation precedes
removal of the pending entry and staged file. Fork requests exclude history
from the response and defer automatic Goal continuation. Working-directory
changes use native thread settings and never infer or create a new project.

Focused verification includes ZIP/integrity/backup fixtures, a fake app-server,
a real DFM form fixture, pending-import contract checks and read-only metadata
queries against the installed CLI. Import/replacement of real user threads is
deliberately excluded from automated verification.

## Version 2.0 browser compatibility — CLI 0.162.0

The following records current implementation details. Historical release ZIPs
are unchanged. See Release Notes and User Guide for public operational guidance.

- Browser group identity is separate from server project identity. Pinned and
  Archived are presentation scopes, not projects created in Codex or in local
  settings. One canonical thread ID backs its project row and any Pinned alias;
  updates are reflected in all representations without duplicate resume/writers.
- Group order is Pinned, real projects in their normal order, Unassigned,
  Documents, Archived. Pinned is initially open per frontend and thereafter
  changes only through user/search actions, not work-state expansion logic.
  Special groups remain available when empty; Show Archived is removed.
- The installed 0.162.0 schema uses `threadSection/list`, `threadSection/create`,
  `thread/section/move`, and paginated `thread/list` with `sectionId` and
  `sortKey: section_position`. No `isPinned` protocol is invented. Pinned active
  queries use `sortDirection: asc`, `archived: false` and
  `useStateDbOnly: true`; Archived metadata
  uses `archived: true` and is loaded on expansion. No rollout/history scan or
  new polling is added. The shared section is created only on an explicit Pin.
  The ascending section order is confirmed by the
  [0.162.0 thread-list implementation](https://github.com/openai/codex/blob/rust-v0.162.0/codex-rs/app-server/src/request_processors/thread_processor.rs):
  `SectionPosition` defaults to ascending, while timestamp keys default to
  descending. The [shared-pinning change](https://github.com/openai/codex/pull/51500)
  uses this same section order. This source check is not an end-to-end live test.
- Archived selection never resumes a writer. Its row menu is restricted to
  Unarchive/Delete; deletion keeps the existing explicit confirmation. Search
  still includes archives and does not change the ordinary project membership.
- New preparation no longer uses a folder-to-project creation path. Global New
  and special-group New prepare an unassigned thread; New in a real project
  retains that project ID. The chosen actual cwd is preserved, and thread/start
  remains deferred until the first prompt. Project creation remains explicit in
  Assign to project / New or in the accepted manual assignment preview.
- Documents comparison uses the normalized Windows-known folder and all its
  descendants with a directory-boundary match, not a literal folder name.
  Documents presentation takes precedence over a stale project ID without
  replacing the actual cwd or clearing that ID. Both assignment modes exclude
  these paths. Empty-project automatic assignment remains once per frontend:
  only explicit expansion of a real project requests the check, and only its
  server response with zero active threads triggers it,
  not a count reduced by agent, Documents or other presentation filtering.
  It reuses existing unique roots and never creates projects. No automatic
  project deletion or metadata repair is added.
- Tray JSON ownership follows produce-then-swap: a failed snapshot producer
  leaves the previous valid snapshot intact. The tray is published after
  dependencies/snapshots are ready, and closing-state callbacks do not route
  new UI work. This addresses a code-level lifetime risk, not a reproduced AV.
- Resident JSON transport already clones whole messages. `partial_answer`
  completes a text block, not a turn; only the terminal turn notification
  changes the work to finished. Consolidated `aggregatedOutput` remains intact,
  without reviving obsolete per-item stdout/stderr consumers or adding noisy
  command output to Activity. Extra server metadata remains opaque.
- The isolated HostReattachCheck fixture now exercises partial answer, command,
  commentary and final-answer items before the terminal turn. It checks retained
  phases/text, multiline command output, continued Working state and terminal
  cleanup. Existing settings/profile, host-only maintenance stream, native
  writer, early OAuth and new-thread token-baseline assertions also pass.
  No real user thread is modified by this check.

Normal bounded resume and Full Load retain their existing contract and limits.
This work does not add historical tool payloads, estimate tool token costs,
change conversation counters or assume new model-step RPCs.

## Completed 1.7 work

Historical baseline/minimum: Codex CLI 0.157.1. Stable release: 3 October 2026.
Release build environment: CLI 0.160.0; native queue minimum: 0.159.0.
The following historical notes do not describe missing features in current 2.3.

- Server-owned prompt queue, next-item preview and an on-demand editing panel;
  no local queue scheduler or invented pause protocol.
- Agent follow-up/file-citation markup, question links and case-insensitive
  title search including Unassigned.
- Manual assignment preview for the complete candidate list, with Windows-known
  Documents/system exclusions and shared-path project reuse.
- Automatic optional LaTeX preview insertion with stable native RichEdit offsets,
  cached bitmap reuse and original-resolution clipboard copying in the viewer.
- Fork requests retain project identity independently of cwd. Selective thread
  settings preserve unrelated confirmed sandbox/permission data; rejection
  restores confirmed UI values instead of resending rejected settings.
- Native Computer Use with CLI 0.160.0 requires the ChatGPT desktop native host
  in the tested setup. TMI 1.7 cannot bootstrap that host; standalone
  `sky.list_windows()` reports a missing native pipe (OS error 2). An independent
  Windows backend belongs to the TMI 2.0 plan and is not included in 1.7.

The sections below retain earlier implementation and verification history.

Earlier rebranding build: 1.6.9767.30081. Technical stem TMILitePlus; repository
SbiriJJ/tmi-lite-plus. Runtime title/client metadata use uApplicationIdentity.
Version components G.T remain compiler-generated. Old v1.6/rolling assets are
preserved; that renamed package used the separate v1.6-tmi release.

## Server-backed conversations and search

Rolling 1.6.9767.41769 adds a once-per-process empty-project trigger for active-only
assignment. The server query filters projectId=null; the existing assignment
pipeline is reused. Active-only protocol fixture and Release/Win64 compile pass.

Rolling 1.6.9767.40409 adds explicit bulk assignment in TThreadCatalog and the
browser context menu. It collects candidates across active/archived pages before
writing metadata, skips agents and assigned threads, and reports server failures.
CatalogStartupCheck covers these cases; ThreadUiCheck and Release/Win64 build pass.

- Startup fix: server project metadata is initialized independently of a local
  directory. This removes the erroneous empty-directory constructor call that
  stopped project loading. A protocol-fixture regression test covers catalog
  completion, rootless projects and read-only catalog/repair requests.

- Project identity, name, roots and order come from app-server. Ordinary thread
  browsing uses the state index, not a rollout scan. Unassigned threads stay
  unassigned until explicitly assigned. No second local project database.
- The Threads overlay now uses Raize GroupBar: distinctive collapsible project
  headers and thread items, persistent Show Threads/New/Sessions/Search
  commands and row-targeted context actions. The last-used project expands after its
  list is ready and visible, with no automatic thread resume. Expansion loads
  metadata only. Full Load is unchanged.
- Active thread name, project and effective working directory are displayed
  separately. Server-backed working-directory changes do not move files and
  have been verified across app-server restart in an isolated fixture.
- Project deletion was verified to preserve and unassign member threads.
  Thread deletion is permanent, including spawned descendants. Both show
  explicit target details and default to Cancel. No real data was deleted in tests.
- Project assignment has a designer-owned selection dialog keyed by IDs, not
  names. Browser rows also retain stable IDs during paginated list updates.
- New and Duplicate use real server project IDs and an explicit working
  directory. Duplicate retains its first-prompt requirement. Unassigned and
  Documents remain available; Assign changes membership without moving files.
- Dedicated Search appends paginated title matches before content results;
  title filtering is case-insensitive on DB-only metadata, including Unassigned.
  Content candidates receive one anchored excerpt per thread, disclosed in status.
  Stop/new-query generations consume late responses without inserting stale rows.
  No history is prefetched. Clicking an excerpt fetches one inclusive-cursor turn
  with full items, retaining partial assistant answers, in a separate modeless
  historical view. Exact UTF-16 snippet offsets are preserved; ambiguous/missing
  message context is explicit. Full Load has independent state and requests.
- Agent discovery reads only the current owner's spawned descendants from DB
  metadata after resume/New or concrete agent events. Rows survive metadata
  refresh; duplicate IDs are merged, old-owner replies ignored and concurrent
  refreshes coalesced without polling. Runtime IDs appear immediately, with
  nickname/role filled later. Live Subagents preserves its turn lifecycle and
  existing tool/model/message details when names change.
- Server occurrences cover user and partial/final assistant messages. Full Load retains its
  previous message-summary retrieval, local text search, memory bound and partial stop.

## Item timing and runtime tools

- Activity correlates tool start/completion by thread, turn and item identity.
  Optional CLI 0.166 `startedAtMs`/`completedAtMs` values supply item lifecycle
  timestamps in live Activity and loaded history. Prefer native `durationMs`
  when present; otherwise use complete start/end timing. Live Activity retains
  observed event times when native timestamps are absent; missing historical
  fields do not produce invented times or durations.
  Hints show local start/end times. No new timer or estimated per-tool token cost.
- Full Load keeps thread/turns/list with summary items, existing turn timestamps
  and optional per-item timing. The tested thread/items/list replacement repeatedly timed out
  on an older page; it has been removed, not retained as an automatic fallback.
  The optional timing fields do not require that replacement or broader history
  reads. Normal bounded resume is unchanged.
- MCP opens a modeless runtime inventory: MCP servers, installed connectors and
  background terminals. Details are loaded on demand, pagination is followed,
  discovery errors stay visible, and cached tools are not described as connected.
  Existing permanent-approval revocation remains behind Permanent approvals.
- Browser integration can be supplied by a plugin such as cua_repl rather than
  a server named Chrome. Only inventory actually exposed to app-server is shown.
- Client-side prompt automation and generic plugin UI hosting remain deferred.
  Version 1.7 uses the server-owned queue, not a client scheduler.

The Release executable is built in update and packaged separately as stable 1.7.
Existing published ZIPs and the current Rolling release are unchanged.

## Final 1.6 interface refinements

- Project roots merges associated active/archived thread directories, prevents
  removing used roots, and assigns a selected root only to the current thread.
- Assign to project supports New without changing the working directory.
- Config exposes the history-cache toggle; defaults remain enabled.
- The embedded title-search edit and its obsolete scope-search state are removed.
  The persistent magnifier opens the designer-owned Search form. The Show Threads
  crossed-out toggle reflects every browser visibility change. Conversation and
  Activity titles are bold; the fixed-width Activity title shares resident work
  colors with the browser and leaves the rest of the header for live activity.
- Windows-localized dates, longer path hints, and a 60-second Usage tooltip.
- No new timers for initial browser expansion. Cosmetic focused-button styling
  remains subject to Raize's rendering.

## Usage statistics window

**Usage**, beside Refresh, requests `account/usage/read` and opens a designer-owned,
non-modal window at the same width as the main window. It shows the account
lifetime-token total, peak daily tokens, longest turn and activity streaks,
followed by the first/last source dates and the number of returned daily entries.
All daily entries are retained in memory; there is no 30-entry truncation.

- **30 days** (default) and **7 days** always draw the full number of daily slots,
  using the available horizontal space. Each arrow moves by one day.
- **Extended** uses fixed-size squares in Monday-to-Sunday columns. The number
  of complete weeks depends on the width. The color scale uses the largest
  daily value in the complete received data. Weekly totals appear below each
  column. Arrows move by one week, retaining the selected weekday.
- **Weekly** shows seven groups of overlaid maximum/average/minimum bars, not
  stacked sums. Statistics cover the selected start through the last source
  date. Every calendar day in that source interval counts, with gaps counted as
  zero. Days outside the source interval are excluded. Arrows move to the next
  or previous Monday, with the first available source day as the lower bound.
- The date picker accepts any date, including dates outside the source range.
  Only backward navigation is stopped at the first source day. Changing mode
  selects the most recent window, or the first source day for Weekly.
- Labels use millions of tokens; hover for exact daily/weekly totals or weekday
  minimum, mean, maximum and sample count. Cells outside the source/selection
  are crossed out, rather than being presented as zero. Today may be partial.

These are account analytics, not the remaining allowance or the current
conversation total. Existing counters, limits, percentages, polling and token
summaries are unchanged. No new polling is introduced. The account response
does not supply all sections available in the terminal's full-screen `/usage`
dashboard (for example, a complete plugin/skill breakdown).

Snapshots are not sent to the model, saved to threads, or written into the
conversation cache. The window keeps its independent snapshot until closed;
changing dates/modes makes no server requests. Closing it does not rebuild the
conversation. Press Usage again to obtain a new snapshot.

## Thread usage comparison

**Thread usage**, beside Usage, requests the same endpoint with the current
`threadId`. For evaluation it prints the complete, indented JSON response,
including nulls, groups and additional fields, together with the requested ID.
No returned entries are omitted. This diagnostic report remains temporary in
the conversation, waits for a streaming message to finish, and does not modify
existing counters. A reply for a conversation no longer selected is discarded.

## Local display features

- Supported TeX expressions are converted to Unicode after a message block
  completes. Original text remains the source; resize reuses the display text.
- Code, Markdown link targets and plain URLs/paths are excluded. Unsupported
  expressions retain their original syntax. User prompts are not converted.
- Full Load uses the same Unicode conversion for assistant text, without
  graphic-view links. Original server history is not rewritten.
- Click supported image links for an on-demand, non-modal image viewer.
  No image previews are generated while loading the conversation.
- Graphical formulas are available only when a compatible external MicroTeX
  renderer/resource tree is installed. Version 1.7 inserts previews automatically
  for completed and rolling-loaded messages; the viewer reuses cached bitmaps
  and provides Copy image. Full Load stays text-only.
  See [MicroTeX setup](MICROTEX_SETUP.md).
  No third-party renderer or fonts are bundled with TMI Lite+.
- Reloaded image attachments now have compact textual markers, including
  `fileId` references when the server supplies no directly viewable URL/path.
