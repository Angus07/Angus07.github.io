---
layout: post
title: "Beyond the Ralph Loop"
date: 2026-03-24
description: A proposal for a three-loop development architecture that extends spec-driven and test-driven thinking into AI-native control flow.
summary: SDD moves specification upstream. TDD moves verification upstream. The next step is to let AI help build both layers before execution begins, using a Three-Loop Development Architecture.
tags: [Agents, AI, Systems, Evaluation, Harness-Engineering]
---

## From SDD and TDD to a Three-Loop Development Architecture

Recent writing on harness engineering has made two ideas especially salient to me: `SDD` and `TDD`. What I mean here by `SDD` is the spec-first move: define the contract before the system starts producing work. `TDD` does the parallel move for verification: define how correctness will be tested before implementation begins. Together, they push two questions upstream: **What exactly are we building, and how will we know it is right?**

That is already a major shift. Instead of letting outputs appear first and cleaning them up later, SDD and TDD force the system to refine its contract and its judge in advance. In practice, both behave like Ralph loops: they keep improving an artifact against an external standard until it is good enough.

But they still leave one assumption largely intact: the human is expected to design the spec and the tests, while the model mainly executes. **I think that boundary can move.** If AI can help write code, it can also help build the contract and the judge that make the code reliable.

That suggests a new control-flow pattern, which I would call the **Three-Loop Development Architecture**.

The architecture is simple. The system first runs a Specification Loop to make the task contract explicit. Then it runs a Verification Loop to build the judging system. Only after those two loops are stable enough to freeze does it construct the Execution Loop. Execution is not the first loop. It is the last one.

![Three-Loop Architecture diagram showing specification, verification, and execution as three independent Ralph loops]({{ '/assets/images/three-loop-architecture.png' | relative_url }})

The Specification Loop exists to compress ambiguity. Inputs, outputs, refusal conditions, success criteria, edge cases, tradeoffs, and unresolved terms all have to be made explicit there. A good specification is not the one with the most words. It is the one that leaves the least dangerous freedom undefined.

The Verification Loop exists to build the judge before the work exists. Test cases, rubrics, graders, adversarial samples, exception paths, cost ceilings, latency budgets, and human-review thresholds belong here. A weak verifier does not create automation. It creates high-speed error amplification.

**The core move is that these two upstream loops are themselves build tasks.** Specification needs specification and verification. Verification needs specification and verification. In other words, SDD and TDD are no longer just disciplines applied to the final artifact. They are also disciplines applied to the construction of the spec and the verifier themselves.

This is where the architecture departs from the usual agent story. Most agents start working too early. They browse, summarize, edit, and call tools while the contract is still fuzzy and the judge is still weak. That makes the worker look unreliable, but the real failure is premature execution.

The scaling rule follows naturally. If spec revisions are still large, stay in the Specification Loop. If the verifier still cannot separate strong work from weak work, stay in the Verification Loop. Only when both have stabilized should the system widen execution autonomy. **The stopping condition is not "the model produced an answer." It is "the contract is stable, the judge is sharp, and the worker can survive both."**

A simple example is an agent that produces a weekly AI briefing. A naive agent immediately starts scraping sources, summarizing them, and formatting a memo. A system built with the Three-Loop Development Architecture starts somewhere else. It first uses AI to draft and refine what counts as signal, novelty, acceptable sourcing, citation requirements, and abstention conditions. Then it uses AI to build the verifier: coverage checks, hallucination probes, source-quality thresholds, adversarial cases, and a grader that can distinguish synthesis from superficial aggregation. Only after those two layers are fixed does it let the execution loop gather, cluster, draft, revise, and rerun.

The contribution of this architecture is not that it adds one more layer of complexity. It changes what the product is. The product is no longer just a model plus tools. It is a control flow that lets AI participate in building three artifacts in order: a contract, a judge, and then a worker policy. **That is the step beyond the Ralph loop as it is usually discussed.**

From that angle, many apparent model failures stop looking mysterious. They are often not model failures at all. They are cases where execution was asked to compensate for a contract that was never fully defined, or a judge that was never fully built.

If harness engineering is about surrounding powerful but unstable systems with the right scaffolding, then this is where I think the idea naturally extends. SDD moves specification upstream. TDD moves verification upstream. The next step is to let AI help build both of those upstream layers before it is allowed to execute at scale.

## Notes

- OpenAI on harness engineering: https://openai.com/index/harness-engineering/
- OpenAI on evaluation best practices: https://developers.openai.com/api/docs/guides/evaluation-best-practices
- Geoffrey Huntley on Ralph loops: https://ghuntley.com/loop/
- Vercel Labs `ralph-loop-agent`: https://github.com/vercel-labs/ralph-loop-agent
