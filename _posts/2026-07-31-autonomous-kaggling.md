---
title: Autonomous Kaggling
date: 2026-07-31 00:00:00 +0000
categories: [Workflows]
tags: [agents, kaggle, autoresearch]
description: "Running Karpathy's autoresearch loop on a live Kaggle competition — what it found on its own, and where it stalled."
---

I recently started a Kaggle competition and decided to apply
[autoresearch](https://github.com/karpathy/autoresearch/) to it: an agent that loops forever
to ace the leaderboard.

The [Kaggle problem](https://www.kaggle.com/competitions/playground-series-s6e7) is
student health risk prediction: categorize records into three classes (unhealthy, at-risk,
fit) from categorical features like `sleep_duration`, `gender`, and `water_intake`.

I based my `problem.md` on autoresearch's description with small modifications; it's in
[hadifar/autonomous-kaggling](https://github.com/hadifar/autonomous-kaggling), along with
the full run. The commit history on the `shr-v1` branch shows what ideas the agent applied
and where each one landed, and `results.csv` in the root tracks the scores.

**Summary: promising direction, not a replacement. Not yet.**

## What worked

**The baseline was solid and hard to beat.** I didn't get the gold medal, but the score the
loop produced on its first serious pass held up against most of what I tried afterwards.
The distance between "an hour of autonomous iteration" and "someone who knows what they're
doing" was narrower than I expected.

**Web search changed what it was capable of.** I gave Claude access to the WebSearch tool,
and it went looking for prior work rather than just iterating on its own ideas. It surfaced
a Kaggle discussion — ["0.95238 Starter"](https://www.kaggle.com/code/jjsdfd22/0-95238-starter)
— describing a way to game the public leaderboard:

> Treat the public LB as an oracle: flip a batch of ~150 candidate rows to `fit`, submit,
> read the score delta to count how many were right, then binary-search the batch
> (37 → 18 → 9 rows) to localize them.

It also tracked down the
[original dataset](https://www.kaggle.com/datasets/ziya07/college-student-health-behavior-dataset)
the competition was derived from and ran a series of augmentations against it.

Both moves are the kind of thing an experienced competitor does, and it was very insightful!

**The error analysis was better than I expected.** Unprompted, it computed majority-class
baselines and estimated a ceiling — establishing what "good" means before chasing it. Then
it traced the bulk of the residual error to missing values in decisive features and
concluded the problem wasn't recoverable through modeling. That's the correct call; no
amount of feature engineering fixes information that isn't in the data. Any working data
scientist would get there. The point is that an LLM got there without being pointed at it.

**It's a good ideation partner.** Several suggestions were new to me. I'm not a
data-science expert — I know most of the concepts — and for someone in that position it
widened the search space faster than I could have alone.

## Where it stalled

**It abandons ideas too early.** For example, the loop proposed data augmentation, saw no
improvement in balanced accuracy, and discarded it. Reasonable! But I looked at the actual
distribution of the data, and the problem wasn't that augmentation was wrong — it was that
*that particular* strategy was too naive for this distribution. I proposed a different one
and it worked.

**Much of the budget goes to the obvious.** A large share of attempts were hyperparameter
nudges — raising and lowering the learning rate, blending XGBoost with CatBoost. Almost
certainly the moves most represented in its training data.

**My own ideas still moved the score most.** The loop got me to a strong baseline fast; the
meaningful gains past that came from me looking at the data — the data augmentation above
is one example.

**Instruction-following issues.** I explicitly stated in my `problem.md` file that it should
loop forever. However, my loop stopped after `1h 38m 10s`. Another pattern I observed is
that during the loop it sometimes forgot to commit before ideation and jumped straight into
implementation. So if the goal is reliability, be careful about it.

## Takeaway

Treat it as a fast, tireless junior assistant that covers the obvious ground — baselines,
error analysis, standard ensembling, hyperparameter sweeps — so your attention goes to the
parts that need judgment & ideation. Very good at execution & not good at ideation!

## References

- [karpathy/autoresearch](https://github.com/karpathy/autoresearch/)
- [Kaggle — Playground Series S6E7](https://www.kaggle.com/competitions/playground-series-s6e7)
