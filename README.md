# Agent orchestration

Your agents work alone. Even when there are five of them.

---

You can spawn a team now. Team lead, three workers, shared task list. It looks like coordination. The agents message each other, claim tasks, report back. It feels like a team.

Then one agent rewrites a file another agent is reading. A third agent gives confident advice based on a hallucination from the first. The "team lead" spends 30% of its tokens just routing — not thinking. You add a fourth worker and everything gets *slower*.

These aren't bugs in the framework. They're the fundamental problems of distributed systems, consensus, and coordination that computer scientists have been working on for fifty years. The frameworks give you plumbing. The hard problems are underneath.

---

## The two patterns

Every multi-agent system you'll ever build is some mixture of two ideas:

**The supervisor** — One agent owns control flow. It decides who acts, when, with what context. Debuggable. Auditable. A single point of failure and a throughput bottleneck. pentagon.run makes this spatial and visual — a canvas where every agent has a desk.

**The swarm** — No boss. Agents coordinate through shared state, environment signals, or direct peer-to-peer messaging. Coordination emerges from local rules. The ant colony. claude-peers-mcp is the minimal version: a broker, messages, and nothing else. MiroShark is the extreme: thousands of agents with personalities forming emergent consensus.

Most production systems end up hybrid. A pipeline for structure, a swarm for exploration. A supervisor for synthesis, peers for the actual work.

---

## Primitives

The load-bearing concepts. Each one grounded in where the research actually is.

- **[supervisor](primitives/supervisor.md)** — One agent to rule them all. When it works, when it breaks, what it costs.
- **[swarm](primitives/swarm.md)** — No boss, emergent coordination. From ant colonies to a million simulated humans.
- **[communication](primitives/communication.md)** — How agents actually talk. Text as bottleneck. KV-cache sharing as frontier.
- **[memory](primitives/memory.md)** — Shared state is a lie. Consensus memory, context rot, the consistency models nobody's applying.

## Exercises

Hands-on. Pick one, build something, break something.

- **[peer-to-peer](exercises/peer-to-peer.md)** — claude-peers-mcp. Two sessions, no hierarchy, just messages.
- **[supervise a team](exercises/supervise-a-team.md)** — pentagon.run or Agent Teams. Assign roles, watch the canvas, feel the overhead.
- **[spawn a swarm](exercises/spawn-a-swarm.md)** — MiroShark. Feed it a document, watch hundreds of agents argue their way to consensus.

## Provocations

The unsolved problems. The conversations worth having.

- **[the coordination tax](provocations/the-coordination-tax.md)** — Measure it. When does adding an agent make things worse?
- **[beyond text](provocations/beyond-text.md)** — What if agents could share thoughts instead of words?
- **[your reports are stochastic](provocations/your-reports-are-stochastic.md)** — You're already an engineering manager. But your reports hallucinate.

---

**Tools for tonight:**

| Tool | What it is | Pattern |
|------|-----------|---------|
| [claude-peers-mcp](https://github.com/louislva/claude-peers-mcp) | Peer-to-peer messaging between Claude Code sessions | Swarm / stigmergy |
| [pentagon.run](https://www.pentagon.run/) | Spatial canvas for managing agent teams | Supervisor / mission control |
| [MiroShark](https://github.com/aaronjmars/MiroShark) | Multi-agent simulation engine, hundreds of agents with personalities | Swarm intelligence at scale |

**Bring** a codebase that needs parallel work, a document worth simulating reactions to, or just curiosity about why your agents keep stepping on each other.

**Leave** with an intuition for when coordination helps and when it hurts — and the patterns underneath both.

No lectures. Just the structures underneath.
