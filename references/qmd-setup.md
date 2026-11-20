# Set up Obsidian and QMD

Use this guide to prepare QMD on the computer that stores the vault. QMD's installation requirements and model defaults can change; check the linked official documentation if a command or dependency behaves differently.

## 1. Install Obsidian and create a vault

Download Obsidian from [obsidian.md/download](https://obsidian.md/download), install it for your operating system, and open or create a vault. A vault is a folder on your device containing Markdown notes.

QMD is a command-line tool, so configure it on a desktop or laptop that can access the vault folder. The Obsidian mobile app can still sync or edit notes separately; QMD itself runs where you install its CLI.

## 2. Install QMD

The current QMD README lists Node.js 22 or later, or Bun 1.0 or later. Install QMD globally with either package manager:

```sh
npm install -g @tobilu/qmd
```

Or:

```sh
bun install -g @tobilu/qmd
```

On macOS, QMD currently documents Homebrew SQLite as a requirement for extension support:

```sh
brew install sqlite
```

Check the [official QMD installation and requirements](https://github.com/tobi/qmd#installation) for current operating-system details. Then confirm the command is on your PATH:

```sh
qmd --help
```

## 3. Register the vault as a collection

First inspect existing collections so you do not register the same vault twice:

```sh
qmd collection list
```

If it is not already present, replace the path below with the absolute path to your vault:

```sh
qmd collection add "/absolute/path/to/your/vault" --name personal-vault --mask "**/*.md"
qmd collection show personal-vault
qmd update
```

Check that the displayed path is the vault you intended to index. QMD reads the Markdown files in that collection; choose a narrower folder or mask if the vault contains areas you do not want searchable. Do not add the skill repository itself as a collection unless you want to search it.

Optionally describe the collection to help QMD rank results with context:

```sh
qmd context add qmd://personal-vault "Personal Obsidian notes and project handoffs"
```

## 4. Generate local embeddings

```sh
qmd embed -c personal-vault
qmd status
```

QMD downloads local GGUF models when required and stores them in its model cache. The current README lists `embeddinggemma-300M-Q8_0` as the default embedding model at about 300 MB. Its query-expansion and reranking models add about 1.7 GB if a search mode loads them. Actual disk use can vary by model version and cache contents.

For a mostly English vault, start with the default and check search quality. QMD also documents Qwen3-Embedding as a multilingual alternative. For a multilingual or Spanish-heavy vault, you can set this in the `models` section of QMD's `index.yml` (see QMD's [configuration docs](https://github.com/tobi/qmd#configuring-indexyml) for the config path):

```yaml
models:
  embed: "hf:Qwen/Qwen3-Embedding-0.6B-GGUF/Qwen3-Embedding-0.6B-Q8_0.gguf"
```

Then rebuild the collection's vectors with `qmd embed -f -c personal-vault`. Do not switch models casually: vectors from different embedding models are not interchangeable, and the default is English-optimized.

## 5. Verify keyword and semantic search

Keyword search does not require embeddings:

```sh
qmd search "an exact name, date, or phrase in your notes" -c personal-vault -n 3
```

After embeddings are ready, this typed vector query tests semantic search without query expansion or reranking:

```sh
qmd query "vec: a concept described in different words" --no-rerank -c personal-vault -n 3
```

To open a specific result, use its path from QMD:

```sh
qmd get "relative/path/to/note.md" -l 40
```

Use your own note paths and query text. Search only within the intended collection.

## 6. Connect QMD to an MCP client (optional)

QMD exposes a local stdio MCP server. In an MCP-capable assistant, configure the server command as `qmd` with the argument `mcp`, following that assistant's setup instructions. QMD documents client examples and additional transport options in [Using QMD with AI agents](https://github.com/tobi/qmd#using-with-ai-agents).

The CLI works without MCP, so skip this step if you prefer. Keep the server local for personal vault access; do not expose an unauthenticated QMD server to the public internet.

## Keep the index current

After adding or editing notes, update the text index and embed any changed documents:

```sh
qmd update
qmd embed -c personal-vault
```

Routine searches do not require a full re-embed. Use `qmd embed -f` only after changing embedding models or when you intentionally need to regenerate every vector.

## Troubleshooting

- **`qmd` is not found:** restart the terminal after installing Node/Bun packages, then check that the package manager's global bin directory is on `PATH`.
- **A new note does not appear:** run `qmd update`, then `qmd embed -c personal-vault` if you need semantic results for it.
- **Exact names work but semantic search does not:** check `qmd status` and confirm embedding completed for the collection.
- **The wrong notes appear:** inspect `qmd collection show personal-vault`; verify the path and mask, then scope queries to that collection.
- **A model download or native runtime fails:** run `qmd doctor` and compare the result with the current [QMD README](https://github.com/tobi/qmd).

## Privacy

QMD indexes and runs its local models on your device. When an assistant retrieves a note excerpt, it may pass that excerpt to its own model; check that assistant's data-handling settings before indexing or searching confidential notes.
