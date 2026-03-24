# Communication

Text is a bottleneck. Agents have richer things to say.

---

Every time two agents coordinate today, this happens:

1. Agent A has a thought (a high-dimensional activation pattern across billions of parameters)
2. That thought gets compressed into tokens (a lossy projection into a discrete vocabulary)
3. Tokens get transmitted as text
4. Agent B parses the text back into its own internal representation

The information loss at steps 2 and 4 is enormous. It's like two humans who think in full sensory experience but can only communicate through morse code. It works. It's slow. It throws away most of the signal.

---

## The protocol stack (March 2026)

Three layers are consolidating, all now under the Linux Foundation's Agentic AI Foundation:

**MCP (Model Context Protocol)** — Agent-to-tool. How an agent connects to external data, APIs, services. Anthropic created it, donated it in December 2025. 97 million monthly SDK downloads by February 2026. This is plumbing. Important plumbing, but plumbing.

**A2A (Agent-to-Agent Protocol)** — Agent-to-agent. Google created it, 50+ enterprise partners. Agents discover each other's capabilities via Agent Cards (JSON metadata), negotiate responsibilities, maintain context across extended interactions. This is the coordination layer on top of MCP.

**ACP (Agent Communication Protocol)** — The emerging federated layer. Decentralized identity, semantic intent mapping, automated service-level agreements. This is the "TCP/IP of the agentic web" — still early, still academic, but the shape of what's coming.

claude-peers-mcp is simpler than all of these. A broker daemon on localhost:7899, a SQLite database, and the claude/channel protocol for pushing inbound messages. No capability discovery. No Agent Cards. Just: "hey, what are you working on?" and the other session responds immediately. Sometimes simple is right.

---

## Beyond text: the KV-cache frontier

The most genuinely frontier work in agent communication isn't about better protocols. It's about bypassing text entirely.

**Cache-to-Cache (C2C)** — A neural network projects and fuses one model's KV-cache with another model's KV-cache. Direct semantic transfer. No tokenization, no parsing, no information loss from the discrete bottleneck. Results: 6-14% higher accuracy than individual models, 3-5% better than text communication, 2.5x speedup in latency. Works across different model families and sizes.

Why this matters: KV-cache is a naturally richer representation than text. It enables fully parallel communication through direct projection, avoiding slow sequential decoding. Oracle experiments show that enriching KV-cache under the same context length leads to increased accuracy, that KV-cache is convertible between LLMs, and that different LLMs encode distinct semantic understandings of the same input.

**LatentMAS** — Layer-wise KV-cache concatenation so agents share "working memory" as internal representations, not decoded tokens. Unlike existing cache-sharing methods that only exchange information on prefilled input context, LatentMAS encapsulates both the initial input context and the newly generated latent thoughts of an agent.

**KV-cache Coprocessor** — A frozen LLM augmented with an offline coprocessor that operates on the model's key-value cache. The coprocessor augments the cache with latent embeddings designed to improve subsequent decoding. Essentially a System 1/System 2 architecture: the base model does fast association, the coprocessor does slow deliberation, and they communicate through cache, not text.

---

## The emergent language question

In 2017-2019, researchers at OpenAI and FAIR showed that agents playing referential games in multi-agent reinforcement learning would co-evolve compact symbol systems — compositional, more token-efficient than English for their target domain, and almost always opaque to humans. The symbols were grounded in reward structure, not human semantics.

These languages exhibited Zipfian distributions and compositionality. But they were brittle — task-specific, non-transferable, impossible to audit.

The KV-cache work is the modern version of this question. When models communicate through cache projections, they're speaking a language humans can't read. The tradeoff: higher bandwidth and accuracy, lower auditability and interpretability.

Is that acceptable? For coding agents building your production system? For agents making financial decisions? For a swarm of thousands forming consensus about public reaction to a policy?

---

## What to notice

When you set up claude-peers-mcp and watch two agents communicate:
- How much of their messages are actual content vs. meta-coordination ("I'm going to work on X, you take Y")
- How efficiently they compress their findings for each other
- What information gets lost because it had to be expressed in text
- Whether they develop any conventions or shorthand over the conversation

The text bottleneck is invisible until you start measuring what's lost in translation.

---

*← [back to primitives](../README.md)*
