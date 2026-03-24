# Beyond text

What if agents could share thoughts instead of words?

---

Every agent handoff today goes through text. Internal representation → tokens → parse → internal representation. It's the lowest-bandwidth channel available. The model's "thought" exists as a high-dimensional activation pattern — projecting it into tokens throws away enormous amounts of information.

But what if you could skip the text?

---

## Cache-to-Cache (C2C)

A paper from October 2025, updated March 2026. The idea: a neural network projects and fuses one model's KV-cache with another model's KV-cache. No tokenization step. No parsing step. Direct semantic transfer.

The results:
- 6-14% higher average accuracy than individual models
- 3-5% better than text communication between the same models
- 2.5x speedup in latency
- Works across different model families and different model sizes

The mechanism: a learnable gating mechanism selects which target layers benefit from cache communication. A fuser architecture keeps the receiver's original KV-cache and blends it with the sharer's projected cache. The complementary contextual understanding from a heterogeneous sharer is real — identical self-communication also outperforms single models, but cross-model communication performs better.

The implication: text is not just a slow channel. It's a *lossy* channel. Models talking through cache projections communicate richer information than models talking through text, and the difference shows up in task performance.

---

## The coprocessor architecture

A separate line of work: augment a frozen LLM with an offline coprocessor that operates on its KV-cache. The coprocessor adds latent embeddings designed to improve subsequent decoding. Trained using the language modeling loss from the decoder on standard pretraining data, while keeping the decoder frozen.

This is essentially System 1 / System 2 for language models. The base model does fast association (System 1). The coprocessor does slow deliberation (System 2). They communicate through cache, not text. The coprocessor can operate offline and asynchronously — the language model works normally if the coprocessor is unavailable.

Neuroscience evidence points to partially distinct substrates for deliberative control and habitual responses. The coprocessor architecture mirrors this separation.

---

## What this means for orchestration

Current agent orchestration is built on the assumption that text is the communication medium. MCP sends JSON. A2A sends structured messages. claude-peers-mcp sends natural language. All text.

If agents could share KV-caches instead:
- The 30% coordination tax drops (no tokenization/parsing overhead)
- Information loss at handoffs decreases (richer representations)
- Agents could share intermediate reasoning states, not just conclusions
- New failure modes emerge (cache corruption, representation drift, unauditable communication)

The tradeoff is stark: higher bandwidth and accuracy, lower auditability and interpretability. When your agents communicate in a language you can't read, debugging gets harder. Governance gets harder. Trust gets harder.

---

## The question for tonight

Is text the right medium for agent coordination?

For coding agents building your production system — where you need to audit every decision — probably yes. The overhead is worth the transparency.

For a swarm of thousands forming consensus about public reaction to a policy — where emergence is the point — maybe not. The information loss from text might be hiding signal you need.

The frontier isn't "better text protocols." It's "should we use text at all?"

---

*← [back to provocations](../README.md)*
