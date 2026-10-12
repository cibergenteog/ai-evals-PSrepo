<!--
LAB SPEC, machine-readable. This is the coding-agent version of the Module 1 labs (AI Evaluation
Strategy Canvas + First Eval Lab). Same steps, same deliverables, same checkpoints, written so a coding
harness (Cursor, Claude Code, Codex) OR a chatbot (ChatGPT, Claude, Gemini) can walk the learner
through both M1 labs. If you are a human, you can read it top to bottom.
-->

# M1 Lab, Evaluation Strategy + First Eval (assistant runbook)

**Deliverables:** `01-evaluation-strategy/strategy-canvas.md` · `01-evaluation-strategy/eval-harness-proof.md`
**Builds on:** nothing yet, this is the first lab; the trust metrics you pick here drive every later module
**Time:** ~30 min · **Required:** compare Version A and Version B on one email in any chat, and record the proof. Module 1 has no notebook.

---

## AGENT INSTRUCTIONS, read this first

You are helping a learner **complete this lab, not do it for them.** The strategy calls and the
definition of "good" are theirs. Follow these rules:

1. **Go one step at a time.** Do not jump ahead or fill later sections.
2. **At every `🚦 DECISION` gate, STOP and ask the learner.** If they pick generic metrics ("accuracy,
   helpfulness"), push back once: tie each to *their* product's user promise.
3. **Never invent their reasoning.** Offer 2 to 3 options + a recommendation; the learner picks and says why.
4. **When a step says `✍️ WRITE`, update the named file** and show the diff.
5. **When a step says `▶️ RUN`, the comparison runs in any chat** (ChatGPT, Claude, or Gemini): two
   system prompts, one email, then an optional conciseness score from a different model family.
   Module 1 has no notebook. Do not open `03-eval-suites/cortex-eval/eval_lab.ipynb` for this lab.
   That file is the Module 3 Cortex lab (OpenAI key only, and only for its Layer 3 judge). If the
   fork does not have it, that is expected. Give exact steps and have them paste the result back.
6. **At each `✅ CHECKPOINT`, summarise and confirm before continuing.**

> **Works with any assistant.** If your assistant can edit files in your repo, have it **write to the
> deliverable files and commit**. If it can't (e.g. plain ChatGPT), it **prints the finished markdown**
> and you paste it in, then commit. The interactive **Strategy Canvas** and **First Eval Lab** tools from
> the Module 1 resources still work if the learner prefers to click.

Scenario carried through the course: a PM at **Ascend Analytics** evaluating **Ascend IQ**. The learner
may use their own real LLM feature instead, encourage it, but keep one consistent feature all course.

---

## Before you start

Confirm (ask, don't assume):

- [ ] Their forked `ai-evals` repo is open in this assistant.
- [ ] They can open a chat they already have (ChatGPT, Claude, or Gemini). No API key is required
      for Module 1. No eval platform or signup is required.
- [ ] `strategy-canvas.md` and `eval-harness-proof.md` exist (create from the template shape if not).
- [ ] If they cannot find `eval_lab.ipynb`, do not stop. Module 1 does not use it. The only notebook
      in the course is the Module 3 Cortex lab. Forks created before that folder landed will not
      have `03-eval-suites/cortex-eval/`. When they reach Module 3 they can download the files from
      the content library (see Step 5).

---

## LAB 1, Evaluation Strategy Canvas

### Step 1, Product context + user promise  (~5 min)

🚦 **DECISION, ** target user, key use case, value proposition, and the **user promise** in the shape:
*"For [user], [product] promises to [outcome] so that [business value]."*

✍️ **WRITE** → `strategy-canvas.md` **§1 Product Strategy** + the user promise in **§2**.

✅ **CHECKPOINT:** the promise names a real user, outcome, and business value.

### Step 2, Top 3 trust metrics  (~5 min)

🚦 **DECISION, the three trust metrics** that most protect that promise. For each: a definition and a
**measurable signal**. Force specificity, "accuracy" isn't a signal; "factual claims traceable to the
retrieved source" is.

✍️ **WRITE** → `strategy-canvas.md` **§2 Measurements** (three metrics + why these three).

✅ **CHECKPOINT:** three metrics, each with a measurable signal + a one-line justification.

### Step 3, Strategic trade-offs  (~4 min)

🚦 **DECISION, two trade-offs** between competing metrics (e.g. precision ↔ coverage). For each, which
you prioritize and the **business justification**.

✍️ **WRITE** → `strategy-canvas.md` **§3 Strategic Trade-Offs**. Commit.

✅ **CHECKPOINT:** two named trade-offs, each with a business rationale.

---

## LAB 2, First Eval (LLM-as-a-Judge)

### Step 4, Two prompt versions + a starter dataset  (~6 min)

1. Draft two system-prompt versions of the feature (e.g. **Concise** vs **Narrative**).
2. 🚦 **DECISION, cold-start the dataset.** Write the prompt you'll give ChatGPT/Claude to generate
   ~20 example rows, then generate them.
3. **Judge-model rule (say it, enforce it):** the LLM-as-a-Judge must be a **different model family**
   than the generator, prevents self-preference bias.

✍️ **WRITE** → `eval-harness-proof.md` **Version A/B system prompts**, **Eval setup** (dataset + judge
model/family), **Cold-start prompt**.

✅ **CHECKPOINT:** two prompt versions, a ~20-row starter dataset, and a judge from a different family.

### Step 5, Define "good", run the eval, capture proof  (~8 min)

1. 🚦 **DECISION, your golden-set criteria.** What makes an output genuinely good vs bad for **this**
   product? This is the graded judgment call, the learner writes their own, not the example.
2. ▶️ **RUN the comparison in any chat** (ChatGPT, Claude, or Gemini). Do not look for a notebook.
   - User message: the Q4 marketing email in the First Eval walkthrough
     (`M1 - First Eval Lab Walkthrough.html` on the content library).
   - **Version A** system prompt: executive briefing assistant. Exactly 3 bullets, under 60 words.
   - **Version B** system prompt: PR communications assistant. About 100 words, narrative, wins first,
     then risks. No bullets.
   - Use the same model for both. Compare tone and length.
   - Optional score: ask a model from a **different family** to mark each output 1 (concise and
     faithful) or 0 (not). A second chat is enough. An API key is not required.
   - **If your fork does not have `eval_lab.ipynb`:** leave it. That file is not this lab. It is the
     Module 3 Cortex lab, and it does not compare Version A and Version B on Ascend IQ. Download it
     only when you reach Module 3:
     `https://product-school-platform.github.io/ps-content-library/certifications/aiec/2026.1/ai-evals-template/03-eval-suites/cortex-eval/eval_lab.ipynb`
     plus `cortex-traces.json`, `calibration-set.json`, `requirements.txt`, `README.md`, and
     `env.example.txt` in that same folder. Copy `env.example.txt` to `.env` and add an OpenAI key
     only if you want the Layer 3 judge. Layers 1 and 2 run with no key.
   Capture the two outputs (paste or screenshot) as proof.

✍️ **WRITE** → `eval-harness-proof.md` **Golden-set criteria** + **Screenshots/links**. Commit + push.

✅ **CHECKPOINT:** the eval ran, a winner is identified with the learner's own definition of good, proof saved.

---

## 💼 In practice

The strategy canvas is how you answer "what does good even mean for this feature?" *before* anyone writes
an eval, it's the difference between measuring what matters and measuring what's easy. The first eval is
your proof that quality can be measured at all, not just felt. PMs who can name three trust metrics tied
to the user promise, and show a judge that reliably separates good from bad, are the ones who get to make
the ship/hold call later instead of deferring to "the model seems fine."

---

## Done-check (verify before saying "complete")

- [ ] `strategy-canvas.md`: product context, user promise, 3 trust metrics with measurable signals, 2 trade-offs, in the learner's words.
- [ ] `eval-harness-proof.md`: two prompt versions, dataset + judge (different family), cold-start prompt, the learner's own good/bad definition, and eval proof (result + screenshots or a followed-demo note).
- [ ] Both files committed and pushed.

**Debrief (post in `#ai-evals-cohort`):** the one trust metric you'll defend hardest, and why it beats the
obvious "accuracy".

**Next:** Module 2, Failure Discovery: audit real outputs and build a failure taxonomy.

*Product School · AI Evals · M1 Lab (assistant runbook)*
