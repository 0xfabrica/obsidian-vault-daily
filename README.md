# Obsidian Vault Daily

**A small, privacy-conscious handoff skill for Obsidian.** It helps an AI assistant find the few notes that matter, resume a project, and save a useful daily handoff without loading or copying an entire vault.

Works with any vault. QMD is optional: it adds local keyword and semantic search, while focused file search remains available without it.

## Install the skill

The [Skills CLI](https://www.skills.sh/docs/cli) installs this repository into supported agents. It runs through `npx`, so no global CLI install is required:

```sh
npx skills add 0xfabrica/obsidian-vault-daily
```

The CLI detects available agents and asks where to install. To target Claude Code and Codex globally in one command:

```sh
npx skills add 0xfabrica/obsidian-vault-daily --skill obsidian-vault-daily --agent claude-code codex --global --yes
```

To link it to every supported agent, use `--agent '*'` instead. The same Agent Skill also works with other compatible clients; see the [supported agents](https://www.skills.sh/).

The skill is installable directly from GitHub. skills.sh adds public skills to its directory from install events, so its searchable listing may appear after someone installs it with the CLI and leaves anonymous telemetry enabled. No separate submission or plugin package is required.

## Get started

1. Install [Obsidian](https://obsidian.md/download) and open or create a vault on your computer.
2. (Optional) Install QMD and register that vault as a local collection. Follow [QMD setup](references/qmd-setup.md).
3. Ask your assistant to **“read today's Obsidian handoff”**, **“find the decisions about my project”**, or **“save a concise handoff to my vault.”**

If you have more than one vault, tell the assistant which one to use. The skill does not assume a personal path or share your notes with this repository.

## What it does

- Finds the right daily note and relevant project handoffs.
- Uses exact search for names and dates, semantic search for concepts, and opens only relevant excerpts.
- Saves a concise note with changes, decisions, gotchas, commands, links, and open work.
- Preserves existing vault conventions and avoids duplicate notes or destructive rewrites.
- Falls back to focused file search if QMD is not installed or connected.

## Local search and privacy

QMD stores its index and model cache locally, and its default models are downloaded on demand. The index and weights are not part of this repository. When an assistant reads note excerpts, those excerpts may be sent to the assistant's model under that provider's data-handling rules. Review those settings before using private or work notes.

## Files

- `SKILL.md` — instructions loaded by the assistant.
- `references/qmd-setup.md` — Obsidian, QMD, models, MCP, and troubleshooting.
- `agents/openai.yaml` — display name and invocation prompt for compatible skill UIs.
