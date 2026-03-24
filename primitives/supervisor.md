# supervisor

one agent to rule them all.

---

the supervisor pattern is the most natural thing in the world. you have a complex task. you break it into pieces. you assign each piece to a specialist. you collect the results. synthesize. done.

this is how Claude Code agent teams works. a team lead coordinates via a shared task list. teammates claim tasks, work in isolated git worktrees, message each other through an inbox. the lead doesn't do the work — it *orchestrates* the work.

pentagon.run makes this spatial and visible. SOUL.md defines who the agent is. HEARTBEAT.md keeps it alive. TASKS.md tells it what to do. MEMORY.md lets it remember. every agent has a desk on a canvas. you see who's active, who's stalled, who's idle. no terminal tabs. one screen, complete awareness.

Gas Town calls its supervisor the "mayor." Multiclaude calls it the "supervisor agent." the pattern is the same everywhere because it works.

---

## when it works

the supervisor pattern is the most production-ready approach for most teams. it's debuggable — you can trace every decision back to the supervisor's reasoning. it's auditable — there's a clear record of who did what. it's predictable — the control flow is explicit, not emergent.

start with 3-5 teammates. 5-6 tasks per teammate keeps everyone productive. this isn't a guess — it's the empirical finding from teams actually running these systems.

the pattern works best when:
- tasks are genuinely independent (different files, different modules)
- each worker's output doesn't depend on another worker's intermediate state
- the synthesis step is well-defined
- you need a human-readable audit trail

---

## when it breaks

**the 30% tax.** LangGraph measured this directly: the supervisor's routing calls accounted for over 30% of total response time. every request triggers multiple sequential LLM calls — first to the supervisor, then to workers, then back to the supervisor. you're paying for coordination, not work.

**the context bottleneck.** the supervisor must hold the full task description, all worker results, and enough context to synthesize a final response. for tasks producing more than 50 intermediate results, this exceeds current context window limits even on 128k token models.

**the single point of failure.** if the supervisor hallucinates the task decomposition, every worker builds on a broken foundation. if the supervisor misroutes, work gets wasted. there's no peer to check the supervisor's reasoning.

**diminishing returns.** beyond a certain point, additional teammates don't speed up work proportionally. the coordination overhead grows faster than the parallelism gains. Addy Osmani's observation: the skills that make someone a strong engineering manager translate directly into effective agent orchestration. task sizing matters. file ownership matters. context loading matters.

---

## the practical shape

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

Claude Code agent teams:
```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

then: "create an agent team with 3 teammates to refactor the payment module. one for the API layer, one for database migrations, one for test coverage."

the lead spawns three independent teammates, coordinates through a shared task list and direct messaging. each teammate owns their scope. no conflicts, no isolation.

pentagon.run does the same thing but you *see* it — a spatial canvas instead of terminal output.

---

## what to notice

when you run a supervised team, pay attention to:
- how much time the lead spends routing vs. the workers spend working
- what happens when two workers need to touch the same file
- what happens when a worker produces output that contradicts another worker's assumptions
- whether the lead catches errors or just passes them through

the supervisor pattern doesn't solve coordination. it *centralizes* coordination. that's a different thing.

---

*← [back to primitives](../README.md)*
