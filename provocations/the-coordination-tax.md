# The coordination tax

Measure it. When does adding an agent make things worse?

---

Here's the dirty secret of multi-agent systems: they burn tokens fast. Four agents coordinating can easily 10x costs versus a single agent doing the same task. You're paying for coordination, not work.

LangGraph measured it: the supervisor's routing calls accounted for over 30% of total response time. That's not the agents thinking about your problem. That's the agents thinking about each other.

Production data from Manus AI: agents solving complex tasks average 50 tool calls per task with 100:1 input-to-output token ratios. With context tokens at $0.30-$3.00 per million, inefficient coordination becomes prohibitively expensive at scale.

---

## The math

Single agent on a task: maybe 4K tokens total.

Four agents on the same task via supervisor pattern:
- Supervisor decomposition: ~1K tokens
- Supervisor routing (3 workers × assignment): ~1.5K tokens
- Worker 1 execution: ~3K tokens
- Worker 2 execution: ~3K tokens
- Worker 3 execution: ~3K tokens
- Supervisor synthesis: ~2K tokens
- Total: ~13.5K tokens

You paid 3.4x more. Did you get 3.4x better output? Did you get it 3.4x faster? Probably not — because much of the work was sequential (supervisor → worker → supervisor).

With context isolation (each worker in its own context window), token usage is *linear* in the number of agents. Each worker loads the full CLAUDE.md, MCP configs, and skills. That's fixed overhead per agent.

---

## When more agents make things worse

**Sequential dependencies.** If task B depends on task A's output, parallelizing gives you no gain and adds coordination overhead.

**Same-file edits.** Two agents editing the same file leads to overwrites, regardless of git worktrees. Conceptual conflicts — two agents making incompatible design decisions — are worse because they're invisible until integration.

**Small tasks.** Subagents have overhead: spinning up context, token cost, result synthesis. For small, fast tasks, the overhead exceeds the benefit.

**Context accumulation.** In handoff patterns where agents pass conversation history forward, every subsequent agent processes all previous agents' output. Token usage grows quadratically with the number of handoffs.

---

## The experiment

Tonight, run the same task three ways and measure:

1. **Single agent** — one Claude Code session, vanilla
2. **Supervised team** — Agent Teams, 3 workers
3. **Peer-to-peer** — claude-peers-mcp, 2 sessions

For each, record:
- Wall-clock time to completion
- Total tokens consumed (check your usage dashboard)
- Quality of output (your subjective judgment)
- Ratio of coordination tokens to work tokens (estimate from the conversation)

The coordination tax is the gap between what you paid and what a single agent would have cost, adjusted for quality. Sometimes it's worth it. Sometimes it's not. The point is knowing which.

---

*← [back to provocations](../README.md)*
