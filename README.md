# Context Hub Plugin

Unified context retrieval for Claude Code - orchestrates **Forgetful Memory**, **Context7 documentation**, and **Serena symbol analysis** into a single context-gathering workflow.

## Installation

```bash
claude plugins install context-hub-plugin --marketplace forgetful-plugins
```

Or install from local path:
```bash
claude plugins install /path/to/context-hub-plugin
```

## Prerequisites

Context Hub requires these plugins to be installed:

### Required: Serena (for `/encode-repo-serena`)
```bash
claude plugins install serena
```

### Recommended: Context7 (for framework docs in `/context_gather`)
```bash
claude plugins install context7 --marketplace pleaseai/claude-code-plugins
```

## Setup

Run the setup command to configure Forgetful MCP and verify prerequisites:

```bash
/context-hub-setup
```

This will:
1. Check if Serena and Context7 plugins are installed
2. Configure Forgetful MCP server
3. Report setup status

## Skills

Context Hub includes auto-discovered skills that guide Claude on effective tool usage:

### Forgetful Skills (included)
| Skill | Description |
|-------|-------------|
| `using-forgetful-memory` | When/how to query and create memories, atomic memory principles, importance scoring |
| `curating-memories` | Updating, linking, and marking memories obsolete |
| `exploring-knowledge-graph` | Deep traversal across memories, entities, and relationships |

### Serena Skills
| Skill | Description |
|-------|-------------|
| `using-serena-symbols` | When to use Serena vs grep, symbol path notation, core tools |
| `serena-code-architecture` | Multi-phase architectural analysis workflow with Forgetful integration |

Skills are automatically invoked by Claude when relevant to your task. They include:
- **Tool Quick Reference** - Call Forgetful tools directly without meta-tool discovery
- **TOOL_REFERENCE.md** - Complete schemas for all 38 Forgetful tools

## Commands

### Context Retrieval

| Command | Description | Prerequisites |
|---------|-------------|---------------|
| `/context_gather <task>` | Gather context from all sources | Forgetful, Context7 (recommended) |
| `/encode-repo-serena` | Bootstrap repository into Forgetful using symbol analysis | Forgetful, Serena |

### Memory Management

| Command | Description | Prerequisites |
|---------|-------------|---------------|
| `/memory-search <query>` | Search memories semantically | Forgetful |
| `/memory-list [count]` | List recent memories | Forgetful |
| `/memory-save` | Save current context as atomic memory | Forgetful |
| `/memory-explore <query>` | Deep knowledge graph traversal | Forgetful |

### Setup

| Command | Description |
|---------|-------------|
| `/context-hub-setup` | Configure Forgetful MCP and check plugin prerequisites |

## How It Works

### /context_gather

The main command orchestrates multiple sources:

1. **Forgetful Memory** - Searches your knowledge base across ALL projects
2. **File System** - Reads actual code files referenced by memories
3. **Context7** - Retrieves framework-specific documentation (if installed)
4. **Serena** - Provides symbol-level code analysis (if installed)
5. **WebSearch** - Falls back to web for recent information

Returns a synthesized summary with:
- Relevant memories and their connections
- Code patterns and snippets
- Framework guidance
- Architectural decisions
- Implementation notes

### /encode-repo-serena

Uses Serena's LSP-powered analysis for accurate repo encoding:
- **Symbol extraction** - Classes, functions, methods with precise locations
- **Relationship discovery** - Who calls what, what depends on what
- **Cross-file analysis** - Understand component connections

Stores findings as atomic memories in Forgetful for future retrieval.

## Example Usage

```bash
# Before implementing a feature
/context_gather implement OAuth2 authentication for the API

# Bootstrap a new project into the knowledge base
/encode-repo-serena

# Quick memory search
/memory-search authentication patterns

# Save an important decision
/memory-save
```

## Architecture

```
context-hub-plugin (self-contained)
├── skills/
│   ├── using-forgetful-memory/  ─── Memory usage + Tool Reference
│   ├── curating-memories/       ─── Memory maintenance
│   ├── exploring-knowledge-graph/ ─ Graph traversal
│   ├── using-serena-symbols/    ─── Symbol analysis guidance
│   └── serena-code-architecture/ ── Architecture workflows
├── commands/
│   ├── context_gather.md        ─── Multi-source context retrieval
│   ├── encode-repo-serena.md    ─── Repository encoding
│   └── memory-*.md              ─── Memory management
└── Integrations
    ├── Forgetful (MCP)          ─── Semantic memory storage
    ├── Serena (Plugin)          ─── Symbol-level code analysis
    └── Context7 (Plugin)        ─── Framework documentation
```

**Note**: Context Hub is self-contained - you don't need to install forgetful-plugin separately. All Forgetful skills are included.

## Part of forgetful-plugins

This plugin is part of the [forgetful-plugins](https://github.com/ScottRBK/forgetful-plugin) marketplace.

## License

MIT
