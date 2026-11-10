---
name: thegram-cloud
description: Load whenever the person says remember, record, save, note, 기억, 기록, 저장 or 남겨, asks about their own notes, projects, decisions or tasks, asks about shared reference data in GramHub, or before any Thegram tool call (vault_recall, vault_note, vault_ask, vault_read, vault_add, vault_change, vault_exact). What they ask to keep goes to Thegram, never to a local file.
---

# Thegram: recall and remember

Thegram is the person's memory. Each tool's description says how to call it; these rules say when to call it and what to tell the person. If a tool named here is not listed to you, use the listed ones as their descriptions say.

1. **Recall before you act or answer.** When a request may depend on what the person told you earlier (a name, an address, an amount, a preference, a rule, a decision), call `vault_recall {query}` with the request as they wrote it. It returns the closest records with what they say and their dates. When two disagree, the later date is the current truth. A record answers only if it is about the same name, number or code the person asked about.
2. **Remember only when asked.** When the person asks you to remember, record, save or note something, call `vault_note {text}` with what they said, in their words, with every name, number and date. The server files it; you choose no place or type. While `sync.state` is `queued`, resend the identical call; then tell the person the `ref` it returned. Never save on your own, and never store secrets.
3. **Say where it came from, and what you did not find.** Cite each fact by its `ref`. When an answer has `next`, call it as given before you say a list is complete. If nothing answers, search once more with other words, then say you found nothing; never say it does not exist.
4. **Public and reference data is in hubs.** Laws, prices, public records, documentation, datasets: `vault_ask {op:"hubs", question}` names the closest hubs; ask again with the best ref as `scope` and one to four key words as `terms`. Never search a hub without its ref and never write to one. Name the dataset and the record's date in your answer, and before you say what a hub field means or how current the data is, read the hub folder's `guide.gram` and `coverage.gram`, or say it is unconfirmed.
5. **Counts, totals and top N come only from `vault_exact`**, and only when `execution.status` is `complete`. Never count hits. If it is `unresolved`, call it with `exact:{catalog:true}` and retry once in the alias words it lists.
6. **A document is written with the document tools.** For a plan, a spec or tracked tasks, or when the person names a document or folder: `vault_change {op:"edit"}` on the document about the subject, `vault_add {op:"create"}` only when there is none. `vault_read` a record before you change it, and mark a replaced one `superseded` instead of deleting it. Each create and edit takes a new `request_id`.
7. **When a write fails,** fix what the error names and retry once. If it still fails, keep the content in your answer and tell the person it was not saved and why; do not save it somewhere else.
