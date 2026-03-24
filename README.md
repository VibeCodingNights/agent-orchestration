# agent orchestration

your agents work alone. even when there are five of them.

---

you can spawn a team now. team lead, three workers, shared task list. it looks like coordination. the agents message each other, claim tasks, report back. it feels like a team.

then one agent rewrites a file another agent is reading. a third agent gives confident advice based on a hallucination from the first. the "team lead" spends 30% of its tokens just routing — not thinking. you add a fourth worker and everything gets *slower*.

these aren't bugs in the framework. they're the fundamental problems of distributed systems, consensus, and coordination that computer scientists have been working on for fifty years. the frameworks give you plumbing. the hard problems are underneath.

---

## the two patterns

every multi-agent system you'll ever build is some mixture of two ideas:

**the supervisor** — one agent owns control flow. it decides who acts, when, with what context. debuggable. auditable. a single point of failure and a throughput bottleneck. pentagon.run makes this spatial and visual — a canvas where every agent has a desk.

**the swarm** — no boss. agents coordinate through shared state, environment signals, or direct peer-to-peer messaging. coordination emerges from local rules. the ant colony. claude-peers-mcp is the minimal version: a broker, messages, and nothing else. MiroShark is the extreme: thousands of agents with personalities forming emergent consensus.

most production systems end up hybrid. a pipeline for structure, a swarm for exploration. a supervisor for synthesis, peers for the actual work.

---

## primitives

the load-bearing concepts. each one grounded in where the research actually is.

- **[supervisor](primitives/supervisor.md)** — one agent to rule them all. when it works, when it breaks, what it costs.
- **[swarm](primitives/swarm.md)** — no boss, emergent coordination. from ant colonies to a million simulated humans.
- **[communication](primitives/communication.md)** — how agents actually talk. text as bottleneck. KV-cache sharing as frontier.
- **[memory](primitives/memory.md)** — shared state is a lie. consensus memory, context rot, the consistency models nobody's applying.

## exercises

hands-on. pick one, build something, break something.

- **[peer-to-peer](exercises/peer-to-peer.md)** — claude-peers-mcp. two sessions, no hierarchy, just messages.
- **[supervise a team](exercises/supervise-a-team.md)** — pentagon.run or agent teams. assign roles, watch the canvas, feel the overhead.
- **[spawn a swarm](exercises/spawn-a-swarm.md)** — MiroShark. feed it a document, watch hundreds of agents argue their way to consensus.

## provocations

the unsolved problems. the conversations worth having.

- **[the coordination tax](provocations/the-coordination-tax.md)** — measure it. when does adding an agent make things worse?
- **[beyond text](provocations/beyond-text.md)** — what if agents could share thoughts instead of words?
- **[your reports are stochastic](provocations/your-reports-are-stochastic.md)** — you're already an engineering manager. but your reports hallucinate.

---

**tools for tonight:**

| tool | what it is | pattern |
|------|-----------|---------|
| [claude-peers-mcp](https://github.com/louislva/claude-peers-mcp) | peer-to-peer messaging between Claude Code sessions | swarm / stigmergy |
| [pentagon.run](https://www.pentagon.run/) | spatial canvas for managing agent teams | supervisor / mission control |
| [MiroShark](https://github.com/aaronjmars/MiroShark) | multi-agent simulation engine, hundreds of agents with personalities | swarm intelligence at scale |

**bring** a codebase that needs parallel work, a document worth simulating reactions to, or just curiosity about why your agents keep stepping on each other.

**leave** with an intuition for when coordination helps and when it hurts — and the patterns underneath both.

no lectures. just the structures underneath.
