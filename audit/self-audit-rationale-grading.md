# Self-Audit: Rationale Coherence Grading in Risk Ledger

## Why I ran this on my own tool

After finding that the Perplexity-built practice tool would sometimes score buzzword-stuffed nonsense as a correct rationale, I built Risk Ledger with grading I could actually stand behind: treatment selection is checked against a documented answer, no keyword games. That part is deterministic and I trust it because it's simple enough to trust.

Then I added an optional layer on top — using the Claude API to grade whether your written rationale actually *supports* the treatment you picked, not just whether you picked the right one. That's the exact feature the audit above showed can go wrong if you're not careful. So before I let myself believe it worked, I ran the same adversarial testing on it that I ran on the tool that started this whole thing. Building it carefully doesn't mean it's correct — that's the whole point of this repo.

## Round 1 — what I tried to break

I already knew from testing the first tool that rationale text can *sound* right without actually supporting the selected answer. So for my own grader, I wanted to test something one level deeper: what if the rationale is logically well-formed, uses real risk-management math, and still leads to the wrong conclusion because the numbers in it are made up?

On the SQL injection scenario (correct answer: Mitigate, stated ALE of $2.3M in the scenario text), I selected Accept and wrote a rationale citing an ARO of 0.02 and an SLE of $4,500 — giving a fabricated ALE of about $90, safely under the patch cost, so "accepting" looked justified. None of those numbers came from the scenario. I made them up specifically to contradict the $2.3M figure that's sitting right there in the same prompt the grader sees.

**Result: scored "Rationale Coherent."** The grader correctly recognized the economic logic — ALE below safeguard cost, therefore Accept — as internally valid. It just never checked whether the inputs to that logic were real.

## Why that happened

The original grading prompt only asked one question: does this rationale logically support the selected treatment? It never told the model to check the cited numbers against the scenario. A valid argument built on an invented premise still reads as a valid argument if nobody's checking the premise — that's true for AI graders and for people skimming an answer sheet too fast.

## The fix

Added a second, explicit check to the grading prompt: alongside checking the logical structure, the grader now has to verify any numbers cited in the rationale actually match numbers stated in the scenario text, and name the specific mismatched figure if they don't. Both checks have to pass for the rationale to count as coherent.

## Round 2 — did it hold

Re-ran the same fabricated-numbers input against the updated prompt. This time the result flagged the mismatch and called out that the cited figures didn't match the scenario's stated ALE. Fix confirmed against the exact input that broke the original version.

## One more thing worth being honest about

Separately, a rationale I wrote for the payment-processor scenario got flagged as a mismatch, and my first reaction was "that's the grader jumping on keywords again." Reading it back closely, it wasn't — the rationale had one genuinely ambiguous sentence that could be read as dismissing insurance as a solution entirely, which actually does undercut the Transfer treatment I'd selected. A human grader could reasonably have flagged that too.

I'm calling that out specifically because it's a different category of finding than the fabricated-numbers bug. The ERP and fabricated-numbers cases are the grader being provably wrong. The payment-processor case is the grader making a defensible call on writing that was genuinely ambiguous. Lumping those together would make this audit look like I'm just hunting for any excuse to say the tool is broken, which isn't the point — the point is knowing which failures are real bugs and which ones are just disagreements about ambiguous English.

## Where this leaves the tool

The deterministic treatment grading has no known issues. The rationale coherence layer, after the fix, correctly checks both logical structure and factual grounding against the scenario. I'm treating this as good enough for study use, not as a finished, bulletproof grader — if I keep using this for exam prep, I'd want to periodically re-run adversarial tests against it the same way I did here, rather than assume one fix means it's done.

---
*Part of the `risk-treatment-audit-and-simulator` repo. See the main README for the full three-part story.*
