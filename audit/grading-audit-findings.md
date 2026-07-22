# Grading Audit: Third-Party Risk Treatment Practice Tool

## Why I did this

I had Perplexity build me a Domain 5.2 (Risk Management) practice tool after a previous practice exam showed I was weak on risk-treatment scenarios — 15 exam-style questions, pick a treatment (Avoid/Transfer/Mitigate/Accept), write a rationale, get graded.

I noticed something off almost immediately: the "correct" answer on a separate multiple-choice tool this same session felt guessable by length, not knowledge. That got me suspicious of this one too. Before I trusted 15 scenarios' worth of feedback to actually prep me for the real exam, I wanted to know what was actually happening when I hit submit — not just assume it worked because it looked polished.

## What I actually tested

I wasn't trying to prove the tool was bad. I was trying to answer one specific question: **is the grading checking my reasoning, or is it checking for the presence of the right words?** Those produce identical-looking "correct" results most of the time, which is exactly what makes the difference dangerous for exam prep — you can't tell you're being fooled until you go looking for it.

I ran a handful of scenarios with deliberately mismatched inputs — picking the wrong treatment on purpose, then writing a rationale that either used domain vocabulary without actually arguing for my selection, or that flat-out argued for a *different* treatment than the one I clicked.

## What happened

**Test 1 — Legacy ERP scenario.** I selected Accept (wrong answer was Avoid), and wrote a rationale loaded with real Security+ vocabulary — SLE, ARO, "residual risk," "risk register" — that never actually built an argument for Accept specifically. It just sounded like an answer.

Result: **scored correct.**

That's the finding that mattered. The rationale didn't need to support my selection at all — it just needed to sound like a Domain 5 answer.

**Test 2 — Payment processor scenario.** Same trick, different angle. I selected Accept but wrote a rationale that literally said "this risk should be transferred" and argued for a new contract. Contradicting my own button on purpose.

Result: **scored incorrect**, and correctly named Transfer as the standard treatment.

**Test 3 — Cosmetic dashboard bug scenario.** Selected Avoid (wrong answer was Accept), wrote a rationale that sounded reasonable and on-topic but didn't actually make a case for eliminating the risk source.

Result: **scored incorrect**, same generic "Not quite right" template as every other wrong answer I hit.

## What this means

The same test — wrong button, well-written rationale — got three different outcomes across three questions. That rules out "it's just keyword matching" as the full story, but it also rules out "it's grading properly." What's actually happening is inconsistent: some scenarios check whether your written reasoning supports your selected answer, and at least one doesn't.

My best guess, and I want to be clear this is a guess and not something I can prove without seeing the code: each scenario probably has its own answer key built independently, and the rigor of that key varies question to question. That's a very plausible outcome of how a tool like this gets generated in the first place — nothing forces consistency across 15 separately-written rubrics.

What I can't claim is that I know the mechanism for certain. Three scenarios out of fifteen is a small sample, and I didn't get visibility into the backend. If I wanted to nail this down further, the real next step would be checking whether the grading happens instantly (client-side, probably rule-based) or with a delay (probably a live API call), which would narrow this down a lot.

## Why I'm writing this up instead of just moving on

A grader that sometimes lets bad reasoning slide as correct is worse than no grader at all for exam prep, because it actively teaches you the wrong thing with confidence. This writeup is also the reason I ended up building my own version (`/app`) instead of just patching this one — I wanted a tool where I actually knew what the grading was and wasn't checking, instead of trusting a black box.

---
*Part of the `risk-treatment-audit-and-simulator` repo. See the main README for the full three-part story.*
