# beyond text

what if agents could share thoughts instead of words?

---

every agent handoff today goes through text. internal representation → tokens → parse → internal representation. it's the lowest-bandwidth channel available. the model's "thought" exists as a high-dimensional activation pattern — projecting it into tokens throws away enormous amounts of information.

but what if you could skip the text?

---

## cache-to-cache (C2C)

a paper from october 2025, updated march 2026. the idea: a neural network projects and fuses one model's KV-cache with another model's KV-cache. no tokenization step. no parsing step. direct semantic transfer.

the results:
- 6-14% higher average accuracy than individual models
- 3-5% better than text communication between the same models
- 2.5x speedup in latency
- works across different model families and different model sizes

the mechanism: a learnable gating mechanism selects which target layers benefit from cache communication. a fuser architecture keeps the receiver's original KV-cache and blends it with the sharer's projected cache. the complementary contextual understanding from a heterogeneous sharer is real — identical self-communication also outperforms single models, but cross-model communication performs better.

the implication: text is not just a slow channel. it's a *lossy* channel. models talking through cache projections communicate richer information than models talking through text, and the difference shows up in task performance.

---

## the coprocessor architecture

a separate line of work: augment a frozen LLM with an offline coprocessor that operates on its KV-cache. the coprocessor adds latent embeddings designed to improve subsequent decoding. trained using the language modeling loss from the decoder on standard pretraining data, while keeping the decoder frozen.

this is essentially system 1 / system 2 for language models. the base model does fast association (system 1). the coprocessor does slow deliberation (system 2). they communicate through cache, not text. the coprocessor can operate offline and asynchronously — the language model works normally if the coprocessor is unavailable.

neuroscience evidence points to partially distinct substrates for deliberative control and habitual responses. the coprocessor architecture mirrors this separation.

---

## what this means for orchestration

current agent orchestration is built on the assumption that text is the communication medium. MCP sends JSON. A2A sends structured messages. claude-peers-mcp sends natural language. all text.

if agents could share KV-caches instead:
- the 30% coordination tax drops (no tokenization/parsing overhead)
- information loss at handoffs decreases (richer representations)
- agents could share intermediate reasoning states, not just conclusions
- new failure modes emerge (cache corruption, representation drift, unauditable communication)

the tradeoff is stark: higher bandwidth and accuracy, lower auditability and interpretability. when your agents communicate in a language you can't read, debugging gets harder. governance gets harder. trust gets harder.

---

## the question for tonight

is text the right medium for agent coordination?

for coding agents building your production system — where you need to audit every decision — probably yes. the overhead is worth the transparency.

for a swarm of thousands forming consensus about public reaction to a policy — where emergence is the point — maybe not. the information loss from text might be hiding signal you need.

the frontier isn't "better text protocols." it's "should we use text at all?"

---

*← [back to provocations](../README.md)*
