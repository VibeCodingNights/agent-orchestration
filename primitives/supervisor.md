# Supervisor

One agent to rule them all.

---

The supervisor pattern is the most natural thing in the world. You have a complex task. You break it into pieces. You assign each piece to a specialist. You collect the results. Synthesize. Done.

This is how Claude Code Agent Teams works. A team lead coordinates via a shared task list. Teammates claim tasks, work in isolated git worktrees, message each other through an inbox. The lead doesn't do the work — it *orchestrates* the work.

pentagon.run makes this spatial and visible. SOUL.md defines who the agent is. HEARTBEAT.md keeps it alive. TASKS.md tells it what to do. MEMORY.md lets it remember. Every agent has a desk on a canvas. You see who's active, who's stalled, who's idle. No terminal tabs. One screen, complete awareness.

Gas Town calls its supervisor the "mayor." Multiclaude calls it the "supervisor agent." The pattern is the same everywhere because it works.

---

## When it works

The supervisor pattern is the most production-ready approach for most teams. It's debuggable — you can trace every decision back to the supervisor's reasoning. It's auditable — there's a clear record of who did what. It's predictable — the control flow is explicit, not emergent.

Start with 3-5 teammates. 5-6 tasks per teammate keeps everyone productive. This isn't a guess — it's the empirical finding from teams actually running these systems.

The pattern works best when:
- Tasks are genuinely independent (different files, different modules)
- Each worker's output doesn't depend on another worker's intermediate state
- The synthesis step is well-defined
- You need a human-readable audit trail

---

## When it breaks

**The 30% tax.** LangGraph measured this directly: the supervisor's routing calls accounted for over 30% of total response time. Every request triggers multiple sequential LLM calls — first to the supervisor, then to workers, then back to the supervisor. You're paying for coordination, not work.

**The context bottleneck.** The supervisor must hold the full task description, all worker results, and enough context to synthesize a final response. For tasks producing more than 50 intermediate results, this exceeds current context window limits even on 128k token models.

**The single point of failure.** If the supervisor hallucinates the task decomposition, every worker builds on a broken foundation. If the supervisor misroutes, work gets wasted. There's no peer to check the supervisor's reasoning.

**Diminishing returns.** Beyond a certain point, additional teammates don't speed up work proportionally. The coordination overhead grows faster than the parallelism gains. Addy Osmani's observation: the skills that make someone a strong engineering manager translate directly into effective agent orchestration. Task sizing matters. File ownership matters. Context loading matters.

---

## The practical shape

```
Lead Agent
├── skills: project-conventions
├── MCP: filesystem, github
│
├── Teammate: API Designer
│   └── skills: api-design, openapi-spec
│
├── Teammate: Implementer
│   └── skills: payment-patterns, error-handling
│
└── Teammate: Reviewer + Test Writer
    └── skills: code-review, test-writing
```

Claude Code Agent Teams:
```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

Then: "create an agent team with 3 teammates to refactor the payment module. One for the API layer, one for database migrations, one for test coverage."

The lead spawns three independent teammates, coordinates through a shared task list and direct messaging. Each teammate owns their scope. No conflicts, no isolation.

pentagon.run does the same thing but you *see* it — a spatial canvas instead of terminal output.

---

## What to notice

When you run a supervised team, pay attention to:
- How much time the lead spends routing vs. the workers spend working
- What happens when two workers need to touch the same file
- What happens when a worker produces output that contradicts another worker's assumptions
- Whether the lead catches errors or just passes them through

The supervisor pattern doesn't solve coordination. It *centralizes* coordination. That's a different thing.

---

*← [back to primitives](../README.md)*
