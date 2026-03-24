# peer-to-peer

set up claude-peers-mcp. two Claude Code sessions, no hierarchy, just messages.

---

## setup

```bash
# clone
git clone https://github.com/louislva/claude-peers-mcp.git ~/claude-peers-mcp
cd ~/claude-peers-mcp
bun install

# register globally so every Claude Code session has it
claude mcp add --scope user --transport stdio claude-peers -- bun ~/claude-peers-mcp/server.ts
```

start two Claude Code sessions with development channels enabled:

```bash
# terminal 1
claude --dangerously-skip-permissions --dangerously-load-development-channels server:claude-peers

# terminal 2 (different directory, ideally different part of a codebase)
claude --dangerously-skip-permissions --dangerously-load-development-channels server:claude-peers
```

tip: alias it so you don't type that every time:
```bash
alias claudepeers='claude --dangerously-load-development-channels server:claude-peers'
```

## how it works

a broker daemon runs on localhost:7899 with a SQLite database. each session spawns an MCP server that registers with the broker and polls for messages every second. inbound messages are pushed via the claude/channel protocol — Claude sees them immediately.

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

broker auto-launches when the first session starts. cleans up dead peers automatically. everything is localhost-only.

## what to try

**round 1: discovery.** in either session, ask: "who else is running?" or "list my peers." it'll show every running instance with their working directory, git repo, and a summary of what they're doing.

**round 2: ad-hoc coordination.** give both agents tasks on the same codebase but different modules. then tell one: "message your peer and ask what they're working on." watch what happens when they discover overlapping concerns.

**round 3: deliberate conflict.** give both agents a task that touches the same file. don't tell them about each other. see if they discover the conflict through messaging, or if they just step on each other.

**round 4: emergent division of labor.** give both agents the same high-level goal (e.g., "improve the test coverage of this project"). don't assign specific files. tell each one to coordinate with its peers. watch how they divide the work — or don't.

## CLI tools

```bash
cd ~/claude-peers-mcp
bun cli.ts status        # broker status + all peers
bun cli.ts peers         # list peers
bun cli.ts send <id> <msg>  # send a message into a Claude session
bun cli.ts kill-broker   # stop the broker
```

## what to notice

- how quickly do the agents establish a division of labor?
- do they develop any conventions or shorthand?
- what happens when they disagree?
- how much of their communication is content vs. meta-coordination?
- compare this to the supervised version in the next exercise

---

*← [back to exercises](../README.md)*
