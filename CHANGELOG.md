# Changelog

## 4.8.2 — 2026-11-10

- Skill: four sentences of the rules changed, together with the server's instructions (the same SHA-256 on both sides, `44fef823…2fbc04`), so that the rules ask a tool only for what its job needs and speak only of Thegram's tools. A host's connector check had reported the wording of 4.8.0 and 4.8.1 as asking for information beyond the stated operation. `vault_recall` is called for a question about what the person said, decided or prefers, and before the person is asked for a detail that a request leaves out, with the few words that name it; it was called for anything the assistant was about to do or answer, with the request as written. `vault_note` takes what the person asked to save, in their words. The first and last paragraphs name Thegram's tools only.
- Needs the server release of 2026-11-10 (thegram-one/thegram-vault #421), which is deployed. The tool list is unchanged.
- Checked with one step per request, the instructions and the tool list: default model 72 of 72; small model 54 of 54 with the skill and 212 of 216 with the instructions alone (108 of 108 with the 4.8.1 wording). Whole requests on the deployed server: a decision recalled and cited in one call by both models; an order with nothing remembered, small model 12 of 12 recalled, ordered nothing and asked. The host's own check was not rerun; a stand-in built from the published connector review rules reports over-collection for the 4.8.1 wording in 14 of 30 runs and for this one in 0 of 14.
- No change to the `thegram-cloud` identity, the MCP URL, OAuth configuration, assets or listing metadata.

## 4.8.1 — 2026-11-10

- Skill: two rules changed, together with the server's instructions (the same SHA-256 on both sides, `e23a7f6d…8c83a69`). A count that comes back `unresolved` lists the words it did not read in `unread`: the skill asks again without them only when they add no condition, and otherwise says that condition cannot be counted. It used to send the agent to the catalog, which never fitted an answer. For a shared hub the skill gives the key words as `terms` in the language the hub is written in. The skill comes from the shared-skill release `thegram-cloud-4.8.1.zip` (`skill-source.lock.json`).
- Needs the server release of 2026-11-10 (thegram-one/thegram-vault #419, #420), which is deployed: a hub finds records by the caller's key words, `vault_exact` lists every unread word at once, and its catalog fits one answer.
- Checked on the deployed server with whole requests and real tool results: counts in the person's own vaults, small model 9 of 9 (2 of 4 before) and default model 6 of 6; two questions about Korean statutes asked in English, small model 6 of 6 (0 of 2 before) and default model 4 of 4. A count that names a hub's records by a word the server does not read ("vulnerabilities") still stops the small model in about two runs of three: it gives no number rather than a wrong one. No Codex or ChatGPT host has run it.
- No change to the `thegram-cloud` identity, the MCP URL, OAuth configuration, assets or listing metadata.

## 4.8.0 — 2026-11-10

- Skill: rewritten as one page of 3.8 KB; it was eighteen rules in 6.5 KB. Its rules are the text the Thegram server sends as its instructions, byte for byte (`thegram-skill` and `thegram-vault` pin the same SHA-256, `7e20df79…08df6`), so an assistant given the skill, the instructions or both reads one contract. The rules route a request to one tool, first match wins: remember with `vault_note` only when asked, in any language; a count with `vault_exact`; public data through `vault_ask {op:"hubs"}`; what can be read or what changed with `vault_ask`; a document with `vault_change` and `vault_add`; and anything else that may depend on what the person said with `vault_recall`, before asking the person for the missing detail. The skill no longer lists the record types, tells the agent to find a document before a note, or sends it to the guide. The skill comes from the shared-skill release `thegram-cloud-4.8.0.zip` (`skill-source.lock.json`).
- Checked with a model given exactly the server's instructions and tool list (`thegram-skill`, `evals/scenarios.json`, eighteen one-step scenarios): the small model chose the expected call in 107 of 108 runs and the default model in all 72, with the skill loaded or not. No real host has run it.
- Publish after the server release that lists `vault_recall` and `vault_note`, drops the `guide` and `feedback` ops and sends these rules (thegram-one/thegram-vault #416, #417 and the unified-rules change). Before it the two tools are not listed, and the skill says to use the listed tools as their descriptions say.
- Review materials (`plugin.json`, `review/`) no longer expect `vault_ask {op:"guide"}` and accept `vault_recall` where a scenario recalls.
- No change to the `thegram-cloud` identity, the MCP URL, OAuth configuration, assets or listing metadata.

## 4.7.5 — 2026-11-09

- Skill: a move, delete or restore takes no `request_id` and refuses one; only a create and an edit carry it. The skill said every write has one, which a delete refused; after a lost reply it now reads the result before repeating a move or delete. The skill also comes from the new shared-skill release `thegram-cloud-4.7.5.zip` (`skill-source.lock.json`).
- First public release since 4.7.2, so it also carries 4.7.3 and 4.7.4: no `hooks/`, no `tests/e2e/` or API-key commands in the README, and the Claude and API-key plugins moved to their own repositories (`thegram-one/thegram-plugin`, `thegram-one/thegram-plugin-api`).
- No change to the `thegram-cloud` identity, the MCP URL, OAuth configuration, assets or listing metadata. The server side of this fix is in thegram-one/thegram-vault #397.

## 4.7.4 — 2026-11-07

- Split the plugin into three repositories, one per host: Claude stays in `thegram-one/thegram-plugin`, this repository carries Codex and ChatGPT, and `thegram-one/thegram-plugin-api` carries the API-key plugin. The Claude directory refused 4.7.3 for the packaging scripts and tests at the repository root (`LOCAL_CODE_FILE_TYPE`), so the Claude repository now holds only the Claude plugin files.
- Remove `.claude-plugin/`, `.mcp.json` and `release/claude-README.md` from this repository, and drop `build-claude` and the Claude compatibility checks from `scripts/package_plugin.py`. The portable ZIP now holds `plugin.json`, `mcp.json`, `skills/`, `assets/`, `LICENSE` and `NOTICE` only; the release publishes `thegram-cloud.zip` and no Claude ZIP.
- The Codex marketplace moves with the plugin: run `codex plugin marketplace remove thegram-cloud` and `codex plugin marketplace add thegram-one/thegram-plugin-codex`. The plugin identity, the MCP URL and the OAuth login do not change.
- No change to the bundled skill, the `thegram-cloud` identity, the MCP configuration or the listing metadata apart from the repository and support links.

## 4.7.3 — 2026-11-06

- Remove what the directory policy hold `MCP_FORWARDS_CREDENTIAL_ENV` ("Uses a credential from the user's machine", on 4.6.0 through 4.7.2) pointed at: `tests/e2e/` (the archived live harnesses read OAuth and bearer tokens from the environment and a local refresh-token store, and carried a local Chrome path and test mailbox) and the `THEGRAM_API_KEY` bearer-header commands in the README. API-key sign-in is documented at https://docs.thegram.one/guides/local-setup/; the plugin itself still signs in with OAuth only.
- Remove `hooks/hooks.json`. The directory will accept `command` hooks only from Verified organizations. The citation and completeness rules it echoed after each read stay in the bundled skill and the server instructions; only the per-read reminder is gone.
- No change to the bundled skill, the `thegram-cloud` identity, the MCP URL, OAuth configuration, assets or listing metadata.

## 4.7.2 — 2026-11-05

- Fix the portable ZIP for the OpenAI plugin portal: it no longer contains `hooks/hooks.json`, and the packager now rejects any `hooks/` file in a public archive. The portal refuses plugins that contain hooks ("Plugins containing hooks cannot be submitted"), which blocked the 4.7.1 portable ZIP.
- Claude Code installs from this repository still load `hooks/hooks.json` (the citation and completeness reminder after each Thegram read). The Claude ZIP never carried hooks.
- No change to the bundled skill, the `thegram-cloud` identity, the MCP URL, OAuth configuration, assets or listing metadata.

## 4.7.1 — 2026-11-05

- Remove the forced recording rules from the bundled skill: it no longer requires the agent to offer "Save this to Thegram?" for decisions, tasks, commitments and outcomes the person states, to avoid silent saves, or to report what it left unsaved. Whether to suggest or make a save the person did not ask for is the agent's judgment.
- Keep the rest of recording unchanged: a request to record goes to the named Thegram vault, the agent writes what is known and names what is missing, follows the write procedure, and keeps the content in its answer when a write cannot land. Every read rule is unchanged.
- Keep `thegram-cloud`, the remote MCP URL, OAuth configuration, assets and listing metadata unchanged. The server instructions drop the same rules; no tool, scope or credential changes.

## 4.7.0 — 2026-11-04

- Replace the bundled skill with the 4.7.0 release from the shared skill source (6 KB, was 15.4 KB) for the five tools the server now lists: `vault_ask` (questions; `op` `hubs`, `browse`, `changes`, `comments`, `guide`), `vault_read`, `vault_add` (`op` `create`, `comment`, `restore`, `feedback`), `vault_change` (`op` `edit`, `move`, `delete`) and `vault_exact`. It states which surface it serves and defers to the server instructions on any other.
- Recording is the point: the agent records what the person asks to save, offers to record decisions, tasks, commitments and outcomes they state, never saves silently, and when a write cannot land keeps the content in its answer and says where it was not saved. Record types fall back to the closest type instead of a refusal.
- A request to record into a named vault or folder ("Thegram-dev docs") goes to that Thegram vault, never a local file; the skill description lists the record words (record, save, 기록, 저장, 남겨) so hosts load it for them. The agent reads the subject's own sources (a repository's README, design notes, manifest) and records what is known, naming what is missing instead of refusing.
- Reads decide the target first: the person's own work goes to `vault_ask` without scope; public or reference data goes through `vault_ask {op:"hubs", question}`, which ranks any number of hubs against the question (an unscoped answer also lists `scope.hub_candidates`). No hub is named in the skill. Hubs are never searched unscoped or mixed with own vaults, tags are copied from hits, `guide.gram`/`coverage.gram` explain a dataset, and a hit counts only when it matches every entity the question names.
- Filters go only in quoted `plan.conditions`; `terms` and `options` are for a find only. Counts come only from a complete `vault_exact`; an unresolved one is retried once in the catalog's alias words. A complete result is exact only within its scope, unknown upstream coverage is reported, and exact snapshots are read within their ~15-minute life. A read that cannot meet a rule still answers and names what is partial.
- Add a Claude Code hook (`hooks/hooks.json`) that follows every read answer with the citation and completeness rules. Other hosts rely on the skill and the server instructions.
- Keep `thegram-cloud`, the remote MCP URL, OAuth configuration, assets and listing metadata unchanged. Needs vaultd with the five-tool surface (thegram-one/thegram-vault#341) and the Gateway retrying `vault_add`/`vault_change` writes (thegram-one/thegram-mcp-gateway#5); rescan the MCP server on the plugin page.

## 4.6.0 — 2026-11-04

- License the plugin under Apache-2.0 (`LICENSE`) with a `NOTICE` that reserves the Thegram name and logo. Both files ship in the portable and Claude ZIPs, and `license` in `plugin.json` and `.claude-plugin/plugin.json` changes from `UNLICENSED` to `Apache-2.0`.
- Rewrite the listing around the current thegram.one concept, the AI knowledge base for enterprise AX: new descriptions, keywords, capabilities, default prompts and Korean translation. Add `documentationUrl` (https://docs.thegram.one) and point the portable manifest at `thegram-one/thegram-plugin`.
- Replace the bundled skill with the 4.6.0 release from the shared skill source. It covers the 14-tool surface including `vault_exact`, adds a table of the 22 record types (what each holds and which neighbouring type it is not) that drives the filter when reading and the type when writing, maps common questions to `type`, `status`, date and `vault_exact` calls, asks for `vault_guide` before the first write, and carries the coverage and validity cautions: candidates and groups never prove a count, and only a complete `vault_exact` execution does.
- Add the Codex marketplace `.agents/plugins/marketplace.json` (`codex plugin marketplace add thegram-one/thegram-plugin`, then `codex plugin add thegram-cloud@thegram-cloud`) and the README steps for installing on Codex and for replacing OAuth with an API key.

## 4.5.0 — 2026-10-31

- Replace the bundled skill with one short `SKILL.md` and drop the ten references. One method reads every vault and shared GramHub dataset: browse once for scope, search inside a folder, copy tags from a hit, narrow with `tag`, dates, `terms`, `group_by` and `order` (`-at` newest first, `at` oldest first), read the metadata of every answer, and open records before quoting them. The skill holds no recipe for a particular dataset; a dataset that does not fit the method is fixed in how it is stored.
- Record shapes, templates and write rules come from `vault_guide` and the tool schemas. The skill keeps only what a first write gets wrong: the record types, `x` values, id and link rules, the `request_id` and `sync.state` loop, the `edit-text` append for a new section, and the error table.
- Coding agents use the same plugin and endpoint with an API key bearer header (`claude mcp add --transport http … --header`, `codex mcp add … --bearer-token-env-var`, `opencode mcp add … --header`). The separate `thegram-memory` plugin is retired.
- Keep `thegram-cloud`, the remote MCP URL, OAuth configuration, assets and listing metadata unchanged; only versions, release notes and the skill change. Needs vaultd with `order` and title and tag ranking (thegram-one/thegram-vault#280) and the GramHub kind tags; rescan the MCP server on the plugin page.

## 4.4.3 — 2026-10-30

- Move the bundled skill, references and review cases from the 18 `gram_*` tools to the 13 `vault_*` tools the server now lists: `vault_guide`, `vault_search`, `vault_read`, `vault_browse`, `vault_changes`, `vault_comments`, `vault_create`, `vault_edit`, `vault_comment`, `vault_feedback`, `vault_move`, `vault_delete`, `vault_restore`.
- Reads keep their reach: `vault_search` takes the ranked recall's filters (`type`, `status`, `from`/`to`/`date`, `scope`, `mode`, `as_of`) and returns its `receipt`, `relevance` and `LOW_RELEVANCE` guidance, and `vault_search {receipt}` re-runs an earlier search; `vault_browse` lists vaults and folders, summarizes a document, lists records by `select {type, status, ...}` and a document's entity ids; `vault_read` reads records, sections (`§heading`, `§제3조`) and documents, at a past state with `as_of`, and follows a record's links with `relations`.
- A filtered search reads one folder of at most one cell of documents and a `select` five cells; a wider scope returns `E_SCOPE_TOO_WIDE` with the folders to choose from. `vault_guide` never fails on its own arguments.
- Keep `thegram-cloud`, the remote MCP URL, OAuth configuration, assets and listing metadata unchanged; only versions, release notes and the review cases' tool names change in the manifests. Needs the vaultd release that lists the `vault_*` tools (thegram-one/thegram-vault#246, #247 and #248); rescan the MCP server on the plugin page.

## 4.4.2 — 2026-10-27

- Import the validated public 4.4.2 skill for the 18-tool cloud surface: `gram_replay {receipt}` re-runs an earlier recall against the documents as they were, and `gram_recall` first pages carry a `receipt`.
- A recall first page that stops with `LOW_RELEVANCE` holds the strong matches; the skill follows `more` past it only when a question needs every match, and still follows and reports other incomplete coverage.
- Keep `thegram-cloud`, the remote MCP URL, OAuth configuration, assets and listing metadata unchanged; only versions and release notes change in the manifests.

## 4.4.1 — 2026-10-27

- Import the validated public 4.4.1 skill: task-driven research in bounded units, one deeply explained document per subject, typed claims with a verification basis (`basis:observed`, `basis:reported`, `basis:inferred`), version-pinned sources and `supports`/`contradicts` reasoning links, re-verification of claims whose sources changed, and a logic audit before completion.
- `SKILL.md` becomes a compact router with always-on rules; references are one step away. Synthetic test fixtures (`classification.json`, `lifecycle.json`) and `authoring-patterns.md` leave the shipped skill; `backfill.md` moves under `references/`.
- Keep `thegram-cloud`, the remote MCP URL, OAuth configuration, assets and listing metadata unchanged; only versions and release notes change in the manifests.

## 4.0.2 — 2026-10-24

- Import the validated public 4.0.2 workflow with self-contained records, natural-question retrieval checks and source-read revision/hash protection for section replacements.
- Pin the accepted release archive SHA256 and exact skill file hashes instead of the obsolete `thegram-memory` source path.
- Keep `thegram-cloud`, the remote MCP URL and OAuth configuration unchanged; remove document-chat delegation from current installation guidance.
- Retain the existing Claude listing metadata and marketplace while updating their versions for the directory rescan.

## 2.4.1 — 2026-10-23

- Import the canonical `thegram-memory` 3.5.1 search-first project-binding workflow; preserve the 2.4.0 document-chat read-mode behavior.
- Keep the OAuth configuration, MCP endpoint, plugin identity and other bundled workflows unchanged.

## 2.4.0 — 2026-10-23

- Import the canonical `thegram-memory` 3.5.0 read-mode workflow. Agents answer requests sent in read mode without operations; `E_READ_ONLY` means resubmit with the reply alone.
- Keep the OAuth configuration, MCP endpoint, plugin identity and other bundled workflows unchanged.

## 2.3.0 — 2026-10-19

- Import the canonical 3.4.0 skill: an agent keeps each piece of work as one task (`owner` its agent name, `s` the repository, `start`/`done`), moves `x` with the work (`waiting` with the question, `review` with what to look at, `blocked` with what stops it, `completed` with the outcome) and records each PR, merge, release, deployment or verification as one step event linked `part-of` that task, never one per commit or test run.
- The AI work board (thegram-ui 3.32) reads this shape. The OAuth configuration and MCP endpoint are unchanged.

## 2.2.0 — 2026-10-19

- Import the canonical 3.3.0 skill: scheduled automations write one run record per execution (failed sources as `failed:<source>` tags), keep each found item once with `ref`, `at`, `source:`/`topic:` tags and a `derives` link to the run that first found it, and tag watched things `target`; what needs a person follows the project's own rule, a change still ahead becomes a dated task and harm nobody can act on stays a risk.
- The research, monitoring and collected-log dashboards (thegram-ui 3.31.0, vaultd `1a6bf40`) read this shape. The OAuth configuration and MCP endpoint are unchanged.

## 2.1.2 — 2026-10-19

- Import the canonical 3.2.2 skill: every document follows Core's one record vocabulary (`gram@1`), so `gram_create` takes no template. One status table gives each status its open, done or closed class, which the dashboard, `gram_brief`, `gram_recall {status}` and the graph all count by; `mitigated` risks and `proposed` decisions stay open.
- Closing a record: rewrite `d` in the same patch, link resolving evidence `related` rather than `supports`, reconcile the same fact in other documents first, and keep `ref` to links others can open (no `file://` or local paths). A new classification case covers a fix that resolves one risk and only mitigates another.
- Needs the vaultd release that serves the one vocabulary; the OAuth configuration and MCP endpoint are unchanged.

## 2.1.1 — 2026-10-19

- Import the canonical 3.2.1 skill: classify atomic facts by meaning; reuse tracked tasks, components and metric identities; preserve proposed decisions, incomplete work, relation scope and source dates. No automatic commit events or invented historical task lifecycles.
- Add worked classification regressions and widget data contracts for status classes, goal links, calendars, metrics, saved-layout readiness and runtime-only capabilities.
- The eight shared skill files match the tested private ChatGPT plugin 2.1.1 after the canonical frontmatter identity replacement. Package and adapter configuration are unchanged.

## 2.1.0 — 2026-10-18

- Document chat: the skill (pinned from `thegram-mcp` 3.2.0, commit `bc0e2dd`) tells ChatGPT how to subscribe to the `chat.task.created` MCP event and answer each request with `chat_task_get`, `chat_doc_read` and one `chat_task_submit`, which read and change only the document the request came from. The contract reference lists the chat tools, event and errors.
- The listing adds the document chat capability, one positive and one negative review case, and 2.1.0 release notes. The MCP E2E accepts the four chat tools beside the `gram_*` tools.
- The OAuth configuration and MCP endpoint are unchanged. The tools and event need the vaultd release from thegram-one/thegram-vault#26; rescan the MCP server on the plugin page after it is deployed.

## 2.0.9 — 2026-10-16

- The Claude manifest declares the directory listing fields: display name Thegram, the Thegram logo as its icon, and the support, privacy policy and terms URLs. This clears the directory validator's missing privacy policy warning. No MCP, OAuth, skill, server, credential or production setting is changed.

## 2.0.8 — 2026-10-16

- The OpenAI listing's AI access section, in English and Korean, now says "compatible AI assistants and coding agents" instead of naming ChatGPT and Claude. No MCP, OAuth, skill, server, credential or production setting is changed.

## 2.0.7 — 2026-10-16

- Rewrite the OpenAI and Claude listing around "Where your thoughts connect": everyday life, learning, hobbies and teamwork, not only projects. Refresh the English and Korean descriptions, capabilities, example prompts, keywords and Claude manifest descriptions.
- Describe web document editing, document/dashboard/graph views on the web, widget dashboards, semantic search with word fallback, revision history and per-agent access. Drop the outdated claim that Graph is desktop-only.
- Add the Claude Code marketplace install line to the README. No MCP, OAuth, skill, server, credential or production setting is changed.

## 2.0.6 — 2026-10-16

- Return to the single portable package used through 2.0.4: root `plugin.json` with the OpenAI listing, icons, review cases and publication notes, plus `mcp.json`, `.claude-plugin/` and `.mcp.json`. The OpenAI directory reads its listing from it again; the Claude directory is submitted from the repository's `.claude-plugin/plugin.json` root.
- Remove the `.app.json` binding; directory ZIP uploads must not include apps.
- Keep the 2.0.5 shared workflow: `skills/thegram-cloud/` is the pinned import of `thegram-mcp`'s `skills/thegram-memory/`, and builds reject drift.
- Coding agents continue to use `thegram-mcp` with scoped API keys. No server, OAuth scope, credential or production setting is changed.

## 2.0.5 — 2026-10-16 (withdrawn)

- Split the release into `thegram-chatgpt-2.0.5.zip` and `thegram-claude-2.0.5.zip`. Neither is a valid OpenAI directory upload: without the root `plugin.json` the listing metadata and icons are missing, and the ChatGPT ZIP includes `.app.json`, which directory ZIP uploads do not accept. Do not submit either.
- Imported the `thegram-mcp` 3.1 skill and references as the one workflow source with pinned file hashes.

## 2.0.4 — 2026-10-16

- A record of the wrong kind, such as a commit written as a task, is treated as a mistake rather than history: the skill reports it and, with the user's consent, keeps the fact as an `event` and removes the wrong record. Changing records to move a number stays forbidden.
- Tools, MCP/OAuth configuration, package identity and review cases are unchanged from 2.0.3.

## 2.0.3 — 2026-10-16

- Dashboard: the skill now says what the Project Tracker counts. Progress, open tasks and the task board count `task` records (cancelled/superseded stay in the total); goal rings count tasks linked `part-of` the goal; risks count by status; a metric chart needs repeated `value`/`observed` updates on one metric; revision history is recording time. Planned work is a `task` closed with `completed` + `done`; a commit, release or deploy is an `event`. Adding, splitting, relabelling or completing records to move a number is forbidden; a misleading number is reported instead.
- Body: a mention `[label](@id)` sits inside a sentence; body collections render in the document view, not on the dashboard.
- Tools, MCP/OAuth configuration, package identity and review cases are unchanged from 2.0.2.

## 2.0.2 — 2026-10-16

- Recording: when saving is authorized, the skill maps each kind of outcome to one linked record (accepted or replaced decision, shipped or verified event, caveat as risk or task, status change with `old`, metric observation) and reads the root project record before the first write. Nothing qualifying means nothing is written.
- Folders: Thegram Cloud vaults support folders (`gram_folder`, `gram_move`, `gram_list {}`), so the skill reuses a document wherever it sits, moves or creates folders only when asked, and never creates a second document because an expected path is empty. Contracts no longer claim only root vault documents are writable.
- Writes: `gram_create` accepts `request_id` and replays an identical create; Cloud `gram_patch` rejects `action`/`action_target` (`E_ARGUMENT`), so template requirements are met with ordinary operations and `E_ACTION` guidance is removed. `gram_relations` takes `from: {path, entity}`.
- Tools, MCP/OAuth configuration, package identity and review cases are unchanged from 2.0.1.

## 2.0.1 — 2026-10-14

- Thegram 2.0.1 shows one dashboard per document type: `project@1` Project Tracker, `schedule@1` Journal, `knowledge@1` Knowledge. The bundled skill, contracts and README now say so: pick one template for a new document with `gram_create {template}`; only a declared document is editable on its dashboard; a document without a template shows Core's recommended dashboard (`~project@1`) read-only, and the agent declares a template only when the user asks; an unknown template declaration has no dashboard. Wording that suggested several screens or boards per template is removed.
- Tools, MCP/OAuth configuration, package identity and review cases are unchanged from 2.0.0.

## 2.0.0 — 2026-10-14

- Start new memories from a template: schedule@1 for schedules and journals, project@1 for tasks and projects, knowledge@1 for research, knowledge and operations notes. Create with `gram_create {template}` and follow `gram_profile {topic: "templates"}` for required fields, status values and action ids.
- Write to the head by default: omit base_revision and expected_hash, give each set/unset `old` (the value read, null if empty) and use `values` for repeated fields. Reread and decide on `E_PRECONDITION`; fix `E_TEMPLATE` and `E_ACTION` by `details.diagnostics` (field, expected, example) before dropping an action.
- Add live agent-scope MCP E2E with a fresh Logto client, one-time-token cookie sign-in, CSRF-protected grant management, default-only visibility, hidden ungranted vaults, read/write enforcement, and immediate revocation with the same unexpired MCP token/session. Clean temporary vaults/documents and the Logto application; write credential-redacted JSON request/response evidence.
- Keep the ordinary MCP E2E on the default granted Thegram vault unless a pre-granted mount is specified; creating an arbitrary vault no longer implies agent write access.
- Explain per-agent scopes and Connected agents settings in README, package descriptions, reviewer expectations and bundled skill error guidance. Preserve five positive and three negative review cases and existing package identity/version/OAuth bindings.
- Record the unavailable MCP document-delete coverage explicitly: no deletion tool exists, and app API tokens do not exercise agent grants. Report the next endpoint’s production MCP audience as an isolation risk.
- Document Claude custom-connector setup using the existing MCP endpoint and CIMD OAuth, separately from bundled-skill installation and directory publication.
- Preserve safety-hint checks in the live consent E2E without pinning the entire annotations object or initialization wording.

## 1.5.4 — 2026-10-13

- Thegram Cloud recall now ranks by meaning like the desktop app: `semantic` and `hybrid` work and `auto` fuses keyword and semantic ranks. Update the skill, contracts and README, which previously said cloud recall was keyword-only.
- Explain how to recognize a keyword fallback: `gram_capabilities.semantic` unavailable, or a recall reporting `mode`/`reason` such as `SEMANTIC_INDEX_STALE` while a new document is indexed. Historical (`as_of`) recall stays keyword-only.

The server-side change was deployed separately to vault.thegram.one; this package changes no MCP/OAuth configuration, credentials, access or scopes.

## 1.5.3 — 2026-10-12

- Clarify purpose-based .gram memory for people and AI: save only what the user asks to save, with source attribution and revision history; Cloud supports Document and Dashboard, with Graph in the desktop app. MCP/OAuth configuration is unchanged.
- Update the package's save-consent guidance and public installation documentation to require explicit user authorization. This package change does not establish the deployment state of server-side onboarding instructions.
- Prepare the synthetic Atlas review fixture; keep reviewer account details and portal configuration private.
- Record five positive and three negative live cases in ChatGPT web on 2026-10-12. The runs used a dev-mode custom `gram` MCP connection without the bundled skill and do not establish bundled-skill behavior. The recordings are published at https://thegram.one/review/openai/.
- The three recordings are publicly available: [review cases](https://thegram.one/assets/review/thegram-review-cases-en.mp4), [ChatGPT walkthrough](https://thegram.one/assets/review/chatgpt-gram-demo-en.mp4), and [web/Mac montage](https://thegram.one/assets/review/thegram-chatgpt-web-mac-montage-en.mp4).

Owner maintenance: if the service requests updated terms acceptance, complete it before continuing. Before each repeat run, restore Atlas to the clean fixture and remove `northstar-website-refresh` if present; the existing authorized user account can be reused.

No server deployment or credential/access/scope change is performed by this package update. The review recordings are publicly available at the URL above.

## 1.5.2 — 2026-10-12

- Reposition the public plugin for people and AI working from shared, purpose-based .gram memory, with source-grounded history and relationships kept in one reusable file.
- Clarify the same records' document/dashboard/graph presentation and revision history in compatible apps: Cloud supports Document and Dashboard; Graph is available in the desktop app. Preserve connector-provided context only when saving is authorized, without implying direct Slack/Gmail fetching or indiscriminate capture.
- Synchronize package manifests at 1.5.2 and align the English/Korean listing, marketplace and skill description. Preserve thegram-cloud identity, C.Lab developer, Roy Kim author and existing MCP/OAuth configuration.
- Keep five positive and three negative scenarios; attribute the staging outcome to the user's report rather than independently verified evidence.

These package changes do not deploy the cloud server, change accounts/access/scopes/OAuth/credentials, or establish review submission or publication.

## 1.5.1 — 2026-10-12

- Align the OpenAI interface developer name with C.Lab and synchronize public package manifests at version 1.5.1; keep Roy Kim as package author.
- Finalize the five-positive/three-negative English review cases and proposed recording sequence; add explicit ISO 8601 UTC historical `gram_changes` guidance to the reviewer materials and bundled skill/contracts.
- Record that 1.5.3 updates package consent guidance and reviewer setup, cases, and published recording status.

No cloud server deployment, account/access/scope change, OAuth change, or credential change accompanies these metadata and documentation updates.

## 1.5.0 — 2026-10-11

- Replace broad connector capture instructions with the English durable project-memory lifecycle for dot and other cloud agents: current context, selective retrieval, authorized outcomes, readable purpose/state/reasons/evolution and unresolved work.
- Add source-grounded typed-record/relationship guidance, stable metric IDs, honest event/recording time, conflict/replay recovery, supported-view limits and fictional lifecycle payloads.
- Bundle the single thegram-cloud skill and its references through the portable release package and the existing private ChatGPT ZIP update route.
- Preserve private plugin identity, app bindings, connection settings, unknown metadata and unrelated skills during ZIP updates; validate discovery and reject unsafe archives.
- Add offline package/behavior validation and exact-commit CI. Keep cloud/local integration, storage verification, host installation and renderer verification distinct.

No cloud server deployment, access/scope change or credential change accompanied 1.5.0.
