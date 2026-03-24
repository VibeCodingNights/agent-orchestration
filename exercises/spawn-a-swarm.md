# Spawn a swarm

MiroShark. Feed it a document, watch hundreds of agents argue their way to consensus.

---

## What it is

MiroShark is a multi-agent simulation engine built on MiroFish/OASIS from CAMEL-AI. You upload a document — press release, policy draft, financial report, even a novel — and it generates hundreds of AI agents with unique personalities, memories, and social connections. They interact on simulated social platforms (Twitter-like, Reddit-like), forming opinions, arguing, persuading, shifting positions. A ReportAgent analyzes what emerged.

This isn't agent orchestration for completing tasks. This is swarm intelligence: emergent behavior from many simple agents following local rules.

## Setup

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

Edit `.env` with your LLM provider:

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

## What to try

**Round 1: Feed it something you care about.** A press release from your company. A controversial policy proposal. A product announcement. Describe your prediction question in natural language. Watch the simulation run.

**Round 2: Observe emergence.** As agents interact, coalitions form. Dominant narratives emerge. Minority positions get amplified or suppressed. None of this was programmed — it grows from agent-to-agent interaction. Watch for:
- Which agents become influential and why
- How opinions cluster and polarize
- What narratives gain traction vs. die out
- Whether the emergent consensus matches your intuition

**Round 3: Interrogate the agents.** After the simulation, you can chat with any individual agent. Ask them *why* they hold their position. Ask what changed their mind. Ask about their interactions. This is the diagnostic tool for understanding emergent behavior.

**Round 4: Inject a variable.** Run the same seed document but change one parameter — add a new piece of information mid-simulation, or modify an agent's initial stance. Watch how the butterfly effect propagates through the swarm.

## What this teaches about orchestration

MiroShark is the extreme case of the swarm pattern. Thousands of agents, no supervisor, emergent outcomes. It reveals:

- **Emergence is real.** Complex behavior comes from simple local rules. You don't need to program the outcome — you need to design the interaction rules.
- **Convergence isn't guaranteed.** Sometimes the swarm polarizes instead of converging. Sometimes a loud minority dominates. These are failure modes of all swarm architectures, not just simulation.
- **Auditability is hard.** You can interrogate individual agents, but explaining *why the swarm as a whole* reached a conclusion requires different tools than explaining why a single agent did.
- **Scale changes the dynamics.** 10 agents behave differently than 100, which behave differently than 1,000. Coordination patterns that work at one scale break at another.

## What to notice

- Did the swarm produce insights you wouldn't have gotten from a single agent?
- How much of the emergent behavior is signal vs. noise?
- Could you have predicted the outcome from the individual agent configurations?
- What happens when one agent is confidently wrong — does the swarm correct it or amplify it?

That last question is the core tension of swarm intelligence: collective wisdom vs. collective hallucination.

---

*← [back to exercises](../README.md)*
