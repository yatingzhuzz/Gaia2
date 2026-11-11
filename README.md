# Gaia2 on a budget: reproduction, failure analysis, and a waiting instruction

Yating Zhu · CS329Z Paper Video, Stanford, Fall 2026

Paper: Froger et al., *Gaia2: Benchmarking LLM Agents on Dynamic and Asynchronous Environments*, ICLR 2026 ([arXiv:2602.11964](https://arxiv.org/abs/2602.11964)).
Harness: the released ARE package, `meta-agents-research-environments==1.2.0`.

Video: _link_

## Summary

Gaia2 evaluates agents in a simulated phone whose environment keeps changing while the agent works. The paper reports that every model scores close to zero on Time, the capability that requires acting within a time window. This repository reruns a small part of the benchmark with the released code, looks at why the Time scenarios fail, checks how much the verdicts depend on the judge model, and tests whether a single instruction about waiting changes the result.

## Setup

- Data: Gaia2 validation split, first 10 scenarios of the `execution` and `time` configs
- Agents: ARE default agent with Gemini 2.5 Flash and with Claude Sonnet 4.5, via OpenRouter
- Judge: Llama-3.3-70B, as in the paper; GPT-5-mini for the re-judging comparison
- One run per scenario (the paper averages three)

## Results

| Run | Agent | Scenarios | Passed | Avg. duration |
| --- | --- | --- | --- | --- |
| exec_flash | Gemini 2.5 Flash | 10 Execution | — | — |
| exec_strong | Claude Sonnet 4.5 | 10 Execution | — | — |
| time_flash | Gemini 2.5 Flash | 10 Time | — | — |
| time_strong | Claude Sonnet 4.5 | 10 Time | — | — |
| time_flash_wait | Gemini 2.5 Flash + waiting instruction | 10 Time | — | — |

Judge sensitivity: — of 20 verdicts differ between Llama-3.3-70B and GPT-5-mini.

API cost: —

### Observations

_To be added._

With one run on 10 scenarios per setting, these results are case studies rather than estimates of benchmark scores.

## Reproducing

Open `gaia2.ipynb` in Google Colab, add the Colab Secrets `HF_TOKEN` (Hugging Face, read access) and `OPENROUTER_API_KEY`, and select Runtime → Run all. A full run takes about three hours. LLM outputs are not deterministic, so individual verdicts can differ between runs.

## Repository contents

| Path | Content |
| --- | --- |
| `gaia2.ipynb` | All code, with outputs |
| `results/<run>/hf/`, `results/<run>/lite/` | Full traces and per-scenario verdicts written by ARE |
| `results/<run>/benchmark_stats.json` | ARE's aggregate statistics |
| `results/<run>/command.sh` | The exact command used for the run |
| `results/<run>_rejudge/` | Verdicts from the GPT-5-mini judge |
| `results/single_*/` | Single-scenario runs shown in the video |
| `results/figs/` | Timeline plots and `summary.csv` |
| `slides.pdf` | Slides used in the video |

## Use of AI

Claude was used to explain parts of the paper, to help write and debug the notebook code, and to give feedback on the script and slides. The slides, script, analysis, and narration are my own.
