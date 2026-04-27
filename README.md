# llm-reason-council
A structured epistemic audit skill for Claude that evaluates whether a claim, AI output, or assumption is grounded in reason or at risk of hallucination and confabulation.
Built on the same architecture as the LLM Council (Karpathy/Lehmann), with all five components adapted for epistemic evaluation: Verbalized Sampling on five parallel investigators, criteria-based peer review, four research-backed Phase 2 lenses, and a chairman synthesis producing a constrained Reason Verdict.
What it does
Takes any claim — a number an AI gave you, a causal assumption underlying a plan, an institutional fact you are about to use in a professional context — and runs it through five epistemic investigators, each using Verbalized Sampling to surface the confabulation signals that mode-collapsed first-instinct analysis suppresses. The Phase 2 lenses apply Semantic Entropy testing, Chain-of-Verification, calibration auditing, and source grounding. The Epistemic Chairman synthesizes everything into a verdict with a risk level, a warranted confidence range, and one of four actionable recommendations: PROCEED / VERIFY BEFORE USING / DISCARD AND RECONSTRUCT / FLAG AS UNVERIFIABLE.
Built on

Farquhar, Kossen, Kuhn, Gal. "Detecting Hallucinations in Large Language Models Using Semantic Entropy." Nature, 2024.
Dhuliawala et al. "Chain-of-Verification Reduces Hallucination in Large Language Models." ACL Findings, 2024.
KalshiBench. "Do Large Language Models Know What They Don't Know?" 2025.
Zhang et al. "Verbalized Sampling." Stanford, 2024.

Trigger phrases
reason-council this · is this grounded · fact-check this · epistemic audit · sanity-check this · is this real or AI
Relationship to the LLM Council
This is not a replacement. The LLM Council helps you decide. The Reason Council evaluates whether the claims your decision depends on are true. Run them in sequence: council first, then reason-council on the claims the verdict rests on.
