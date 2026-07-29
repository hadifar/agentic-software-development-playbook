---
title: Agentic data science
parent: My Takes
grand_parent: Resources
nav_order: 1
description: "Running Karpathy's autoresearch loop on a live Kaggle competition — what it found on its own, and where it stalled."
layout: minimal
---

# Agentic data science

I recently started a Kaggle competition and decided to apply
[autoresearch](https://github.com/karpathy/autoresearch/) to it. The problem —
[Playground Series S6E7](https://www.kaggle.com/competitions/playground-series-s6e7) — is
student health risk prediction: categorize records into three classes (unhealthy, at-risk,
fit) from categorical features like `sleep_duration`, `gender`, and `water_intake`.

I based my `problem.md` on autoresearch's description with small modifications; it's in
[hadifar/student-health-risk](https://github.com/hadifar/student-health-risk), along with
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

Both moves are the kind of thing an experienced competitor does. 

**The error analysis was better than I expected.** Unprompted, it computed majority-class
baselines and estimated a ceiling — establishing what "good" means before chasing it. Then
it traced the bulk of the residual error to missing values in three decisive fields and
concluded the problem wasn't recoverable through modeling. That's the correct call; no
amount of feature engineering fixes information that isn't in the data. Any working data
scientist would get there. The point is that an LLM got there without being pointed at it.

**It's a good ideation partner.** Several suggestions were new to me. I'm not a
data-science expert — I know most of the concepts but don't work in the field — and for
someone in that position it widened the search space faster than I would have alone.

## Where it stalled

**It abandons ideas too early.** The loop proposed data augmentation, saw no improvement in
balanced accuracy, and discarded it. Reasonable on the evidence. But I looked at the actual
distribution of the data and the problem wasn't that augmentation was wrong — it was that
*that particular* strategy was wrong for this distribution. I proposed a different one and
it worked.

The gap is diagnostic, not generative. The loop proposes and tests well. It's much weaker at
asking *why* something failed, and whether the failure indicts the approach or just the
implementation. It reads a negative result as a verdict on the hypothesis when it's often a
verdict on the execution.

**Much of the budget goes to the obvious.** A large share of attempts were hyperparameter
nudges — raising and lowering the learning rate, blending XGBoost with CatBoost. Almost
certainly the moves most represented in its training data. It explores by frequency rather
than by expected information gain.

**My own ideas still moved the score most.** The loop got me to a strong baseline fast; the
meaningful gains past that came from me looking at the data. That's not a knock — 90
unsupervised minutes to a strong baseline is most of the tedious work. But it replaces the
first ninety minutes of the person, not the person.

## Takeaway

Treat it as a fast, tireless research assistant that covers the obvious ground — baselines,
error analysis, standard ensembling, hyperparameter sweeps — so your attention goes to the
parts that need judgment.

Where it needs you is the moment an idea fails. The loop treats a negative result as a
closed question. Often it isn't: the implementation was wrong, or the data has structure
nobody has looked at yet. That distinction is the human's job, and it's where nearly all of
my value in this run came from.

## References

- [karpathy/autoresearch](https://github.com/karpathy/autoresearch/)
- [Kaggle — Playground Series S6E7](https://www.kaggle.com/competitions/playground-series-s6e7)
- [hadifar/student-health-risk](https://github.com/hadifar/student-health-risk)
