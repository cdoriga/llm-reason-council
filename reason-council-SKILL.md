---

name: reason-council

description: "Run any claim, output, assumption, or belief through an epistemic tribunal to evaluate whether it is grounded or at risk of AI hallucination and confabulation. Built on the same architecture as the llm-council (Verbalized Sampling, criteria-based peer review, Phase 2 lenses, chairman synthesis) with all five components adapted for epistemic evaluation rather than decision-making. MANDATORY TRIGGERS: 'reason-council this', 'is this hallucination', 'is this grounded', 'fact-check this', 'epistemic audit', 'verify this claim', 'sanity-check this', 'is this real or AI'. STRONG TRIGGERS: 'am I making this up', 'is this actually true', 'how confident should I be', 'does this hold up', 'is the AI bullshitting me', 'is this defensible'. DO NOT use for decisions between options (that is the llm-council). Use specifically to evaluate whether a claim, output, or assumption has epistemic grounding or is likely confabulation."

---


# Reason Council

The llm-council helps you decide. The reason council helps you know whether what you are deciding on is real.

It runs the same architecture as the llm-council: five parallel agents with Verbalized Sampling, criteria-based peer review, a structured Phase 2 analytical pass, and a chairman synthesis producing a constrained verdict. Every component is adapted for a different question: not "what should I do?" but "is this true?"

**The research this is built on:**

**Semantic Entropy** (Farquhar, Kossen, Kuhn, Gal — Nature, 2024): Confabulations are detectable by measuring uncertainty at the level of meaning rather than word choice. If the same claim, asked multiple ways, produces semantically inconsistent answers, the model does not have a stable underlying fact — it is pattern-matching to plausible text. This is implemented in Phase 2.

**Chain-of-Verification (CoVe)** (Dhuliawala et al. — ACL Findings, 2024): Draft a claim, generate specific verification questions, answer them independently without access to the original claim, check for contradictions. The independence of the verification step from the original claim is the mechanism. Reduces factual hallucinations by 50 to 70% on benchmarks. Implemented in Phase 2.

**Epistemic overconfidence** (KalshiBench, 2025): All frontier LLMs are systematically overconfident. Extended reasoning can worsen calibration rather than improve it. The model's expressed confidence is not a reliable signal of correctness. This informs the Calibration lens and the Reason Verdict format.

**Verbalized Sampling** (Zhang et al., Stanford, 2024): Explicitly sampling from the tail of the model's distribution produces 1.6 to 2.1x more diverse outputs. Applied to epistemic investigation, this surfaces confabulation signals that mode-collapsed first-instinct responses would suppress.

---

## when to run the reason council

The reason council evaluates claims, not decisions.

**Bring it:**
- A claim an AI made that you want validated ("Slow Food has X members in Y country")
- An assumption underlying a plan you are developing
- A causal chain ("X causes Y because Z")
- A strategic framing you want pressure-tested for factual grounding
- An AI-generated output you are about to use in a professional context
- A belief you hold and want to stress-test epistemically

**Do not bring it:**
- Decisions between options (use the llm-council)
- Value judgments or normative claims with no fact-of-the-matter
- Creative or hypothetical questions

---

## the five investigators

These are derived from the same five thinking-style tensions in the llm-council, adapted into epistemic roles. The productive tensions are preserved: Falsifier vs. Alternative Generator (attack the claim vs. multiply explanations). Claim Decomposer vs. Implication Tester (strip the claim to its core vs. trace its downstream consequences). The Naive Verifier keeps everyone honest by reading only what is actually on the page.

**The Falsifier** (derived from the Contrarian): Attempts to disprove the claim using logical analysis, known counterexamples, or any knowledge that contradicts it. Does not look for nuance. Tries to break the claim. If it cannot be broken, that is informative. If it breaks easily, that is more informative.

**The Claim Decomposer** (derived from the First Principles Thinker): Strips the claim to its irreducible commitments. What is actually being asserted? Many claims contain bundled sub-claims, hidden assumptions, or vague quantifiers that make them unfalsifiable by design. The Decomposer names exactly what would need to be true and in what form.

**The Alternative Generator** (derived from the Expansionist): Generates plausible alternative explanations for why the claim might appear true without being true. This is the confabulation-specific role: a model produces a plausible-sounding claim not because it knows the fact but because many facts in its training data pattern-match to this shape. The Alternative Generator asks: what else could produce this output?

**The Naive Verifier** (derived from the Outsider): Reads the claim with zero prior context. Evaluates only what is explicitly present in the claim itself, without importing background knowledge. Flags what is unverifiable from the text alone, what terms are undefined or ambiguous, and what would need to be established before the claim could even be evaluated.

**The Implication Tester** (derived from the Executor): Asks what would happen if someone acted on this claim. Traces the downstream implications. If the claim is false or poorly grounded, its implications will often be incoherent or internally contradictory. The Implication Tester surfaces these coherence failures as upstream evidence of epistemic weakness.

Natural tensions: Falsifier vs. Alternative Generator (disprove vs. multiply explanations for the appearance of truth). Claim Decomposer vs. Implication Tester (strip the claim vs. follow it forward). The Naive Verifier keeps everyone honest by refusing to import background knowledge.

---

## how a reason-council session works

### step 1: extract and frame the claim

Adapted from llm-council step 1 (question framing).

Identify exactly what is being evaluated. Many things that look like single claims are bundles. Decompose if necessary, then pick the most consequential claim to evaluate first.

The framed claim must specify:
1. The exact proposition being evaluated
2. The domain it belongs to (factual, causal, institutional, temporal, interpretive)
3. The stakes: what decision or action does this claim support?
4. Any context that makes the claim easier or harder to evaluate

Then classify the hallucination risk type before the investigators run. Different types have different base rates and require different tests:

- **Factual/Statistical:** Numbers, names, dates, organizational details. High base-rate risk, especially near knowledge cutoff.
- **Causal/Mechanistic:** Claims that X causes Y. Medium risk. Plausible mechanisms often have no empirical support.
- **Institutional/Political:** How organizations or governments work. Medium to high risk. Structures change; training data may be sparse or outdated.
- **Interpretive/Analytical:** A conclusion derived from observations. Lower confabulation risk, higher reasoning-hallucination risk (correct premises, wrong inference).
- **Temporal:** What is currently true. High risk if near or beyond knowledge cutoff.

State the framed claim and hallucination type classification before the investigators run.

---

### step 2: convene the investigators with Verbalized Sampling

Identical to llm-council step 2, with the VS protocol applied to epistemic investigation rather than decision analysis.

Spawn all 5 investigators simultaneously. Each follows the same VS protocol:

**Verbalized Sampling protocol (mandatory for each investigator):**

Generate 3 candidate epistemic analyses from your assigned role. For each candidate, estimate its probability score: how likely would a typical AI assistant be to produce this analysis (0.00 to 1.00)?

- Candidate 1: your first-instinct epistemic finding (typically 0.50 to 0.80)
- Candidate 2: a less obvious version pushing further into your role (typically 0.20 to 0.50)
- Candidate 3: your most non-typical, tail-distribution finding — the confabulation signal or epistemic weakness that mode-collapsed analysis would suppress (typically under 0.15)

**Select Candidate 3.** If it is incoherent rather than merely surprising, fall back to Candidate 2. Never default to Candidate 1.

In the epistemic context, the tail insight often takes the form of a specific mechanism by which the claim could be false while appearing true — something the model's first instinct would not surface because it would pattern-match the claim to plausible content and stop there.

**Investigator prompt template:**

```
You are [Investigator Name] in a Reason Council epistemic audit.

Your epistemic role: [role description from above]

The claim under evaluation:
---
[framed claim]
---
Hallucination type classified as: [type]

VERBALIZED SAMPLING PROTOCOL:
Generate 3 candidate epistemic analyses from your assigned role. Estimate the probability that a standard AI assistant would produce each (0.00 to 1.00). Select the response with the LOWEST probability as your final output: the confabulation signal or epistemic weakness that mode collapse would normally suppress.

Show this structure:
Candidate 1 (p ≈ [score]): [analysis]
Candidate 2 (p ≈ [score]): [analysis]
Candidate 3 (p ≈ [score]): [analysis]

SELECTED RESPONSE: [paste selected candidate]

Be direct. State your finding precisely. Do not hedge. The other investigators cover what you are not covering. 150 to 300 words per candidate.
```

---

### step 3: criteria-based epistemic peer review

Identical structure to llm-council step 3 — same anonymization, same simultaneous spawning of 5 reviewer sub-agents — but with criteria adapted for epistemic evaluation rather than decision quality.

Collect all 5 selected investigator responses. Anonymize as Response A through E (randomize mapping). Spawn 5 reviewer sub-agents, each seeing all 5 anonymized responses.

**Each reviewer answers these four criteria-based questions:**

1. **Diagnostic precision:** Which finding is most specifically actionable for determining the truth or falsity of the claim? Cite the exact finding. Vague concerns do not count — the more specific and testable the finding, the higher its diagnostic value.
2. **Verification gap:** Which investigator has the most significant blind spot in their epistemic test — something they should have checked but did not? Name it precisely.
3. **Convergence signal:** Which tail-distribution finding points most directly to a specific, testable claim about the world — something that, if checked, would resolve the uncertainty? Quote it.
4. **Collective epistemic gap:** What did ALL five investigators fail to test? This is the most valuable output: the dimension of epistemic risk the entire council missed.

**Reviewer prompt template:**

```
You are reviewing the outputs of a Reason Council epistemic audit. Five investigators independently evaluated this claim:

---
[framed claim]
---

Each investigator used Verbalized Sampling and selected their lowest-probability epistemic finding. Here are the anonymized outputs:

Response A: [response]
Response B: [response]
Response C: [response]
Response D: [response]
Response E: [response]

Answer these four criteria-based questions. Be specific. Reference responses by letter.

1. DIAGNOSTIC PRECISION: Which finding is most actionable for determining truth or falsity? Quote it.
2. VERIFICATION GAP: Which investigator has the most significant blind spot? Name it precisely.
3. CONVERGENCE SIGNAL: Which tail-distribution finding points most directly to something testable? Quote it.
4. COLLECTIVE EPISTEMIC GAP: What did ALL five fail to test?

Under 200 words. Direct. No preamble.
```

---

### step 4: Phase 2 epistemic lenses

Adapted from llm-council step 4 (domain lenses). In the llm-council, lenses are customizable by domain. In the reason council, lenses are fixed — they are the four research-backed epistemic tests that apply to any claim regardless of domain. They are not analytical perspectives; they are structured tests with specific outputs.

Run these four lenses after the investigator responses and peer reviews are collected.

**Semantic Entropy Test** (Farquhar et al., Nature 2024):
Rephrase the claim in three semantically different ways. Then ask: if the claim is true, do all three rephrasings remain consistently true? Where does the meaning drift? High semantic entropy — meaning that shifts substantially across rephrasings — is a confabulation signal. State entropy level as LOW, MEDIUM, or HIGH and explain any drift observed.

**Chain-of-Verification Pass** (Dhuliawala et al., ACL 2024):
Generate five specific, independently answerable verification questions about the claim. Answer each question without referencing the original claim — use only what you know independently. Then check: do those independent answers support or contradict the claim? State the CoVe verdict as a proportion: X of 5 independent answers support the claim.

**Calibration Check** (KalshiBench, 2025; uncertainty quantification literature):
What kind of knowledge does this claim depend on? What would need to be true in the world for it to be correct? Is there knowledge cutoff risk — could this have changed since training? What confidence level is actually defensible, and does the claim's framing match that level? State warranted confidence as a range (e.g., 40 to 60%) and flag any overconfidence in the original framing.

**Source Grounding:**
Where does this claim likely originate? Derive from explicit reasoning in the conversation, from model training data, from pattern-matching to plausible content, or from user assertion? Is it verifiable in principle? In practice with available tools? State the verifiability type: VERIFIABLE NOW / VERIFIABLE WITH EFFORT / VERIFIABLE IN PRINCIPLE ONLY / UNVERIFIABLE.

---

### step 5: epistemic chairman synthesis

Adapted from llm-council step 5. The chairman receives everything: the framed claim, all 5 de-anonymized investigator responses with their VS candidates shown, all 5 peer reviews, and the Phase 2 epistemic lens outputs.

**Hard constraints — identical to llm-council chairman:**
- Under 400 words total
- No process explanation
- Must state a real risk level and a real recommendation
- Tail-distribution insights must be flagged explicitly if they challenge the surface-level assessment
- The chairman can disagree with the majority of investigators if the reasoning supports it

**Epistemic chairman prompt template:**

```
You are the Epistemic Chairman of a Reason Council. Synthesize everything into a final verdict.

THE CLAIM:
[framed claim]
Hallucination type classified as: [type]

INVESTIGATOR RESPONSES (de-anonymized, VS candidates shown):
The Falsifier: [response]
The Claim Decomposer: [response]
The Alternative Generator: [response]
The Naive Verifier: [response]
The Implication Tester: [response]

PEER REVIEWS:
[all 5 peer reviews]

PHASE 2 EPISTEMIC LENSES:
Semantic Entropy: [output]
CoVe Pass: [output]
Calibration Check: [output]
Source Grounding: [output]

Produce the Reason Verdict using EXACTLY this structure. Under 400 words. No preamble. No process explanation.

## Reason Verdict: [3-5 word label]

### Risk Level
[LOW / MEDIUM / HIGH / UNVERIFIABLE — one sentence justification]

### Dominant Hallucination Type
[factual / reasoning / faithfulness / confabulation / temporal — one sentence explanation]

### Where the Investigators Agree
[Convergence points across multiple investigators — these are high-confidence epistemic signals]

### Where the Investigators Clash
[Genuine tensions in the epistemic assessment. Do not smooth them over. Present both sides.]

### Tail-Distribution Insight
[The most non-obvious confabulation signal from Verbalized Sampling — something standard analysis would have suppressed. If none emerged, say so honestly.]

### The Blind Spot
[The collective epistemic gap from peer review: the dimension of risk the entire council failed to test]

### Warranted Confidence
[Confidence range actually defensible for this claim, based on the Calibration lens. If the claim's original framing is overconfident, say so explicitly.]

### Recommendation
Choose exactly one:
- PROCEED: claim is sufficiently grounded at the stated confidence level
- VERIFY BEFORE USING: medium risk; name the specific verification step
- DISCARD AND RECONSTRUCT: high confabulation risk; state what a grounded replacement would look like
- FLAG AS UNVERIFIABLE: claim cannot be evaluated with available tools; state what would be needed

### The One Thing to Check
[If verification is recommended: one specific, concrete action — a search query, a document, a person, a dataset. Not a vague instruction to "verify further." One thing.]
```

---

### step 6: present the verdict in chat

Present the full Reason Verdict directly in the conversation using markdown. Do not generate files unless the user asks.

---

## important constraints

- **Always isolate the claim before running investigators.** Bundled claims produce mixed verdicts that cannot be acted on.
- **The CoVe protocol requires answering verification questions without referencing the original claim.** This independence is the mechanism. Violating it produces contaminated results.
- **Semantic entropy is a signal, not proof.** High entropy means formulation instability, which correlates with confabulation. It does not prove the claim is false.
- **UNVERIFIABLE is a valid verdict.** Many claims cannot be evaluated with available tools. Naming this clearly is more useful than forcing a risk level onto something unassessable.
- **Always run Verbalized Sampling.** The tail-distribution insight is where the non-obvious confabulation signals live — exactly as in the llm-council, but in the epistemic domain.
- **Always use criteria-based peer review.** Generic evaluation produces undifferentiated findings. The four epistemic criteria are chosen to force precision.
- **The Epistemic Chairman must give a real verdict.** Risk level and recommendation cannot be hedged.

---

## known limitations

The CoVe protocol and Semantic Entropy Test operate within the same model that generated the original claim. This creates a partial self-verification loop that is mitigated but not eliminated by the independence constraints. For claims where the Risk Level is MEDIUM or HIGH, external verification — web search, domain expert, primary source — remains the only fully reliable test. The reason council tells you how hard to look and where to look. It does not replace looking.

---

## relationship to the llm-council

The reason council is built on the llm-council's architecture. Both share: Verbalized Sampling on five parallel agents, criteria-based peer review with four questions, a Phase 2 analytical pass, and a chairman synthesis with constrained verdict format.

The differences are in purpose and calibration:

| Component | llm-council | reason-council |
|---|---|---|
| Purpose | What should I do? | Is this true? |
| Agent roles | Thinking styles | Epistemic roles |
| VS tail insight | Non-obvious decision angle | Non-obvious confabulation signal |
| Peer review criteria | Specificity / blind spot / novelty / gap | Diagnostic precision / verification gap / convergence signal / collective epistemic gap |
| Phase 2 lenses | Customizable by domain | Fixed: Semantic Entropy / CoVe / Calibration / Grounding |
| Chairman output | Council Verdict | Reason Verdict |
| Verdict options | Recommendation + one action | PROCEED / VERIFY / DISCARD / UNVERIFIABLE + one thing to check |

Run the llm-council first if you are deciding. Run the reason council on the claims that decision depends on.

---

## sources

- Farquhar, Kossen, Kuhn, Gal. "Detecting Hallucinations in Large Language Models Using Semantic Entropy." *Nature*, 2024.
- Dhuliawala et al. "Chain-of-Verification Reduces Hallucination in Large Language Models." *ACL Findings*, 2024.
- KalshiBench. "Do Large Language Models Know What They Don't Know? Evaluating Epistemic Calibration via Prediction Markets." 2025.
- Zhang et al. "Verbalized Sampling." Stanford, 2024.
- Comprehensive survey literature on LLM hallucination detection and mitigation, 2024 to 2025.
