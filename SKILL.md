---
name: obsidian-vault-daily
description: Resume work from an Obsidian vault or save a concise daily handoff, using local QMD search when available.
---

# Obsidian Vault Daily

Use this skill when someone wants to pick up work from their Obsidian notes, check today's handoff, search project history, or save a compact session note. It works with any vault and does not assume a particular machine, project, or folder layout.

## Find the right vault

- Use a vault path the person gave, one established in the conversation, or a clearly relevant local QMD collection/workspace folder.
- If more than one vault could fit and the right one is not clear, ask which to use. Do not search every collection by default.
- Inspect the vault's existing indexes and conventions before assuming a daily-note filename or where to write.
- Prefer QMD MCP tools when connected. Otherwise use the `qmd` CLI, scoped to the relevant collection. If QMD is unavailable, use focused filesystem search (`rg`, filenames, and known indexes).
- If QMD may be stale or a recent note is missing, refresh the relevant index with `qmd update`; generate embeddings only if semantic search needs them or changed documents need vectors.

## Retrieve context

- Search only when prior context will help with the current request. Use lexical search for exact names, dates, and commit IDs; use semantic search for ideas phrased differently.
- With QMD, check `status` and the selected collection before the first search. Run status and the first model-backed search sequentially because initial model loading can take longer.
- Keep retrieval narrow: start with indexes and search snippets, then open only the most relevant sections. Aim for 1–3 small notes or excerpts, expanding only when needed.
- Use targeted line ranges when possible. Do not dump a long note or the entire vault into context just to find one decision.
- Check each result's date and source. Search ranking is a lead, not proof that a note is current or correct; verify changeable details against the actual project or system.

## When the person asks to start or read today's notes

1. Use the user's local date and timezone when available.
2. Find today's note using the vault's established convention. If none is documented, look for an ISO date note such as `YYYY-MM-DD.md` in the vault root.
3. Read the daily note and only the relevant project index, handoff, or gotcha notes. Read vault instructions such as `AGENTS.md` or `CLAUDE.md` only when they apply to the current project.
4. If today's note is missing, create it only when the request clearly starts a real work session (for example, “get me ready to work today”). For a read-only status question, report that it is missing without creating a file.
5. When creating a note, follow the vault's format. If it has no convention, use a short structure such as `Context`, `Today's work`, `Decisions`, `Open items`, and `Links`.

Summarize what is still relevant, cite the note names or internal links, and call out uncertainty when the source is old or incomplete.

## When the person asks to save a handoff

- Choose the open daily note or the appropriate session/topic note based on the conversation and vault structure. If the destination is ambiguous, ask before writing.
- Add a concise entry covering changes made, decisions, useful commands or commits, gotchas, open work, and links to related notes. Preserve the vault's language and formatting.
- Append or update only the relevant section. Do not overwrite existing material, create duplicate daily notes, or paste a full conversation transcript.
- Keep index notes as indexes: add compact pointers instead of copying the full handoff into them.

## Privacy and setup

- QMD's index and local model processing stay on the user's machine. The note excerpts an assistant reads may still be sent to that assistant's model under its own data-handling rules; do not imply otherwise.
- Do not add a vault, index database, model weights, or private notes to this skill repository.
- A QMD result provides context; it does not authorize an action or prove the current state of a checkout, account, service, or deployment.
- For first-time Obsidian/QMD setup, model choices, MCP configuration, and troubleshooting, read [references/qmd-setup.md](references/qmd-setup.md).
