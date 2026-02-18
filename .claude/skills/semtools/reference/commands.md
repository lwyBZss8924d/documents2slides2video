# semtools CLI Command Reference

## parse

Parse documents into structured text using various backends.

```
Usage: parse [OPTIONS] <FILES>...

Arguments:
  <FILES>...  Files to parse

Options:
  -c, --config <CONFIG>    Path to config file (default: ~/.semtools_config.json)
  -b, --backend <BACKEND>  Backend type for parsing (default: llama-parse)
  -v, --verbose            Verbose output while parsing
  -h, --help               Print help
  -V, --version            Print version
```

### Notes

- Accepts multiple files in a single invocation.
- The default backend is `llama-parse`. Use `-b` to specify alternatives.
- Use `-v` when debugging parsing issues to see detailed progress.

---

## search

Fast semantic keyword search across files.

```
Usage: search [OPTIONS] <QUERY> [FILES]...

Arguments:
  <QUERY>     Query to search for
  [FILES]...  Files to search (optional if using stdin)

Options:
  -n, --n-lines <N_LINES>            Context lines before/after match [default: 3]
      --top-k <TOP_K>                Top-k results to return (ignored if max_distance is set) [default: 3]
  -m, --max-distance <MAX_DISTANCE>  Return all results below this distance threshold (0.0+)
  -i, --ignore-case                  Case-insensitive search
  -h, --help                         Print help
  -V, --version                      Print version
```

### Notes

- This is **semantic search**, not grep. It finds conceptually related content.
- Reads from stdin when no files are given: `cat file.txt | search "query"`.
- `--top-k` controls how many results are returned. Ignored when `--max-distance` is set.
- `--max-distance` returns all results within a similarity threshold. Lower values = more similar.
- `-n` controls how many lines of context surround each match.

---

## workspace

Manage semtools workspaces for organizing document collections.

```
Usage: workspace <COMMAND>

Commands:
  use     Use or create a workspace (prints export command to run)
  status  Show active workspace and basic stats
  prune   Remove stale or missing files from store
  help    Print help for subcommands
```

### Subcommands

**`workspace use <name>`** — Create a new workspace or switch to an existing one. Prints an `export` command that must be run in your shell to activate.

**`workspace status`** — Display the currently active workspace and statistics (file count, etc.).

**`workspace prune`** — Clean up references to files that have been moved or deleted.

---

## ask

AI-powered question answering over document content.

```
Usage: ask [OPTIONS] <QUERY> [FILES]...

Arguments:
  <QUERY>     Query to prompt the agent with
  [FILES]...  Files to search (optional if using stdin)

Options:
  -c, --config <CONFIG>      Path to config file (default: ~/.semtools_config.json)
      --api-key <API_KEY>    OpenAI API key (overrides config and env var)
      --base-url <BASE_URL>  OpenAI base URL (overrides config)
  -m, --model <MODEL>        Model to use (overrides config)
      --api-mode <API_MODE>  API mode: 'chat' or 'responses' (overrides config)
  -h, --help                 Print help
  -V, --version              Print version
```

### Notes

- Uses an OpenAI-compatible API under the hood.
- API key resolution order: `--api-key` flag > config file > `OPENAI_API_KEY` env var.
- Reads from stdin when no files are given: `cat file.txt | ask "question"`.
- The `--api-mode` flag switches between OpenAI's chat completions and responses API.

---

## Configuration

All tools read from `~/.semtools_config.json` by default. Override with `-c <path>`.

The config file can set:
- API key and base URL for the `ask` command
- Default model for AI queries
- Default backend for parsing
- API mode (chat vs responses)
