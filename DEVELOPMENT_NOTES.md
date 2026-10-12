# TMI Lite+ for AI agents — Development Notes

Document version 2.0  
Stable release: 9 October 2026  
Tested Codex CLI: 0.162.0  
Local IPC protocol: 14

## Version 2.0 architecture

The resident TMIHost owns independent Codex app-server processes for catalog
and conversation work. Frontend detachment does not stop work; reattachment
reuses its writer. Native approvals/questions are routed to the owning GUI,
not shown or automatically answered by the host. All four matching binaries
must be installed together. Version components G.T remain compiler-generated.

Remote 2.x update routing is separate from legacy 1.x: Release reads `stable-2`,
Rolling reads `rolling-2`. No 2.x Rolling has been published yet. Generic GitHub
Latest/legacy `rolling` remain unchanged for 1.x, and 2.0 does not become generic
Latest. The direct versioned release links to the full bundle ZIP, named
`TMILitePlusBundle-2.0.G.T-Win64.zip`. The old single-EXE package recognizer does
not accept this bundle. The 2.x extractor stages all four matching EXEs and
the package's approved runtime/documentation files; personal INI is excluded.
Fixture validation of extraction/staging is distinct from a live network update.

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

Visualize initializes WebView2 only on request. Its sandboxed HTML viewer
provides an inline bitmap, zoom and clipboard copying without rebuilding the
conversation on close. WebView2 Runtime is separate; MicroTeX remains external.
Full Load, Usage, diffs, attachments, Questions and native queues remain available.

Focused tests and installed-bundle use covered reattachment, multi-frontend
operation, desktop actions and dual-monitor warnings, maintenance boundaries,
Windows shutdown, Full Load, Usage and attachments. Pinned/Archived and shared
list refresh have passed practical multi-frontend use. The one-off tray AV has
not recurred, but its original occurrence was not reproduced. This is not an
exhaustive certification of every Windows app or future CLI build.

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
The following historical notes do not describe missing features in current 2.0.

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
  Prefer server tool duration when present; otherwise label elapsed time between
  server events. Missing start/completion timestamps do not produce a duration.
  Hints show local start/end times. No new timer or estimated per-tool token cost.
- Full Load keeps thread/turns/list with summary items, existing turn timestamps
  and cache format. The tested thread/items/list replacement repeatedly timed out
  on an older page; it has been removed, not retained as an automatic fallback.
  Historical per-item timing is deferred. Normal bounded resume is unchanged.
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
