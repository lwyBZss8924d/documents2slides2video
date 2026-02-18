---
name: semtools
description: Semantic search and document parsing tools for the command line. Use when parsing documents (PDF, DOCX, etc.), performing semantic search across files, managing document workspaces, or asking AI-powered questions over document content.
allowed-tools: Bash(parse *), Bash(search *), Bash(workspace *), Bash(ask *)
---

## When to use

Use this skill whenever you need to:

- **Parse documents** into structured text (PDF, DOCX, images, etc.)
- **Semantically search** across parsed document content
- **Manage workspaces** for organizing document collections
- **Ask questions** over document content using an AI agent

## Tools overview

semtools v2.0.0 provides four CLI commands: `parse`, `search`, `workspace`, and `ask`. For detailed usage of each command, see [reference/commands.md](reference/commands.md).

## Quick reference

### Parse documents

```bash
# Parse a single file
parse document.pdf

# Parse multiple files
parse doc1.pdf doc2.docx slides.pptx

# Parse with verbose output
parse -v document.pdf

# Use a specific backend
parse -b llama-parse document.pdf
```

### Semantic search

```bash
# Search across parsed files
search "key concept" file1.txt file2.txt

# Search with more context lines
search -n 5 "query" *.txt

# Return more results
search --top-k 10 "query" *.txt

# Return all results below a distance threshold
search -m 0.5 "query" *.txt

# Case-insensitive search
search -i "query" *.txt

# Search via stdin
cat document.txt | search "query"
```

### Workspace management

```bash
# Create or switch to a workspace
workspace use my-project

# Check active workspace
workspace status

# Clean up stale entries
workspace prune
```

### Ask questions (AI agent)

```bash
# Ask a question about files
ask "What are the main arguments?" document.txt

# Ask with specific model
ask -m gpt-4o "Summarize the key findings" report.txt

# Ask via stdin
cat notes.txt | ask "What action items are mentioned?"
```

## Typical workflow

1. **Set up workspace**: `workspace use <name>` to create/activate a workspace
2. **Parse documents**: `parse *.pdf` to extract text from source documents
3. **Search content**: `search "topic" *.txt` to find relevant passages
4. **Ask questions**: `ask "question" files...` to get AI-powered answers over the content

## Important notes

- The `parse` command defaults to the `llama-parse` backend. Other backends can be specified with `-b`.
- Config is read from `~/.semtools_config.json` by default; override with `-c`.
- The `search` command uses semantic (embedding-based) search, not simple text matching. It finds conceptually related content even when exact words differ.
- The `ask` command uses an OpenAI-compatible API. The API key, base URL, and model can be set via config file, environment variables, or CLI flags.
- The `workspace use` command prints an export command you need to run to activate the workspace in your shell.
