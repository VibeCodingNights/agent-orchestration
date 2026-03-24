# Supervise a team

pentagon.run or Claude Code Agent Teams. Assign roles, watch the canvas, feel the overhead.

---

## Option A: pentagon.run

Pentagon is a native macOS app. Spatial canvas. Every agent has a desk.

The key abstraction is file-based identity:
- **SOUL.md** — Who the agent is. Its role, capabilities, communication style.
- **HEARTBEAT.md** — Keeps it alive. Status, last activity.
- **TASKS.md** — What it's working on.
- **MEMORY.md** — What it remembers across sessions.

Configure once — every session starts with full context. "Atlas handles the backend. Scout owns the frontend. They know their boundaries."

Pentagon is coming soon (native macOS, not yet released as of March 2026). If you can't run it tonight, use Option B.

## Option B: Claude Code Agent Teams

Agent Teams is built into Claude Code. Experimental, disabled by default.

```bash
# enable
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# or in settings.json
```

Then prompt:

```
create an agent team with 3 teammates to refactor the auth module.
one for the API layer, one for database migrations, one for test coverage.
have them coordinate through the shared task list.
```

Claude creates a team lead, spawns three independent teammates, coordinates through a shared task list and direct messaging. Each teammate runs in its own context window. Teammates get your project's CLAUDE.md, MCP servers, and skills automatically — but they don't inherit the lead's conversation history.

If you're in tmux or iTerm2, each teammate opens in its own split pane.

## What to try

**Round 1: The same task as peer-to-peer.** Take whatever you tried in the peer-to-peer exercise and run it with a supervisor. Same codebase, same goal. Compare: time to completion, token usage, quality of result, how conflicts got resolved.

**Round 2: Feel the overhead.** Watch the team lead. How much of its output is routing ("I'll assign this to teammate 2") vs. actual synthesis? Count the tokens spent on coordination. The 30% number from LangGraph research is your benchmark — do you see something similar?

**Round 3: The file ownership problem.** Deliberately create a task where two teammates need to touch the same file. Watch what happens. Agent Teams use git worktrees to isolate workspaces, but conceptual conflicts (two agents making incompatible design decisions about the same module) aren't solved by file isolation.

**Round 4: Scale it.** Add a fourth teammate. Then a fifth. At what point does coordination overhead exceed the benefit of parallelism? For most workflows, 3-5 teammates is the empirical sweet spot. Verify it yourself.

## Comparing patterns

Run the same task three ways:

| Approach | Setup | What to measure |
|----------|-------|----------------|
| Single agent | One Claude Code session | Time, tokens, quality |
| Peers (claude-peers-mcp) | Two sessions, no hierarchy | Time, tokens, quality, coordination behavior |
| Supervised (Agent Teams) | Team lead + 3 workers | Time, tokens, quality, coordination overhead |

The comparison is the exercise. There's no right answer — different tasks favor different patterns. The point is building intuition for when each one helps.

## What to notice

- How much of the lead's work is "thinking about the problem" vs. "managing the team"?
- Do teammates ever challenge the lead's decomposition?
- What happens when a teammate finishes early — does it find useful work or idle?
- Is the final output better, worse, or the same as what a single agent would produce?
- Where do you see the diminishing returns kick in?

---

*← [back to exercises](../README.md)*
