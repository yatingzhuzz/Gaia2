# Gaia2 on a budget: reproduction, failure analysis, and a waiting-instruction extension

CS329Z Paper Video · Yating Zhu · Stanford, Fall 2026

Paper: Froger et al. *Gaia2: Benchmarking LLM Agents on Dynamic and Asynchronous Environments.* ICLR 2026. [arXiv:2602.11964](https://arxiv.org/abs/2602.11964)
Code used: the released ARE harness, `meta-agents-research-environments==1.2.0`.

Video: <YouTube link>

## What I did

1. **Reproduction.** Ran the released Gaia2 harness on the Gaia2-mini validation split: 10 Execution and 10 Time scenarios, with Gemini 2.5 Flash and Claude Sonnet 4.5 as agents and Llama-3.3-70B as judge (the paper's judge). One run per scenario.
2. **Failure analysis.** Plotted, for each Time scenario, when world events happened against when the agent acted, and read the traces of the failures.
3. **Judge sensitivity.** Re-judged the same traces with GPT-5-mini instead of Llama-3.3-70B, without re-running the agents.
4. **Extension.** Added one paragraph to the default agent's system prompt telling it to wait and poll in time-dependent tasks, and re-ran the 10 Time scenarios.

## Results

| Run | Agent | Scenarios | Passed | Avg. time per scenario |
| --- | --- | --- | --- | --- |
| exec_flash | Gemini 2.5 Flash | 10 Execution | 2 | 45 s |
| exec_strong | Claude Sonnet 4.5 | 10 Execution | 1 | 108 s |
| time_flash | Gemini 2.5 Flash | 10 Time | 0 | 38 s |
| time_strong | Claude Sonnet 4.5 | 10 Time | 0 | 120 s |
| time_flash_wait | Gemini 2.5 Flash + waiting instruction | 10 Time | TODO | TODO |

Re-judging with GPT-5-mini: TODO of 20 verdicts flipped (time_flash and exec_strong).

Total API cost: about $TODO (OpenRouter course credits).

Observations:
- In `wezjyh` (ask a housemate which apartment she prefers, wait up to 4 minutes, then act), both agents sent the message and ended their turn. The scripted replies at +75 s and +120 s were never reached.
- In `xvc7uo` (notify a friend whenever an in-budget listing appears), both agents did wait and picked the right listings, but acted a few seconds after the expected window and were judged Invalid.
- In `2tof7f` (Execution), Claude Sonnet 4.5 first emailed a name instead of an address, then emailed the right address with a longer message than the oracle; the verdict says the oracle email was never sent.
- Small sample, one run each: these are case studies, not a leaderboard.

## How to run

Open `gaia2_colab.ipynb` in Google Colab. Add two Colab Secrets, `HF_TOKEN` (Hugging Face, read access) and `OPENROUTER_API_KEY`, and run the cells top to bottom. Part 1 reproduces the runs, Part 2 the analysis and the extension, Part 3 the single-scenario runs shown in the video. Each run's exact command is saved as `results/<run>/command.sh`.

## Files

- `gaia2_colab.ipynb`: everything, with outputs
- `results/<run>/hf/` and `lite/`: full traces and per-scenario verdicts from ARE
- `results/<run>/benchmark_stats.json`: ARE's official statistics
- `results/<run>_rejudge/`: verdicts from the GPT-5-mini judge
- `results/figs/`: timeline plots and `summary.csv`
- `slides.pdf`: slides used in the video

## Use of AI

I used Claude to explain parts of the paper, to help write and debug the notebook code, and to give feedback on my script and slides. The slides, script, analysis, and narration are my own.
