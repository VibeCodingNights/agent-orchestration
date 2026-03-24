# the coordination tax

measure it. when does adding an agent make things worse?

---

here's the dirty secret of multi-agent systems: they burn tokens fast. four agents coordinating can easily 10x costs versus a single agent doing the same task. you're paying for coordination, not work.

LangGraph measured it: the supervisor's routing calls accounted for over 30% of total response time. that's not the agents thinking about your problem. that's the agents thinking about each other.

production data from Manus AI: agents solving complex tasks average 50 tool calls per task with 100:1 input-to-output token ratios. with context tokens at $0.30-$3.00 per million, inefficient coordination becomes prohibitively expensive at scale.

---

## the math

single agent on a task: maybe 4K tokens total.

four agents on the same task via supervisor pattern:
- supervisor decomposition: ~1K tokens
- supervisor routing (3 workers × assignment): ~1.5K tokens  
- worker 1 execution: ~3K tokens
- worker 2 execution: ~3K tokens
- worker 3 execution: ~3K tokens
- supervisor synthesis: ~2K tokens
- total: ~13.5K tokens

you paid 3.4x more. did you get 3.4x better output? did you get it 3.4x faster? probably not — because much of the work was sequential (supervisor → worker → supervisor).

with context isolation (each worker in its own context window), token usage is *linear* in the number of agents. each worker loads the full CLAUDE.md, MCP configs, and skills. that's fixed overhead per agent.

---

## when more agents make things worse

**sequential dependencies.** if task B depends on task A's output, parallelizing gives you no gain and adds coordination overhead.

**same-file edits.** two agents editing the same file leads to overwrites, regardless of git worktrees. conceptual conflicts — two agents making incompatible design decisions — are worse because they're invisible until integration.

**small tasks.** subagents have overhead: spinning up context, token cost, result synthesis. for small, fast tasks, the overhead exceeds the benefit.

**context accumulation.** in handoff patterns where agents pass conversation history forward, every subsequent agent processes all previous agents' output. token usage grows quadratically with the number of handoffs.

---

## the experiment

tonight, run the same task three ways and measure:

1. **single agent** — one Claude Code session, vanilla
2. **supervised team** — Agent Teams, 3 workers
3. **peer-to-peer** — claude-peers-mcp, 2 sessions

for each, record:
- wall-clock time to completion
- total tokens consumed (check your usage dashboard)
- quality of output (your subjective judgment)
- ratio of coordination tokens to work tokens (estimate from the conversation)

the coordination tax is the gap between what you paid and what a single agent would have cost, adjusted for quality. sometimes it's worth it. sometimes it's not. the point is knowing which.

---

*← [back to provocations](../README.md)*
