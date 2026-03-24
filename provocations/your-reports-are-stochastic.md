# your reports are stochastic

you're already an engineering manager. but your reports hallucinate.

---

Addy Osmani keeps coming back to this: the skills that make someone a strong engineering manager translate directly into effective agent orchestration. agent teams make this even more explicit.

**task sizing matters.** too small and coordination overhead dominates. too large and teammates work too long without check-ins, risking wasted effort. the sweet spot is self-contained units that produce a clear deliverable. 5-6 tasks per teammate keeps everyone productive.

**file ownership matters.** two teammates editing the same file leads to overwrites. break the work so each teammate owns a different set of files. same boundary-setting you'd do with a human team to avoid merge conflicts.

**context loading matters.** teammates get your project's CLAUDE.md, MCP servers, and skills automatically, but they don't inherit the lead's conversation history. you need to frontload context — the same way you'd write a thorough ticket for a new hire.

these are all engineering management skills. you already have them.

---

## where the analogy breaks

your human reports don't hallucinate. your agent reports do.

a junior engineer who doesn't know something says "I don't know." a junior engineer who's confused asks a question. a junior engineer who makes a mistake usually knows they made a mistake.

an agent that doesn't know something confabulates with confidence. an agent that's confused produces plausible-sounding nonsense. an agent that makes a mistake often doubles down when questioned.

this changes everything about management. with humans, you can delegate and trust that they'll flag problems. with agents, you need verification at every handoff. the agent's self-report of its own work quality is unreliable.

---

## the cascade

in a human team, one person's mistake is usually caught by another person. code review exists because humans are good at spotting things that "look wrong."

in an agent team, one agent's hallucination becomes another agent's input. the second agent treats it as ground truth — it has no reason not to. the third agent builds on both. by the time the supervisor synthesizes, the hallucination is load-bearing.

research analyzing 200+ execution traces from popular multi-agent frameworks found failure rates of 40-80%. over a third of failures came from inter-agent misalignment — agents operating on different understandings of the shared state.

this is the cascading hallucination problem, and nobody has a good answer. "add a reviewer agent" just adds another LLM that can also hallucinate. "add a fact-checking step" works for verifiable claims but not for design decisions or architectural judgments.

---

## guardian agents

an emerging concept: an agent that both owns tasks and governs other agents. it senses and manages risky behaviors. it's the tech lead who does code review while also shipping features.

the problem: the guardian agent is also an LLM. it has the same failure modes as the agents it's watching. who watches the watchmen?

---

## the question for tonight

every person in this room has managed a team, a project, or at least a group chat. you have intuitions about coordination that are hard-won and real.

those intuitions mostly transfer to agent orchestration. but the failure modes are different. your human reports are sometimes lazy, sometimes confused, sometimes wrong. your agent reports are sometimes *confidently, fluently, persuasively* wrong. and they never flag uncertainty.

what management practices need to change when your team is stochastic?

- do you need more checkpoints or fewer?
- is the right response to add more agents (reviewers, validators) or fewer (reduce the coordination surface)?
- how do you build trust in an agent's output when self-assessment is unreliable?
- what does "code review" mean when the reviewer can hallucinate as easily as the author?

these aren't theoretical questions. they're the practical reality of anyone running agent teams in production today.

---

*← [back to provocations](../README.md)*
