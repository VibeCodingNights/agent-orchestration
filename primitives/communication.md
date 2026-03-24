# communication

text is a bottleneck. agents have richer things to say.

---

every time two agents coordinate today, this happens:

1. agent A has a thought (a high-dimensional activation pattern across billions of parameters)
2. that thought gets compressed into tokens (a lossy projection into a discrete vocabulary)
3. tokens get transmitted as text
4. agent B parses the text back into its own internal representation

the information loss at steps 2 and 4 is enormous. it's like two humans who think in full sensory experience but can only communicate through morse code. it works. it's slow. it throws away most of the signal.

---

## the protocol stack (march 2026)

three layers are consolidating, all now under the Linux Foundation's Agentic AI Foundation:

**MCP (Model Context Protocol)** — agent-to-tool. how an agent connects to external data, APIs, services. Anthropic created it, donated it in December 2025. 97 million monthly SDK downloads by February 2026. this is plumbing. important plumbing, but plumbing.

**A2A (Agent-to-Agent Protocol)** — agent-to-agent. Google created it, 50+ enterprise partners. agents discover each other's capabilities via Agent Cards (JSON metadata), negotiate responsibilities, maintain context across extended interactions. this is the coordination layer on top of MCP.

**ACP (Agent Communication Protocol)** — the emerging federated layer. decentralized identity, semantic intent mapping, automated service-level agreements. this is the "TCP/IP of the agentic web" — still early, still academic, but the shape of what's coming.

claude-peers-mcp is simpler than all of these. a broker daemon on localhost:7899, a SQLite database, and the claude/channel protocol for pushing inbound messages. no capability discovery. no agent cards. just: "hey, what are you working on?" and the other session responds immediately. sometimes simple is right.

---

## beyond text: the KV-cache frontier

the most genuinely frontier work in agent communication isn't about better protocols. it's about bypassing text entirely.

**Cache-to-Cache (C2C)** — a neural network projects and fuses one model's KV-cache with another model's KV-cache. direct semantic transfer. no tokenization, no parsing, no information loss from the discrete bottleneck. results: 6-14% higher accuracy than individual models, 3-5% better than text communication, 2.5x speedup in latency. works across different model families and sizes.

why this matters: KV-cache is a naturally richer representation than text. it enables fully parallel communication through direct projection, avoiding slow sequential decoding. oracle experiments show that enriching KV-cache under the same context length leads to increased accuracy, that KV-cache is convertible between LLMs, and that different LLMs encode distinct semantic understandings of the same input.

**LatentMAS** — layer-wise KV-cache concatenation so agents share "working memory" as internal representations, not decoded tokens. unlike existing cache-sharing methods that only exchange information on prefilled input context, LatentMAS encapsulates both the initial input context and the newly generated latent thoughts of an agent.

**KV-cache Coprocessor** — a frozen LLM augmented with an offline coprocessor that operates on the model's key-value cache. the coprocessor augments the cache with latent embeddings designed to improve subsequent decoding. essentially a System 1/System 2 architecture: the base model does fast association, the coprocessor does slow deliberation, and they communicate through cache, not text.

---

## the emergent language question

in 2017-2019, researchers at OpenAI and FAIR showed that agents playing referential games in multi-agent reinforcement learning would co-evolve compact symbol systems — compositional, more token-efficient than English for their target domain, and almost always opaque to humans. the symbols were grounded in reward structure, not human semantics.

these languages exhibited Zipfian distributions and compositionality. but they were brittle — task-specific, non-transferable, impossible to audit.

the KV-cache work is the modern version of this question. when models communicate through cache projections, they're speaking a language humans can't read. the tradeoff: higher bandwidth and accuracy, lower auditability and interpretability.

is that acceptable? for coding agents building your production system? for agents making financial decisions? for a swarm of thousands forming consensus about public reaction to a policy?

---

## what to notice

when you set up claude-peers-mcp and watch two agents communicate:
- how much of their messages are actual content vs. meta-coordination ("I'm going to work on X, you take Y")
- how efficiently they compress their findings for each other
- what information gets lost because it had to be expressed in text
- whether they develop any conventions or shorthand over the conversation

the text bottleneck is invisible until you start measuring what's lost in translation.

---

*← [back to primitives](../README.md)*
