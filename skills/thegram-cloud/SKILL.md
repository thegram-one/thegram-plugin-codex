---
name: thegram-cloud
description: Load first whenever the person says record, save, note, remember, 기록, 저장 or 남겨, or names a Thegram vault or folder (the vaults listed in the Thegram server instructions, e.g. "Thegram-dev docs"); those go to Thegram, never to local files. Also for any question about the person's notes, projects, decisions or tasks, any question about shared reference data in GramHub, and before any Thegram tool call (vault_ask, vault_read, vault_add, vault_change, vault_exact).
---

# Thegram: how to read and record

For the five-tool surface only: `vault_ask` (read; `op` `ask` default, `hubs`, `browse`, `changes`, `comments`, `guide`), `vault_read`, `vault_add` (`op` `create`, `comment`, `restore`, `feedback`), `vault_change` (`op` `edit`, `move`, `delete`), `vault_exact`. If the tools listed to you have other names, this skill does not apply: follow the server's instructions.

The rules below keep reads honest and records clean; when one cannot be met, do the rest and say exactly what is missing.

## Step 1. Decide the target, every time, before any call

- **Own vaults**: the person's and their organizations' notes, projects, decisions, tasks, meetings → `vault_ask {question}` with NO `scope`.
- **Hubs** (`@mounts/hub-team-*`, read-only): public or reference datasets; there can be any number of them → `vault_ask {op:"hubs", question}` returns the closest hubs. Pick the one whose `label` and `folders` fit and ask with its ref as `scope`. None fits → say Thegram holds no such dataset.
- Unsure → ask without scope first: the answer lists `scope.hub_candidates`; use one if the question is about reference data.
- NEVER search a hub without its ref in `scope`. NEVER mix own vaults and hubs. NEVER use a ref no tool returned. Hits of the wrong kind (personal notes for a reference question, or the reverse) mean the wrong target: go back to Step 1.

## Step 2. Read

1. `question` = the person's words with every exact name, number and code. For a find, add `terms` (1–4 nouns every hit must hold). `terms` and `options` work ONLY with a find: NEVER send them with `plan.operation` `count`, `list` or `changes` (`E_ARGUMENT`).
2. Filters (type, status, date, tag) go ONLY in `plan.conditions`, each with `quote` = the question's words it came from. NEVER invent a value. On `interpretation.unresolved`, use a value from `allowed` or ask the person.
3. Hubs differ from vaults: document names are opaque ids, records carry `tags` as `key:value`, and a hub folder may hold `guide.gram` (field and date meanings) and `coverage.gram` (period covered). NEVER walk a hub's folders. Narrow by copying a tag exactly from a hit into `record.tag`. Read `guide.gram` / `coverage.gram` before stating what a hub field or date means or how current it is; if they do not exist, say the meaning or freshness is unconfirmed.
4. A hit counts only if it matches EVERY entity the question names: same name, number, article or code. Another entity with the same number is not an answer.
5. Use `records` and `hits` first; `vault_read` only what they lack.
6. When an answer has `next`, call `next.tool` with `next.args` as given, even if that tool is not listed. NEVER build a cursor. If that call comes back `unresolved` or fails, apply Step 3 rule 8 before giving up.

## Step 3. Answer

7. Every claim cites `path#id`. No ref, no claim.
8. Counts, totals, averages, maxima, top N: ONLY `vault_exact {query, folder}`. Its planner understands short phrases in the source's alias words, not long questions: if a call is `unresolved`, call `vault_exact {query, folder, exact:{catalog:true}}`, rewrite the query as `<condition words> <type alias> 개수` (e.g. `미해결 할 일 레코드 개수`, `할 일 개수`) using only listed aliases, and retry once. Accept ONLY `execution.status:"complete"`; `running` → call `next` until it ends. Still not complete → say there is no exact result and why. NEVER count hits.
9. A complete exact result is exact only within its scope: if `coverage` or the source says upstream completeness is unknown, say so. Its snapshot (`execution.read_set`) expires in about 15 minutes: read rows with `vault_read {refs, snapshot}` before then, or rerun.
10. Say plainly that a result is partial or approximate when it shows `relevance:"weak"`, `omitted`, `coverage.files_pending` > 0, unknown upstream coverage or `interpretation.exhaustive`. NEVER say "none exists" after one search.
11. Hub data is a published snapshot: name the dataset and the record's date.

## Step 4. Record

12. The destination is a Thegram vault, never a local file or repository document. A vault or folder the person names ("Thegram-dev docs") is one of the vaults in the server instructions; if none is named and it is unclear, ask which vault. The content comes from the conversation and from anything you can read: for a repository, project or file, look for it by name where you can (working directory, its parent and siblings, home directory) and read its README, design notes and manifest first. When you record, what the person said is enough (the name, that it exists, today's date); add what sources state, cite them, and name what is still unknown. Asking for more never replaces writing what you have.
13. Before the first write in a conversation call `vault_ask {op:"guide", task:"memory"}` and follow it.
14. Add to the existing document on the subject; `vault_add {op:"create"}` only when none exists. Hubs are read-only: never write there.
15. One record per fact. `type` is one of: knowledge evidence context history assumption source decision requirement goal task project blocker run risk metric event person agent system service component artifact. When unsure, pick the closest; `knowledge` fits any plain fact.
16. Before changing a record, `vault_read` it. Status: `set x` with `old` = the value read. Mark a replaced record `superseded` instead of deleting it.
17. Every create and edit has a new `request_id`; a move, delete or restore takes none and refuses one, so after a lost reply read the result (browse the path or its trash) before repeating it. On `sync.state:"queued"`, resend the identical call until `"synced"`, then `vault_read` it and report path, revision and record ids.
18. On an error, fix the cause from `details` and retry; never resend unchanged. If the write still cannot land (no write access, the vault refuses), keep the full content in your answer and tell the person where it could not be saved and why; do not save it somewhere else on your own.
