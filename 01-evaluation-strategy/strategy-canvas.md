# AI Evaluation Strategy Canvas

> Repo file `ai-evals/01-evaluation-strategy/strategy-canvas.md` (the repo is your submission).
> Becomes the Strategy Canvas slide of the final pitch deck you assemble in Module 6.

## 1. Product Strategy, The Context

**Target user:** VP-level Strategists and Product Leaders at Fortune 500 companies who pay a premium for verified market intelligence.

**Key use case:** Rapidly extracting specific, verified insights (e.g., comparing competitor pricing models or summarizing G2 reviews) without manual data digging.

**Value proposition:** Personalized, instant answers based on verified data, dramatically reducing the time spent finding and synthesizing information for high-stakes decisions and strategic roadmaps.

## 2. Measurements, The Execution

**User promise.** For VP-level Enterprise Strategists, Ascend IQ promises to deliver verified, citation-backed competitive intelligence in under 5 seconds so that they hit their Q4 roadmap milestones without manual data digging.

**Top 3 trust metrics:**
- **Latency**, Response speed (P95 / P99). Slow kills engagement.
- **Hallucination Rate**, % of numbers or claims in an answer that cannot be traced to the cited source.
- **UX Trust**, Consistency: asking the same question twice returns the same key facts and numbers (% of repeated queries with matching key facts).

**Why these three:**
- Hallucination, VP-level clients pay $50k+ for verified data; one fabricated stat ends the contract.
- Latency, <5s response is the only thing that beats manual digging.
- UX Trust, users only rely on answers that don't change when they ask again; 80% of users who start using it come back to it as their first action.

## 3. Strategic Trade-Offs, The Cost

### Trade-off 1 · Hallucination Rate ↔ Latency

We prioritize Hallucination Rate over Latency because Ascend IQ serves VPs making $1M+ strategic decisions; a single fabricated competitor stat ends a $50k contract, whereas a 3-second wait is the cost of doing business at Enterprise-grade integrity.

### Trade-off 2 · Latency ↔ UX Trust

We prioritize UX Trust over Latency, because Ascend IQ provides information about data and trends that the business relies on to make decisions. We can improve Latency as we optimize models to make them faster, but reliability and accuracy will be the most valuable for a user. Also, the user was used to NO ONE answering questions about their data quickly and now has a solution to that problem. The user will accept a few extra seconds beyond the 5-second target to get an answer that is precise rather than fast.
