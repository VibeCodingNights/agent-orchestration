# swarm

no boss. emergent coordination.

---

the swarm pattern eliminates centralized control entirely. agents operate as autonomous peers that make local decisions based on shared state, environment signals, or direct messages. there's no orchestrator. coordination emerges from simple local rules applied by many agents simultaneously.

this is the principle behind ant colonies, bird flocks, and blockchain consensus. it's also the principle behind claude-peers-mcp: a broker daemon on localhost, a SQLite database, and every Claude Code session can message every other one. no hierarchy. no task list. just "what are you working on?" and "I found something you should know."

MiroShark takes this to its logical extreme: upload a document, and the system generates hundreds of AI agents with unique personalities, memories, and social connections. they argue, persuade, form coalitions, shift positions. what emerges is a prediction — not programmed, but grown from interaction.

---

## the ant colony principle

an individual ant is nearly brainless. but a colony of ants solves optimization problems — shortest paths, resource allocation, load balancing — that are computationally hard. they do it through stigmergy: indirect coordination by modifying the shared environment. an ant lays a pheromone trail. other ants follow strong trails. trails that lead to food get reinforced. trails that lead nowhere evaporate. no ant understands the system. the system understands itself.

in agent swarms, the equivalent is shared state. one agent processes files in a directory and moves them to an output directory. another agent monitors the output directory and begins its work when new files appear. nobody told them to coordinate. the file system is the pheromone trail.

claude-peers-mcp is slightly more explicit — agents send text messages through a broker — but the philosophy is the same. no routing logic. no task decomposition. agents discover what other agents are doing and adapt.

---

## the convergence problem

the hard question with swarms: how does the system know when it's done?

without an orchestrator deciding when to stop, agents need explicit termination conditions — max iterations, quality thresholds, timeout-based convergence. design these conditions carefully. too aggressive and you get incomplete results. too conservative and you burn tokens forever.

MiroShark handles this with simulation cycles — a fixed number of interaction rounds, after which a ReportAgent analyzes what emerged. but for open-ended software engineering tasks, convergence is harder. when is a codebase "done"? when is a refactor "complete"? the swarm doesn't know, and nobody's watching to tell it.

---

## MiroFish / MiroShark: swarm intelligence at scale

a 20-year-old student built MiroFish in ten days via vibe coding. it's built on OASIS (open agent social interaction simulations) from CAMEL-AI — a framework that can scale to one million agents with 23 different social actions: following, commenting, reposting, liking, muting, searching.

the process:
1. feed it a source document — news, financial report, policy draft, even a novel
2. it extracts entities and relationships to build a knowledge graph
3. from the graph, it generates thousands of agent personas with unique personalities, backgrounds, stances
4. agents are released into simulated social platforms (Twitter-like, Reddit-like)
5. they interact freely: post, comment, debate, shift positions
6. a ReportAgent analyzes what emerged — dominant opinions, polarization, coalitions, narrative shifts

one developer plugged it into a Polymarket trading bot: simulated 2,847 digital humans before every trade, reported $4,266 profit over 338 trades.

MiroShark is the open fork that runs locally with Neo4j + Ollama. you can chat with any individual agent after the simulation to understand their reasoning.

this isn't agent orchestration in the usual sense. nobody's completing tickets. but it's the purest expression of swarm intelligence: give many agents simple rules, watch complex behavior emerge.

---

## swarm vs. supervisor: the real tradeoff

| | supervisor | swarm |
|---|---|---|
| control | explicit, centralized | emergent, distributed |
| debuggability | high — trace every decision | low — emergent behavior is hard to explain |
| bottleneck | supervisor is SPOF | convergence is the hard problem |
| overhead | 30%+ routing tax | coordination through shared state (cheaper per-agent) |
| best for | well-decomposable tasks | exploration, diverse perspectives, resilience |
| failure mode | supervisor hallucinates decomposition | agents never converge, or converge on wrong answer |

most production systems are hybrid: a pipeline for structure, a swarm for exploration within a stage. a supervisor to synthesize, peers to do the actual work.

---

## what to notice

when you run a swarm, pay attention to:
- what coordination behaviors emerge that you didn't program
- how long it takes to converge (or whether it converges at all)
- whether the emergent result is better or worse than what a single agent would produce
- what happens when one agent is confidently wrong — does the swarm correct it or amplify it?

that last one is the cascading hallucination problem. in a supervisor system, you have one point of failure. in a swarm, you have N points of amplification.

---

*← [back to primitives](../README.md)*
