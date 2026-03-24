# Memory

Shared state is a lie.

---

Five agents "share context." They all read the same PLAN.md. They all write to the same STATUS.md. They must be coordinated.

Except agent A read PLAN.md at 10:01. Agent B updated it at 10:02. Agent C read the old version at 10:03. Agent D read the new version at 10:04. Agent A is still working based on the 10:01 snapshot. Nobody noticed.

This is the oldest problem in distributed systems. And agent frameworks have barely started thinking about it.

---

## The computer architecture parallel

A March 2026 paper from arXiv frames multi-agent memory explicitly as a computer architecture problem. The observation is sharp: performance and scalability in computer systems are often limited not by compute but by memory hierarchy, bandwidth, and consistency. The same is true for multi-agent LLM systems.

In computer architecture, consistency models specify which updates are visible to a read and in what order concurrent updates may be observed:

**Sequential consistency** — all operations appear in a single global order. Every agent sees the same sequence of events. Simple to reason about, expensive to implement, serializes everything.

**Causal consistency** — operations that are causally related appear in order. Operations that aren't may be seen differently by different agents. Weaker but more scalable.

**Eventual consistency** — all agents will *eventually* see the same state, but at any given moment they might disagree. This is what most agent systems actually implement, usually by accident.

Agent frameworks don't specify any consistency model. They just say "shared memory" and hope for the best. The result: 40-80% failure rates in multi-agent execution traces, with 36.9% of failures attributed to inter-agent misalignment.

---

## Types of agent memory

**Private memory** — each agent's own context window, conversation history, and internal reasoning. This is well-understood from single-agent systems.

**Shared memory** — a common workspace all agents can read and write. The blackboard architecture from 1980s speech recognition: agents post to a shared workspace, a resolver synthesizes. Simple and effective when you have a clear conflict resolution strategy.

**Consensus memory** — a unified source of shared knowledge that all agents align on. Skill libraries, domain knowledge, common sense. The hard part: maintaining integrity as agents update it. Any unauthorized modification can cascade through the entire system.

**Episodic memory** — interaction history between specific agent pairs. "Last time I asked Agent B for a code review, it missed the race condition." Useful for learning which agents to trust, but expensive to maintain.

---

## The missing piece: access protocols

The biggest gap in current research: there's no standard "agent memory access protocol." Key questions that nobody has answered:

- Can one agent read another's long-term memory?
- Is access read-only or read-write?
- What's the unit of access — a document, a chunk, a key-value record, a trace segment?
- When agent A writes to shared memory, when does agent B see it?
- What happens when two agents write conflicting information simultaneously?

Collaborative Memory (a May 2025 paper) proposes two tiers: private fragments visible only to their originating user, and shared fragments selectively accessible. Each fragment carries immutable provenance — contributing agents, accessed resources, timestamps — to support retrospective permission checks.

This is the beginning of an answer. But it's still research, not infrastructure.

---

## Context rot

Even within a single agent, memory degrades. Context rot: the progressive degradation of response quality as context accumulates irrelevant, outdated, or contradictory information.

In multi-agent systems, this compounds. Agent A's hallucination gets written to shared memory. Agent B reads it and treats it as ground truth. Agent C builds on agent B's confident assertion. By the time anyone checks, the hallucination is load-bearing.

The four core failure modes:
- **Context poisoning** — hallucinations contaminate future reasoning, creating a feedback loop
- **Context distraction** — too much information overwhelms decision-making
- **Context confusion** — irrelevant information influences responses
- **Context clash** — conflicting information within the same context window

According to production data from Manus AI, agents solving complex tasks average 50 tool calls per task with 100:1 input-to-output token ratios. With context tokens costing $0.30-$3.00 per million, inefficient memory management isn't just a quality problem — it's an economics problem.

---

## What to notice

When you run multi-agent exercises tonight, pay attention to:
- What each agent *thinks* the shared state is at any given moment
- Whether agents ever contradict each other based on stale reads
- What happens when you deliberately introduce conflicting information
- How (or whether) agents resolve disagreements about what's true

The consistency model your system implements — probably "eventual, by accident" — determines how much you can trust multi-agent output.

---

*← [back to primitives](../README.md)*
