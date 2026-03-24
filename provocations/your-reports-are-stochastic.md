# Your reports are stochastic

You're already an engineering manager. But your reports hallucinate.

---

Addy Osmani keeps coming back to this: the skills that make someone a strong engineering manager translate directly into effective agent orchestration. Agent teams make this even more explicit.

**Task sizing matters.** Too small and coordination overhead dominates. Too large and teammates work too long without check-ins, risking wasted effort. The sweet spot is self-contained units that produce a clear deliverable. 5-6 tasks per teammate keeps everyone productive.

**File ownership matters.** Two teammates editing the same file leads to overwrites. Break the work so each teammate owns a different set of files. Same boundary-setting you'd do with a human team to avoid merge conflicts.

**Context loading matters.** Teammates get your project's CLAUDE.md, MCP servers, and skills automatically, but they don't inherit the lead's conversation history. You need to frontload context — the same way you'd write a thorough ticket for a new hire.

These are all engineering management skills. You already have them.

---

## Where the analogy breaks

Your human reports don't hallucinate. Your agent reports do.

A junior engineer who doesn't know something says "I don't know." A junior engineer who's confused asks a question. A junior engineer who makes a mistake usually knows they made a mistake.

An agent that doesn't know something confabulates with confidence. An agent that's confused produces plausible-sounding nonsense. An agent that makes a mistake often doubles down when questioned.

This changes everything about management. With humans, you can delegate and trust that they'll flag problems. With agents, you need verification at every handoff. The agent's self-report of its own work quality is unreliable.

---

## The cascade

In a human team, one person's mistake is usually caught by another person. Code review exists because humans are good at spotting things that "look wrong."

In an agent team, one agent's hallucination becomes another agent's input. The second agent treats it as ground truth — it has no reason not to. The third agent builds on both. By the time the supervisor synthesizes, the hallucination is load-bearing.

Research analyzing 200+ execution traces from popular multi-agent frameworks found failure rates of 40-80%. Over a third of failures came from inter-agent misalignment — agents operating on different understandings of the shared state.

This is the cascading hallucination problem, and nobody has a good answer. "Add a reviewer agent" just adds another LLM that can also hallucinate. "Add a fact-checking step" works for verifiable claims but not for design decisions or architectural judgments.

---

## Guardian agents

An emerging concept: an agent that both owns tasks and governs other agents. It senses and manages risky behaviors. It's the tech lead who does code review while also shipping features.

The problem: the guardian agent is also an LLM. It has the same failure modes as the agents it's watching. Who watches the watchmen?

---

## The question for tonight

Every person in this room has managed a team, a project, or at least a group chat. You have intuitions about coordination that are hard-won and real.

Those intuitions mostly transfer to agent orchestration. But the failure modes are different. Your human reports are sometimes lazy, sometimes confused, sometimes wrong. Your agent reports are sometimes *confidently, fluently, persuasively* wrong. And they never flag uncertainty.

What management practices need to change when your team is stochastic?

- Do you need more checkpoints or fewer?
- Is the right response to add more agents (reviewers, validators) or fewer (reduce the coordination surface)?
- How do you build trust in an agent's output when self-assessment is unreliable?
- What does "code review" mean when the reviewer can hallucinate as easily as the author?

These aren't theoretical questions. They're the practical reality of anyone running agent teams in production today.

---

*← [back to provocations](../README.md)*
