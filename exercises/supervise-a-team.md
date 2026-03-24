# supervise a team

pentagon.run or Claude Code Agent Teams. assign roles, watch the canvas, feel the overhead.

---

## option A: pentagon.run

pentagon is a native macOS app. spatial canvas. every agent has a desk.

the key abstraction is file-based identity:
- **SOUL.md** — who the agent is. its role, capabilities, communication style.
- **HEARTBEAT.md** — keeps it alive. status, last activity.
- **TASKS.md** — what it's working on.
- **MEMORY.md** — what it remembers across sessions.

configure once — every session starts with full context. "Atlas handles the backend. Scout owns the frontend. They know their boundaries."

pentagon is coming soon (native macOS, not yet released as of March 2026). if you can't run it tonight, use option B.

## option B: Claude Code Agent Teams

Agent Teams is built into Claude Code. experimental, disabled by default.

```bash
# enable
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# or in settings.json
```

then prompt:

```
create an agent team with 3 teammates to refactor the auth module.
one for the API layer, one for database migrations, one for test coverage.
have them coordinate through the shared task list.
```

Claude creates a team lead, spawns three independent teammates, coordinates through a shared task list and direct messaging. each teammate runs in its own context window. teammates get your project's CLAUDE.md, MCP servers, and skills automatically — but they don't inherit the lead's conversation history.

if you're in tmux or iTerm2, each teammate opens in its own split pane.

## what to try

**round 1: the same task as peer-to-peer.** take whatever you tried in the peer-to-peer exercise and run it with a supervisor. same codebase, same goal. compare: time to completion, token usage, quality of result, how conflicts got resolved.

**round 2: feel the overhead.** watch the team lead. how much of its output is routing ("I'll assign this to teammate 2") vs. actual synthesis? count the tokens spent on coordination. the 30% number from LangGraph research is your benchmark — do you see something similar?

**round 3: the file ownership problem.** deliberately create a task where two teammates need to touch the same file. watch what happens. agent teams use git worktrees to isolate workspaces, but conceptual conflicts (two agents making incompatible design decisions about the same module) aren't solved by file isolation.

**round 4: scale it.** add a fourth teammate. then a fifth. at what point does coordination overhead exceed the benefit of parallelism? for most workflows, 3-5 teammates is the empirical sweet spot. verify it yourself.

## comparing patterns

run the same task three ways:

| approach | setup | what to measure |
|----------|-------|----------------|
| single agent | one Claude Code session | time, tokens, quality |
| peers (claude-peers-mcp) | two sessions, no hierarchy | time, tokens, quality, coordination behavior |
| supervised (Agent Teams) | team lead + 3 workers | time, tokens, quality, coordination overhead |

the comparison is the exercise. there's no right answer — different tasks favor different patterns. the point is building intuition for when each one helps.

## what to notice

- how much of the lead's work is "thinking about the problem" vs. "managing the team"?
- do teammates ever challenge the lead's decomposition?
- what happens when a teammate finishes early — does it find useful work or idle?
- is the final output better, worse, or the same as what a single agent would produce?
- where do you see the diminishing returns kick in?

---

*← [back to exercises](../README.md)*
