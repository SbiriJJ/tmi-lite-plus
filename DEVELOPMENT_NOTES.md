# TMI Lite+ for AI agents — 1.6 development changes

Protocol baseline and minimum: Codex CLI 0.157.1. Stable release: 28 September 2026.
Local release build environment: Codex CLI 0.158.0.

Rebranding build: 1.6.9767.30081. Technical stem TMILitePlus; repository
SbiriJJ/tmi-lite-plus. Runtime title/client metadata use uApplicationIdentity.
Version components G.T remain compiler-generated. Old v1.6/rolling assets are
preserved; the new package uses the separate v1.6-tmi release.

## Server-backed conversations and search

- Startup fix: server project metadata is initialized independently of a local
  directory. This removes the erroneous empty-directory constructor call that
  stopped project loading. A protocol-fixture regression test covers catalog
  completion, rootless projects and read-only catalog/repair requests.

- Project identity, name, roots and order come from app-server. Ordinary thread
  browsing uses the state index, not a rollout scan. Unassigned threads stay
  unassigned until explicitly assigned. No second local project database.
- The Threads overlay now uses Raize GroupBar: distinctive collapsible project
  headers and thread items, global title search, clear search, archived-only
  mode and row-targeted context commands. The last-used project expands after its
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
- Content search has its own modeless window, explicit paging and Stop,
  matching-message excerpts and read-only turn previews. Opening uses the same
  instance lock/unsubscribe/resume path as the browser. No per-thread history
  download is used to implement global search.
- Server search covers user/final assistant messages. Full Load retains its
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
- Prompt queue automation and generic plugin UI hosting remain deferred.

The Release executable is built in update and packaged separately as stable 1.6.
Existing published ZIPs and the current Rolling release are unchanged.

## Final 1.6 interface refinements

- Project roots merges associated active/archived thread directories, prevents
  removing used roots, and assigns a selected root only to the current thread.
- Assign to project supports New without changing the working directory.
- Config exposes the history-cache toggle; defaults remain enabled.
- The search edit uses the Raize button and 20-pixel glyphs. Enter/magnifier share
  the search operation; Esc/clear share clearing. Clear restores the magnifier.
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
  renderer/resource tree is installed. See [MicroTeX setup](MICROTEX_SETUP.md).
  No third-party renderer or fonts are bundled with TMI Lite+.
- Reloaded image attachments now have compact textual markers, including
  `fileId` references when the server supplies no directly viewable URL/path.
