# Cortex Eval Lab

A **runnable** version of the Module 3 eval labs, the 3-layer suite, trajectory eval, and judge
calibration, with **no LangSmith / promptfoo / vendor signup**. Just Python and (for the semantic
judge only) your own API key.

You evaluate **Cortex**, the PM chief-of-staff agent from *Run Your AI Agent Team*, so AI Evals and the
agents course share one throughline. The traces are recorded and shipped here, so everything runs
offline except the Layer 3 LLM judge.

## Run it

```bash
pip install -r requirements.txt
cp .env.example .env            # or: cp env.example.txt .env
# paste your OpenAI key (only needed for the Layer 3 judge)
jupyter lab eval_lab.ipynb      # or: jupyter notebook / open in VS Code / Cursor
```

This folder is the **Module 3** lab. It does not run the Module 1 Version A vs Version B comparison.

If your fork does not have this folder, download the files from the content library (same names as below). Use `env.example.txt` if `.env.example` does not open: copy it to `.env`.

Then run the cells top to bottom. **Layers 1 & 2, the trajectory eval, and the Cohen's κ math all run
without any key**, add a key only to run the live judge.

## Files

| File | What it is |
|---|---|
| `eval_lab.ipynb` | The lab: 3-layer suite → trajectory eval → judge calibration (κ). |
| `cortex-traces.json` | Recorded Cortex trajectories (gold + candidates: invented metric, confidential leak, jailbreak refusal, redundant loop, recovery). |
| `calibration-set.json` | 12 hand-labeled grounding examples for the κ calibration. |
| `requirements.txt` / `.env.example` | Deps + where your key goes (stays local; `.env` is gitignored). |
| `env.example.txt` | Same key template as `.env.example`, without a leading dot, so the content library link resolves. |

## Why no vendor console?

The same functions (`layer1_code`, `layer2_safety`, `judge`, `trajectory_eval`) are what you later import
into a **CI gate** (M4) and point at **production traces** (M5). LangSmith / Braintrust / Phoenix are
optional dashboards on top of exactly this logic, you never needed one to do the work.
