# Swarm

No boss. Emergent coordination.

---

The swarm pattern eliminates centralized control entirely. Agents operate as autonomous peers that make local decisions based on shared state, environment signals, or direct messages. There's no orchestrator. Coordination emerges from simple local rules applied by many agents simultaneously.

This is the principle behind ant colonies, bird flocks, and blockchain consensus. It's also the principle behind claude-peers-mcp: a broker daemon on localhost, a SQLite database, and every Claude Code session can message every other one. No hierarchy. No task list. Just "what are you working on?" and "I found something you should know."

MiroShark takes this to its logical extreme: upload a document, and the system generates hundreds of AI agents with unique personalities, memories, and social connections. They argue, persuade, form coalitions, shift positions. What emerges is a prediction — not programmed, but grown from interaction.

---

## The ant colony principle

An individual ant is nearly brainless. But a colony of ants solves optimization problems — shortest paths, resource allocation, load balancing — that are computationally hard. They do it through stigmergy: indirect coordination by modifying the shared environment. An ant lays a pheromone trail. Other ants follow strong trails. Trails that lead to food get reinforced. Trails that lead nowhere evaporate. No ant understands the system. The system understands itself.

In agent swarms, the equivalent is shared state. One agent processes files in a directory and moves them to an output directory. Another agent monitors the output directory and begins its work when new files appear. Nobody told them to coordinate. The file system is the pheromone trail.

Claude-peers-mcp is slightly more explicit — agents send text messages through a broker — but the philosophy is the same. No routing logic. No task decomposition. Agents discover what other agents are doing and adapt.

---

## The convergence problem

The hard question with swarms: how does the system know when it's done?

Without an orchestrator deciding when to stop, agents need explicit termination conditions — max iterations, quality thresholds, timeout-based convergence. Design these conditions carefully. Too aggressive and you get incomplete results. Too conservative and you burn tokens forever.

MiroShark handles this with simulation cycles — a fixed number of interaction rounds, after which a ReportAgent analyzes what emerged. But for open-ended software engineering tasks, convergence is harder. When is a codebase "done"? When is a refactor "complete"? The swarm doesn't know, and nobody's watching to tell it.

---

## MiroFish / MiroShark: Swarm intelligence at scale

A 20-year-old student built MiroFish in ten days via vibe coding. It's built on OASIS (Open Agent Social Interaction Simulations) from CAMEL-AI — a framework that can scale to one million agents with 23 different social actions: following, commenting, reposting, liking, muting, searching.

The process:
1. Feed it a source document — news, financial report, policy draft, even a novel
2. It extracts entities and relationships to build a knowledge graph
3. From the graph, it generates thousands of agent personas with unique personalities, backgrounds, stances
4. Agents are released into simulated social platforms (Twitter-like, Reddit-like)
5. They interact freely: post, comment, debate, shift positions
6. A ReportAgent analyzes what emerged — dominant opinions, polarization, coalitions, narrative shifts

One developer plugged it into a Polymarket trading bot: simulated 2,847 digital humans before every trade, reported $4,266 profit over 338 trades.

MiroShark is the open fork that runs locally with Neo4j + Ollama. You can chat with any individual agent after the simulation to understand their reasoning.

This isn't agent orchestration in the usual sense. Nobody's completing tickets. But it's the purest expression of swarm intelligence: give many agents simple rules, watch complex behavior emerge.

---

## Swarm vs. supervisor: the real tradeoff

| | Supervisor | Swarm |
|---|---|---|
| Control | Explicit, centralized | Emergent, distributed |
| Debuggability | High — trace every decision | Low — emergent behavior is hard to explain |
| Bottleneck | Supervisor is SPOF | Convergence is the hard problem |
| Overhead | 30%+ routing tax | Coordination through shared state (cheaper per-agent) |
| Best for | Well-decomposable tasks | Exploration, diverse perspectives, resilience |
| Failure mode | Supervisor hallucinates decomposition | Agents never converge, or converge on wrong answer |

Most production systems are hybrid: a pipeline for structure, a swarm for exploration within a stage. A supervisor to synthesize, peers to do the actual work.

---

## What to notice

When you run a swarm, pay attention to:
- What coordination behaviors emerge that you didn't program
- How long it takes to converge (or whether it converges at all)
- Whether the emergent result is better or worse than what a single agent would produce
- What happens when one agent is confidently wrong — does the swarm correct it or amplify it?

That last one is the cascading hallucination problem. In a supervisor system, you have one point of failure. In a swarm, you have N points of amplification.

---

*← [back to primitives](../README.md)*
