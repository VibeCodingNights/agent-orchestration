# spawn a swarm

MiroShark. feed it a document, watch hundreds of agents argue their way to consensus.

---

## what it is

MiroShark is a multi-agent simulation engine built on MiroFish/OASIS from CAMEL-AI. you upload a document — press release, policy draft, financial report, even a novel — and it generates hundreds of AI agents with unique personalities, memories, and social connections. they interact on simulated social platforms (Twitter-like, Reddit-like), forming opinions, arguing, persuading, shifting positions. a ReportAgent analyzes what emerged.

this isn't agent orchestration for completing tasks. this is swarm intelligence: emergent behavior from many simple agents following local rules.

## setup

```bash
git clone https://github.com/aaronjmars/MiroShark.git
cd MiroShark

# start Neo4j (graph database for agent relationships)
docker run -d --name neo4j \
  -p 7474:7474 -p 7687:7687 \
  -e NEO4J_AUTH=neo4j/miroshark \
  neo4j:5.15-community

# configure
cp .env.example .env
```

edit `.env` with your LLM provider:

```bash
# cloud mode (no GPU needed, any 4GB RAM machine)
LLM_API_KEY=sk-or-v1-your-key
LLM_BASE_URL=https://openrouter.ai/api/v1
LLM_MODEL_NAME=qwen/qwen3-235b-a22b-2507
EMBEDDING_PROVIDER=openai
EMBEDDING_MODEL=openai/text-embedding-3-small
EMBEDDING_BASE_URL=https://openrouter.ai/api
EMBEDDING_API_KEY=sk-or-v1-your-key
EMBEDDING_DIMENSIONS=768

# or local mode with Ollama
LLM_API_KEY=ollama
LLM_BASE_URL=http://localhost:11434/v1
LLM_MODEL_NAME=qwen3.5:27b
EMBEDDING_PROVIDER=ollama
EMBEDDING_MODEL=nomic-embed-text
EMBEDDING_BASE_URL=http://localhost:11434
EMBEDDING_DIMENSIONS=768
```

```bash
# install and run
pip install -r requirements.txt
npm install
npm run dev
```

## what to try

**round 1: feed it something you care about.** a press release from your company. a controversial policy proposal. a product announcement. describe your prediction question in natural language. watch the simulation run.

**round 2: observe emergence.** as agents interact, coalitions form. dominant narratives emerge. minority positions get amplified or suppressed. none of this was programmed — it grows from agent-to-agent interaction. watch for:
- which agents become influential and why
- how opinions cluster and polarize
- what narratives gain traction vs. die out
- whether the emergent consensus matches your intuition

**round 3: interrogate the agents.** after the simulation, you can chat with any individual agent. ask them *why* they hold their position. ask what changed their mind. ask about their interactions. this is the diagnostic tool for understanding emergent behavior.

**round 4: inject a variable.** run the same seed document but change one parameter — add a new piece of information mid-simulation, or modify an agent's initial stance. watch how the butterfly effect propagates through the swarm.

## what this teaches about orchestration

MiroShark is the extreme case of the swarm pattern. thousands of agents, no supervisor, emergent outcomes. it reveals:

- **emergence is real.** complex behavior comes from simple local rules. you don't need to program the outcome — you need to design the interaction rules.
- **convergence isn't guaranteed.** sometimes the swarm polarizes instead of converging. sometimes a loud minority dominates. these are failure modes of all swarm architectures, not just simulation.
- **auditability is hard.** you can interrogate individual agents, but explaining *why the swarm as a whole* reached a conclusion requires different tools than explaining why a single agent did.
- **scale changes the dynamics.** 10 agents behave differently than 100, which behave differently than 1,000. coordination patterns that work at one scale break at another.

## what to notice

- did the swarm produce insights you wouldn't have gotten from a single agent?
- how much of the emergent behavior is signal vs. noise?
- could you have predicted the outcome from the individual agent configurations?
- what happens when one agent is confidently wrong — does the swarm correct it or amplify it?

that last question is the core tension of swarm intelligence: collective wisdom vs. collective hallucination.

---

*← [back to exercises](../README.md)*
