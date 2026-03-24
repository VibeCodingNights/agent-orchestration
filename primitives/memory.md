# memory

shared state is a lie.

---

five agents "share context." they all read the same PLAN.md. they all write to the same STATUS.md. they must be coordinated.

except agent A read PLAN.md at 10:01. agent B updated it at 10:02. agent C read the old version at 10:03. agent D read the new version at 10:04. agent A is still working based on the 10:01 snapshot. nobody noticed.

this is the oldest problem in distributed systems. and agent frameworks have barely started thinking about it.

---

## the computer architecture parallel

a March 2026 paper from arxiv frames multi-agent memory explicitly as a computer architecture problem. the observation is sharp: performance and scalability in computer systems are often limited not by compute but by memory hierarchy, bandwidth, and consistency. the same is true for multi-agent LLM systems.

in computer architecture, consistency models specify which updates are visible to a read and in what order concurrent updates may be observed:

**sequential consistency** — all operations appear in a single global order. every agent sees the same sequence of events. simple to reason about, expensive to implement, serializes everything.

**causal consistency** — operations that are causally related appear in order. operations that aren't may be seen differently by different agents. weaker but more scalable.

**eventual consistency** — all agents will *eventually* see the same state, but at any given moment they might disagree. this is what most agent systems actually implement, usually by accident.

agent frameworks don't specify any consistency model. they just say "shared memory" and hope for the best. the result: 40-80% failure rates in multi-agent execution traces, with 36.9% of failures attributed to inter-agent misalignment.

---

## types of agent memory

**private memory** — each agent's own context window, conversation history, and internal reasoning. this is well-understood from single-agent systems.

**shared memory** — a common workspace all agents can read and write. the blackboard architecture from 1980s speech recognition: agents post to a shared workspace, a resolver synthesizes. simple and effective when you have a clear conflict resolution strategy.

**consensus memory** — a unified source of shared knowledge that all agents align on. skill libraries, domain knowledge, common sense. the hard part: maintaining integrity as agents update it. any unauthorized modification can cascade through the entire system.

**episodic memory** — interaction history between specific agent pairs. "last time I asked Agent B for a code review, it missed the race condition." useful for learning which agents to trust, but expensive to maintain.

---

## the missing piece: access protocols

the biggest gap in current research: there's no standard "agent memory access protocol." key questions that nobody has answered:

- can one agent read another's long-term memory?
- is access read-only or read-write?
- what's the unit of access — a document, a chunk, a key-value record, a trace segment?
- when agent A writes to shared memory, when does agent B see it?
- what happens when two agents write conflicting information simultaneously?

Collaborative Memory (a May 2025 paper) proposes two tiers: private fragments visible only to their originating user, and shared fragments selectively accessible. each fragment carries immutable provenance — contributing agents, accessed resources, timestamps — to support retrospective permission checks.

this is the beginning of an answer. but it's still research, not infrastructure.

---

## context rot

even within a single agent, memory degrades. context rot: the progressive degradation of response quality as context accumulates irrelevant, outdated, or contradictory information.

in multi-agent systems, this compounds. agent A's hallucination gets written to shared memory. agent B reads it and treats it as ground truth. agent C builds on agent B's confident assertion. by the time anyone checks, the hallucination is load-bearing.

the four core failure modes:
- **context poisoning** — hallucinations contaminate future reasoning, creating a feedback loop
- **context distraction** — too much information overwhelms decision-making
- **context confusion** — irrelevant information influences responses
- **context clash** — conflicting information within the same context window

according to production data from Manus AI, agents solving complex tasks average 50 tool calls per task with 100:1 input-to-output token ratios. with context tokens costing $0.30-$3.00 per million, inefficient memory management isn't just a quality problem — it's an economics problem.

---

## what to notice

when you run multi-agent exercises tonight, pay attention to:
- what each agent *thinks* the shared state is at any given moment
- whether agents ever contradict each other based on stale reads
- what happens when you deliberately introduce conflicting information
- how (or whether) agents resolve disagreements about what's true

the consistency model your system implements — probably "eventual, by accident" — determines how much you can trust multi-agent output.

---

*← [back to primitives](../README.md)*
