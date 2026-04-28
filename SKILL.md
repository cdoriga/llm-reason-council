---

name: reason-council

description: "Run any claim or AI output through a structured epistemic protocol to evaluate hallucination and confabulation risk. Architecturally inspired by the llm-council, adapted for evaluating claims rather than decisions. This is structured skepticism scaffolding, not a hallucination detector: the auditing model is the same model that produced the claim, so verdicts are calibration signals, not determinations. A simpler two-prompt alternative is provided as the default starting point; reserve the full protocol for high-stakes claims. MANDATORY TRIGGERS: 'reason-council this', 'is this hallucination', 'is this grounded', 'fact-check this', 'epistemic audit', 'verify this claim', 'sanity-check this', 'is this real or AI'. STRONG TRIGGERS: 'am I making this up', 'is this actually true', 'how confident should I be', 'does this hold up', 'is the AI bullshitting me', 'is this defensible'. DO NOT use for decisions between options (use the llm-council instead)."

---

# Reason Council

The llm-council helps you decide. The reason council helps you know whether what you are deciding on is real.

It runs the same architecture as the llm-council (five parallel agents, criteria-based peer review, a Phase 2 analytical pass, and a chairman synthesis) adapted for a different question: not "what should I do?" but "is this true, and if not, where would it break?"

---

## read this first

This section is the primary context for every verdict the protocol produces. Skipping it is misuse of the skill. It is short on purpose.

### what this skill is

A structured protocol that forces methodical doubt about a specific claim, surfaces non-obvious confabulation signals, and tells you where verification effort is best spent. It produces a calibration signal, not a determination of truth.

### what this skill is not

A hallucination detector. The protocol runs within the same model that produced the original claim, which means it is structurally incapable of fully escaping the biases that produced the claim in the first place. No prompt-level intervention closes this loop. The skill tells you how hard to look and where; it does not tell you whether the claim is true.

The most dangerous failure mode is structural: claims that look low-risk to the auditing model are precisely the claims most likely to be confabulations the model cannot see. For this reason, every verdict, including LOW risk, requires an external verification step. The verdict format below makes this mandatory rather than optional.

### who this skill is for

People who already practice epistemic care and want a more systematic protocol for it. Small teams that want a shared vocabulary for AI output skepticism. Researchers, project managers, and analysts whose work is read or relied on by others. It is not designed for casual AI users without context for interpreting verdicts critically.

### the simpler alternative comes first

Many checks do not need the full protocol. The two-prompt counterfactual described in the next section produces useful signal in two minutes. Use the full council only when the cost of acting on a confabulation is high (a deliverable to a donor, a public claim, a strategic assumption underlying a project) or when the simpler version produced an undifferentiated or evasive answer.

If you have not done so before, run BOTH protocols on three real claims of different types (factual, causal, institutional) on your first use of this skill. Compare which produces more specific, actionable, or non-obvious findings. If the full council does not clearly win on at least two of three, default to the simpler version for everything except the highest-stakes claims. This calibrates the skill to your context rather than to its author's.

---

## the simpler alternative

Run this first. Most claims do not need more.

**Prompt 1 (falsification):**
> List five concrete, specific ways the following claim could be false. Be specific: name the mechanism, the missing evidence, or the alternative explanation. Do not list generic concerns. Each item must point to something that could in principle be checked.
>
> Claim: [paste claim]

**Prompt 2 (confabulation generation):**
> List three specific reasons the following claim could appear true to a language model without actually being true: pattern matches in training data, plausible but unverified causal chains, or surface features that mimic well-grounded claims. For each, state what would distinguish a real instance from a confabulated one.
>
> Claim: [paste claim]

If both prompts return specific, testable findings, the simpler protocol has done its job. If they return vague or evasive answers, escalate to the full council below.

---

## the architecture this is built on

The full council is architecturally inspired by the following research. The techniques were developed and validated at the model-inference level (controlled multi-sample generation, probability distribution access). This skill adapts their logic into a prompt-engineering protocol, which preserves the epistemic forcing function without claiming full technical equivalence. The empirical figures cited below apply to the original research contexts, not to this implementation.

**Semantic Entropy** (Farquhar, Kossen, Kuhn, Gal, Nature 2024): Confabulations are detectable by measuring uncertainty at the level of meaning rather than word choice. If the same claim, asked multiple ways, produces semantically inconsistent answers, the model is pattern-matching to plausible text rather than retrieving a stable underlying fact. The Phase 2 Semantic Entropy Test adapts this logic by rephrasing the claim and checking for meaning drift. It approximates the diagnostic signal; it does not compute entropy in the technical sense.

**Chain-of-Verification (CoVe)** (Dhuliawala et al., ACL Findings 2024): Draft a claim, generate verification questions, answer them independently without access to the original claim, check for contradictions. The original research achieved independence by blocking model access to the claim during answer generation, which cannot be fully replicated in a prompt-engineering context where the claim remains in the context window. The Phase 2 CoVe Pass approximates this through methodological independence: each verification question is answered before reading the others, and answers are derived without explicitly referencing the claim. This is partial independence. The 50 to 70 percent hallucination reduction figures from the original paper assume full independence in a model-inference setting.

**Epistemic overconfidence** (KalshiBench, 2025): All frontier LLMs are systematically overconfident, and extended reasoning can worsen calibration rather than improve it. Expressed confidence is not a reliable signal of correctness. This informs the Calibration lens and the Reason Verdict format.

**Adversarial Perspective Forcing** (adapted from Verbalized Sampling, Zhang et al., Stanford 2024): Deliberately generating analyses from multiple epistemic stances, including adversarial ones, surfaces findings that a mode-collapsed first-instinct response would suppress. The original VS research demonstrated diversity gains from probabilistic tail-distribution sampling. This skill adapts the insight into a structured three-candidate protocol that forces an adversarial third analysis regardless of the model's default inclination. The mechanism is enforced perspective adversarialism, not probabilistic sampling.

---

## when to run the full council

The full council evaluates claims, not decisions.

**Bring it:**
- A claim an AI made that you want pressure-tested ("Slow Food has X members in Y country")
- An assumption underlying a plan you are developing
- A causal chain ("X causes Y because Z")
- A strategic framing you want stress-tested for factual grounding
- An AI-generated output you are about to use in a professional context
- A belief you hold and want to evaluate epistemically

**Do not bring it:**
- Decisions between options (use the llm-council)
- Value judgments or normative claims with no fact of the matter
- Creative or hypothetical questions
- Quick checks where the simpler alternative will do

---

## the five investigators

These derive from the same five thinking-style tensions in the llm-council, adapted into epistemic roles. The productive tensions are preserved: Falsifier vs. Alternative Generator (attack the claim vs. multiply explanations), Claim Decomposer vs. Implication Tester (strip the claim to its core vs. trace its downstream consequences). The Naive Verifier keeps everyone honest by reading only what is on the page.

**The Falsifier** (derived from the Contrarian): Tries to disprove the claim using logical analysis, counterexamples, or any contradicting knowledge. Does not look for nuance. Tries to break the claim. If it cannot be broken, that is informative. If it breaks easily, that is more informative.

**The Claim Decomposer** (derived from the First Principles Thinker): Strips the claim to its irreducible commitments. What is actually being asserted? Many claims contain bundled sub-claims, hidden assumptions, or vague quantifiers that make them unfalsifiable by design. The Decomposer names exactly what would need to be true and in what form.

**The Alternative Generator** (derived from the Expansionist): Generates plausible alternative explanations for why the claim might appear true without being true. This is the confabulation-specific role: a model produces a plausible-sounding claim not because it knows the fact but because many facts in its training data pattern-match to this shape. The Alternative Generator asks: what else could produce this output?

**The Naive Verifier** (derived from the Outsider): Reads the claim with zero prior context. Evaluates only what is explicitly present in the claim itself, without importing background knowledge. Flags what is unverifiable from the text alone, what terms are undefined or ambiguous, and what would need to be established before the claim could even be evaluated.

**The Implication Tester** (derived from the Executor): Asks what would happen if someone acted on this claim. Traces the downstream consequences. If the claim is false or poorly grounded, its implications will often be incoherent or internally contradictory. The Implication Tester surfaces these coherence failures as upstream evidence of epistemic weakness.

---

## how a reason-council session works

### step 1: extract and frame the claim

Identify exactly what is being evaluated. Many things that look like single claims are bundles. Decompose if necessary, then pick the most consequential claim to evaluate first.

The framed claim must specify:
1. The exact proposition being evaluated
2. The domain it belongs to (factual, causal, institutional, temporal, interpretive)
3. The stakes: what decision or action does this claim support?
4. Any context that makes the claim easier or harder to evaluate

Then classify the hallucination risk type before the investigators run:
- **Factual or statistical:** Numbers, names, dates, organizational details. High base-rate risk, especially near knowledge cutoff.
- **Causal or mechanistic:** Claims that X causes Y. Medium risk. Plausible mechanisms often have no empirical support.
- **Institutional or political:** How organizations or governments work. Medium to high risk; structures change and training data may be sparse.
- **Interpretive or analytical:** A conclusion derived from observations. Lower confabulation risk, higher reasoning-hallucination risk (correct premises, wrong inference).
- **Temporal:** What is currently true. High risk if near or beyond knowledge cutoff.

State the framed claim and hallucination type before the investigators run.

### step 2: convene the investigators with Adversarial Perspective Forcing

Spawn all five investigators simultaneously. Each follows the Adversarial Perspective Forcing protocol below.

**Adversarial Perspective Forcing protocol (mandatory for each investigator):**

Generate three candidate epistemic analyses from your assigned role, in this sequence:

- Candidate 1: your first-instinct finding, the analysis a careful AI assistant would produce immediately, without being pushed.
- Candidate 2: a harder version of Candidate 1 that pushes further into your epistemic role and surfaces what Candidate 1 left unexamined.
- Candidate 3: what a hostile peer reviewer would say about Candidates 1 and 2 combined: the finding both missed, the assumption both shared, the mechanism neither named. This is not a critique of Candidates 1 and 2; it is the epistemic insight that requires actively working against your own prior analyses to reach.

**Select Candidate 3.** If it is incoherent rather than merely non-obvious, fall back to Candidate 2. Never default to Candidate 1.

In the epistemic context, the value of Candidate 3 is typically a specific mechanism by which the claim could be false while appearing true. This is something a mode-collapsed first analysis would not surface because pattern-matching to plausible content is easier and stops sooner.

**Investigator prompt template:**

```
You are [Investigator Name] in a Reason Council epistemic audit.

Your epistemic role: [role description from above]

The claim under evaluation:
---
[framed claim]
---
Hallucination type classified as: [type]

ADVERSARIAL PERSPECTIVE FORCING PROTOCOL:
Generate three candidate epistemic analyses from your assigned role.

Candidate 1: Your first-instinct finding, what a careful analyst would produce without being pushed.
Candidate 2: A harder version that pushes further into your role and surfaces what Candidate 1 left unexamined.
Candidate 3: What a hostile peer reviewer would say about both, the finding they missed, the assumption they shared, the mechanism neither named.

Select Candidate 3 as your final output. If it is incoherent rather than merely non-obvious, use Candidate 2. Never default to Candidate 1.

Show this structure:
Candidate 1: [analysis]
Candidate 2: [analysis]
Candidate 3: [analysis]

SELECTED RESPONSE: [paste selected candidate]

Be direct. State your finding precisely. Do not hedge. The other investigators cover what you are not covering. 150 to 300 words per candidate.
```

### step 3: criteria-based epistemic peer review

Collect all five selected investigator responses. Anonymize as Response A through E (randomize the mapping). Spawn five reviewer sub-agents, each seeing all five anonymized responses.

Each reviewer answers four criteria-based questions:

1. **Diagnostic precision:** Which finding is most specifically actionable for determining the truth or falsity of the claim? Quote the exact finding.
2. **Verification gap:** Which investigator has the most significant blind spot in their epistemic test, something they should have checked but did not?
3. **Convergence signal:** Which tail-distribution finding points most directly to a specific, testable claim about the world, something that, if checked, would resolve the uncertainty? Quote it.
4. **Collective epistemic gap:** What did ALL five investigators fail to test? This is the most valuable output: the dimension of epistemic risk the entire council missed.

**Reviewer prompt template:**

```
You are reviewing the outputs of a Reason Council epistemic audit. Five investigators independently evaluated this claim:

---
[framed claim]
---

Each investigator used Adversarial Perspective Forcing and selected their hostile-peer-review finding. Here are the anonymized outputs:

Response A: [response]
Response B: [response]
Response C: [response]
Response D: [response]
Response E: [response]

Answer these four criteria-based questions. Be specific. Reference responses by letter.

1. DIAGNOSTIC PRECISION: Which finding is most actionable for determining truth or falsity? Quote it.
2. VERIFICATION GAP: Which investigator has the most significant blind spot? Name it precisely.
3. CONVERGENCE SIGNAL: Which finding points most directly to something testable? Quote it.
4. COLLECTIVE EPISTEMIC GAP: What did ALL five fail to test?

Under 200 words. Direct. No preamble.
```

### step 4: Phase 2 epistemic lenses

In the llm-council, lenses are customizable by domain. In the reason council, lenses are fixed: they are the four research-backed epistemic tests that apply to any claim regardless of domain. They are structured tests with specific outputs, not analytical perspectives.

**Semantic Entropy Test** (approximated from Farquhar et al., 2024):
Rephrase the claim in three semantically different ways. If the claim is true, do all three rephrasings remain consistently true? Where does the meaning drift? Substantial drift across rephrasings is a confabulation signal. State entropy as LOW, MEDIUM, or HIGH and explain any drift observed.

**Chain-of-Verification Pass** (approximated from Dhuliawala et al., 2024):
Generate five specific, independently answerable verification questions about the claim. Answer each before reading the others, without explicitly referencing the original claim, drawing only on independent knowledge. Then check whether those independent answers support or contradict the claim. State the CoVe verdict as a proportion: X of 5 independent answers support the claim. Note that this is methodological independence, not technical independence; the claim remains in context.

**Calibration Check** (KalshiBench, 2025; uncertainty quantification literature):
What kind of knowledge does this claim depend on? What would need to be true in the world for it to be correct? Is there knowledge cutoff risk? What confidence level is actually defensible, and does the claim's framing match that level? State warranted confidence as a range (for example, 40 to 60 percent) and flag any overconfidence in the original framing.

**Source Grounding:**
Where does this claim likely originate? From explicit reasoning in the conversation, from model training data, from pattern-matching to plausible content, or from user assertion? Is it verifiable in principle, and in practice with available tools? State the verifiability type: VERIFIABLE NOW / VERIFIABLE WITH EFFORT / VERIFIABLE IN PRINCIPLE ONLY / UNVERIFIABLE.

### step 5: epistemic chairman synthesis

The chairman receives the framed claim, all five de-anonymized investigator responses (with APF candidates shown), all five peer reviews, and the Phase 2 lens outputs.

**Hard constraints:**
- Under 400 words total
- No process explanation
- A real risk level and a real required verification step
- Tail-distribution insights flagged explicitly if they challenge the surface-level assessment
- The chairman can disagree with the majority of investigators if the reasoning supports it

**Epistemic chairman prompt template:**

```
You are the Epistemic Chairman of a Reason Council. Synthesize everything into a final verdict.

THE CLAIM:
[framed claim]
Hallucination type classified as: [type]

INVESTIGATOR RESPONSES (de-anonymized, APF candidates shown):
The Falsifier: [response]
The Claim Decomposer: [response]
The Alternative Generator: [response]
The Naive Verifier: [response]
The Implication Tester: [response]

PEER REVIEWS:
[all five peer reviews]

PHASE 2 EPISTEMIC LENSES:
Semantic Entropy: [output]
CoVe Pass: [output]
Calibration Check: [output]
Source Grounding: [output]

Produce the Reason Verdict using EXACTLY this structure. Under 400 words. No preamble. No process explanation.

## Reason Verdict: [3 to 5 word label]

### Risk Level
[LOW / MEDIUM / HIGH / UNVERIFIABLE, with one sentence justification]

### Dominant Hallucination Type
[factual / reasoning / faithfulness / confabulation / temporal, with one sentence explanation]

### Where the Investigators Agree
[Convergence points across multiple investigators; high-confidence epistemic signals]

### Where the Investigators Clash
[Genuine tensions in the epistemic assessment. Do not smooth them over.]

### Tail-Distribution Insight
[The most non-obvious confabulation signal from APF. If none emerged, say so honestly.]

### The Blind Spot
[The collective epistemic gap from peer review: the dimension of risk the entire council failed to test]

### Warranted Confidence
[Confidence range actually defensible for this claim. If the original framing is overconfident, say so explicitly.]

### Required External Verification
[ONE specific, concrete external action: a search query, a document, a person, a dataset. This is mandatory at every risk level, including LOW. The council's verdict is a calibration signal; this step is what makes it actionable. If the recommended action is "discard and reconstruct," state what a grounded replacement would look like and what would need to be checked to support it.]
```

### step 6: present the verdict in chat

Present the full Reason Verdict directly in the conversation. Do not generate files unless the user asks.

---

## the verdict format, briefly

The verdict reports four things: risk level, dominant hallucination type, what the investigators found, and one required external verification step. The risk level is a description of the council's signal, not an authorization to act. The verification step is what closes the loop. Both are required for the verdict to be useful.

Risk levels:
- **LOW:** No strong confabulation signals were detected within the model. This does not mean the claim is true; the auditing model shares biases with the generating model. Run the verification step before relying on the claim.
- **MEDIUM:** Specific signals warrant verification. Run the verification step before any consequential use.
- **HIGH:** Strong adverse signals detected. Treat the claim as likely false or poorly grounded. The verification step here is typically reconstruction, not confirmation.
- **UNVERIFIABLE:** The claim cannot be evaluated from within the model. State what would be needed to evaluate it; the verification step is the path to that.

There is no PROCEED label and no DISCARD label. The skill produces an epistemic state, not an instruction. The action is yours.

---

## important constraints

- **Always isolate the claim before running investigators.** Bundled claims produce mixed verdicts that cannot be acted on.
- **Always run the simpler alternative first** unless the stakes clearly justify the overhead. The full protocol earns its weight only on claims where the cost of acting on a confabulation is high.
- **The CoVe protocol achieves methodological independence, not technical independence.** The claim remains in the context window. Genuine isolation is impossible in a prompt-engineering context. For HIGH risk claims, treat CoVe as a signal rather than a test.
- **Semantic entropy is a signal, not proof.** High entropy means formulation instability, which correlates with confabulation. It does not prove the claim is false.
- **UNVERIFIABLE is a valid verdict.** Many claims cannot be evaluated with available tools. Naming this clearly is more useful than forcing a risk level onto something unassessable.
- **Always run Adversarial Perspective Forcing.** The Candidate 3 insight is where non-obvious confabulation signals live. The mechanism is enforced perspective adversarialism, not probabilistic sampling.
- **The required external verification step is mandatory at every risk level, including LOW.** This is the architectural mitigation of the self-verification loop. A LOW reading without external verification is incomplete.
- **A structured verdict does not reduce the need for external verification.** The verdict is a calibration signal: it tells you whether and where to verify. It does not replace verifying.

---

## known limitations

**Self-verification loop.** The CoVe and Semantic Entropy approximations operate within the same model that generated the original claim. This is mitigated but not eliminated by the independence constraints, by Adversarial Perspective Forcing, and by the mandatory external verification step. The skill reduces the probability of acting on confabulations; it does not reduce it to zero.

**Adversarial Perspective Forcing is a prompt-level mechanism, not probabilistic sampling.** The original Verbalized Sampling research accessed the model's output distribution directly. Candidate 3 here is selected by enforced perspective adversarialism, not tail distribution access. It reliably surfaces less obvious findings; it does not guarantee they represent the tail of the probability distribution.

**The counterfactual is built into first use, not pre-validated.** This skill has not been systematically compared against simpler alternatives in a controlled study. The simpler two-prompt alternative is provided so each adopter can run the comparison on their own claims and calibrate. If you find the full protocol does not produce meaningfully better signal than the simpler version on your claim types, default to the simpler version. This is by design.

**Structured verdicts can increase overtrust.** A formal risk rating with a structured layout produces more user confidence than an informal analysis, even when both rest on the same foundation. The mandatory verification step is the architectural defense against this. If a verdict is read and acted on without running the verification step, the protocol has failed regardless of what the verdict said.

---

## relationship to the llm-council

The reason council is built on the llm-council's architecture. Both share five parallel agents with structured candidate selection, criteria-based peer review with four anonymized reviewers, a Phase 2 analytical pass, and a chairman synthesis with constrained verdict format.

The differences are in purpose and calibration:

| Component | llm-council | reason-council |
|---|---|---|
| Purpose | What should I do? | Is this true? |
| Agent roles | Thinking styles | Epistemic roles |
| Forcing mechanism | Verbalized Sampling (probabilistic) | Adversarial Perspective Forcing (stance-based) |
| Tail insight | Non-obvious decision angle | Non-obvious confabulation signal |
| Peer review criteria | Specificity, blind spot, novelty, gap | Diagnostic precision, verification gap, convergence signal, collective gap |
| Phase 2 lenses | Customizable by domain | Fixed: Semantic Entropy, CoVe, Calibration, Grounding |
| Chairman output | Council Verdict | Reason Verdict |
| Verdict structure | Recommendation plus one action | Risk level plus mandatory external verification |

Run the llm-council first if you are deciding. Run the reason council on the claims that decision depends on.

---

## sources

- Farquhar, Kossen, Kuhn, Gal. "Detecting Hallucinations in Large Language Models Using Semantic Entropy." *Nature*, 2024.
- Dhuliawala et al. "Chain-of-Verification Reduces Hallucination in Large Language Models." *ACL Findings*, 2024.
- KalshiBench. "Do Large Language Models Know What They Don't Know? Evaluating Epistemic Calibration via Prediction Markets." 2025.
- Zhang et al. "Verbalized Sampling." Stanford, 2024. (Informs the Adversarial Perspective Forcing protocol; the skill adapts the insight about forcing non-default outputs rather than implementing probabilistic tail sampling.)
- Comprehensive survey literature on LLM hallucination detection and mitigation, 2024 to 2025.
