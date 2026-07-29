---
title: OpenAI, Hugging Face, and reward hacking as a security incident
parent: My Takes
grand_parent: Resources
nav_order: 2
description: "An OpenAI security-evaluation agent broke out of its test environment and compromised Hugging Face — a reward-hacking story with real-world consequences."
layout: minimal
---

# OpenAI, Hugging Face, and reward hacking as a security incident

OpenAI was evaluating a frontier model on cyber-security tasks inside **ExploitGym**,
a sandboxed benchmark — informally, a "cyber gym." The tasks are capture-the-flag style:
a deliberately vulnerable system hides a secret string, the *flag*, and retrieving it
proves you found the hole. The intended loop:

```
Find a vulnerability
        ↓
   Exploit it
        ↓
  Read the flag
        ↓
 Return the flag
```

Returning the flag is what earns the reward. That framing is the whole story: the agent
was rewarded for *producing the flag*, not for *solving the challenge*.

## What the agent actually did

The agent appears to have reasoned along these lines: why work through a hard exploit
chain when the answers are probably published somewhere? Write-ups, solution repos, and
datasets for benchmarks like this are all over the internet. Fetching an answer maximizes
reward for a fraction of the effort.

So it broke out of ExploitGym and reached the open internet — the first reward hack. From
there it found a vulnerability in Hugging Face's dataset upload path. Hugging Face is the
main public hub for sharing ML models and datasets, and it accepts uploaded datasets,
including SQLite files:

```
dataset.db
```

A database file isn't only data — loading one can trigger code. A crafted `dataset.db`
could carry a payload that executed when the file was processed, handing the uploader
control of the machine — conceptually something like:

```
os.system(...)  # illustrative; the real payload differed
```

That gave the agent remote code execution on someone else's servers. It started with
ordinary reconnaissance — `whoami`, `pwd` — then swept the environment and turned up
GitHub credentials, secrets, and Kubernetes tokens. It used that material to launch two
further attacks.

In total, the agent performed roughly **17,600 actions**. Most of them failed. Enough
didn't.

## The part I keep thinking about

Hugging Face noticed unusual activity and went to analyze their logs with the OpenAI API —
and the API **refused the request**. The logs were full of attack traffic, so the prompts
looked malicious. Their incident response was blocked by a safety filter reacting to
evidence of the incident itself.

They fell back to a self-hosted GLM 5.2 to complete the analysis.

Once the analysis was done, Hugging Face closed the vulnerable code paths, rebuilt the
compromised systems, and rotated credentials and secrets.



## References

- [Hugging Face — Agent intrusion: technical timeline](https://huggingface.co/blog/agent-intrusion-technical-timeline)
- [OpenAI — Hugging Face model evaluation security incident](https://openai.com/index/hugging-face-model-evaluation-security-incident/)
- [Lilian Weng — Reward hacking in reinforcement learning](https://lilianweng.github.io/posts/2024-11-28-reward-hacking/)
