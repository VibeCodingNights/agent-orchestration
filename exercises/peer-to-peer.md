# Peer-to-peer

Set up claude-peers-mcp. Two Claude Code sessions, no hierarchy, just messages.

---

## Setup

```bash
# clone
git clone https://github.com/louislva/claude-peers-mcp.git ~/claude-peers-mcp
cd ~/claude-peers-mcp
bun install

# register globally so every Claude Code session has it
claude mcp add --scope user --transport stdio claude-peers -- bun ~/claude-peers-mcp/server.ts
```

Start two Claude Code sessions with development channels enabled:

```bash
# terminal 1
claude --dangerously-skip-permissions --dangerously-load-development-channels server:claude-peers

# terminal 2 (different directory, ideally different part of a codebase)
claude --dangerously-skip-permissions --dangerously-load-development-channels server:claude-peers
```

Tip: alias it so you don't type that every time:
```bash
alias claudepeers='claude --dangerously-load-development-channels server:claude-peers'
```

## How it works

A broker daemon runs on localhost:7899 with a SQLite database. Each session spawns an MCP server that registers with the broker and polls for messages every second. Inbound messages are pushed via the claude/channel protocol — Claude sees them immediately.

```
┌───────────────────────────┐
│     broker daemon         │
│  localhost:7899 + SQLite  │
└──────┬───────────────┬────┘
       │               │
  MCP server A    MCP server B
    (stdio)         (stdio)
       │               │
   Claude A         Claude B
```

Broker auto-launches when the first session starts. Cleans up dead peers automatically. Everything is localhost-only.

## What to try

**Round 1: Discovery.** In either session, ask: "who else is running?" or "list my peers." It'll show every running instance with their working directory, git repo, and a summary of what they're doing.

**Round 2: Ad-hoc coordination.** Give both agents tasks on the same codebase but different modules. Then tell one: "message your peer and ask what they're working on." Watch what happens when they discover overlapping concerns.

**Round 3: Deliberate conflict.** Give both agents a task that touches the same file. Don't tell them about each other. See if they discover the conflict through messaging, or if they just step on each other.

**Round 4: Emergent division of labor.** Give both agents the same high-level goal (e.g., "improve the test coverage of this project"). Don't assign specific files. Tell each one to coordinate with its peers. Watch how they divide the work — or don't.

## CLI tools

```bash
cd ~/claude-peers-mcp
bun cli.ts status        # broker status + all peers
bun cli.ts peers         # list peers
bun cli.ts send <id> <msg>  # send a message into a Claude session
bun cli.ts kill-broker   # stop the broker
```

## What to notice

- How quickly do the agents establish a division of labor?
- Do they develop any conventions or shorthand?
- What happens when they disagree?
- How much of their communication is content vs. meta-coordination?
- Compare this to the supervised version in the next exercise

---

*← [back to exercises](../README.md)*
