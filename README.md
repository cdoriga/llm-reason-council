# reason-council

A structured prompt-engineering protocol for evaluating whether AI-generated claims are grounded or hallucinated. Sister project to the [llm-council](https://github.com/cdoriga/llm-council). Where the llm-council helps you decide, the reason-council helps you check whether what you are deciding on is real.

This skill produces a calibration signal, not a determination of truth. It is structured skepticism scaffolding, not a hallucination detector. The distinction matters and is the reason the rest of this README exists.

---

## what this is

A skill for Claude (and any LLM that supports skill-style instructions) that runs a specific claim through a five-investigator epistemic protocol with criteria-based peer review, four research-backed lenses, and a chairman synthesis. The output is a calibrated risk reading and one mandatory external verification step.

Built on four pieces of public research:

- Semantic Entropy (Farquhar et al., *Nature*, 2024)
- Chain-of-Verification (Dhuliawala et al., ACL Findings, 2024)
- Verbalized Sampling (Zhang et al., Stanford, 2024), adapted as Adversarial Perspective Forcing
- Calibration research from KalshiBench, 2025

Each is adapted from its original model-inference setting into a prompt-level approximation. The trade-offs are documented in `SKILL.md` and in the limitations section below.

## what this is not

A hallucination detector. The protocol runs inside the same model that produced the original claim, which means it cannot fully escape the biases that produced the claim in the first place. No prompt-level intervention closes that loop.

The most dangerous failure mode is structural: claims that look low-risk to the auditing model are precisely the claims most likely to be confabulations the model cannot see. **For this reason, every verdict, including LOW risk, requires an external verification step.** If you do not plan to follow that step, do not use this skill.

What the skill does well: forces methodical doubt, surfaces specific verification targets, tells you where verification effort is best spent.

What it cannot do: replace external verification, ground claims by structure alone, determine truth.

## who it is for

People who already practice epistemic care and want a more systematic protocol. Small teams that want a shared vocabulary for AI output skepticism. Researchers, project managers, and analysts whose work is read or relied on by others.

It is not designed for casual AI users. The structured verdict format will produce more confidence than the underlying analysis can defend if you do not understand its limits.

---

## start with the simpler alternative

Most claims do not need the full protocol. Run these two prompts first.

**Prompt 1, falsification:**

```
List five concrete, specific ways the following claim could be false. Be specific:
name the mechanism, the missing evidence, or the alternative explanation. Do not
list generic concerns. Each item must point to something that could in principle
be checked.

Claim: [paste claim]
```

**Prompt 2, confabulation generation:**

```
List three specific reasons the following claim could appear true to a language
model without actually being true: pattern matches in training data, plausible
but unverified causal chains, or surface features that mimic well-grounded
claims. For each, state what would distinguish a real instance from a
confabulated one.

Claim: [paste claim]
```

If both return specific, testable findings, the simpler protocol has done its job. If they return vague or evasive answers, escalate to the full council.

The simpler version is the default for a reason: if a two-minute check produces useful signal, the overhead of the full protocol is unjustified for that claim.

---

## installation

The skill follows the [Agent Skills open standard](https://agentskills.io). Two paths to install it.

### Claude.ai (web or desktop)

1. Download this repo as a ZIP, or just `SKILL.md`.
2. If you downloaded only `SKILL.md`, place it inside a folder named `reason-council/`, then ZIP that folder. The ZIP must contain the folder as its root.
3. In Claude.ai, go to **Settings, then Capabilities, then Skills** (on some plans this lives under **Customize, then Skills**). Make sure **Code execution and file creation** is enabled.
4. Click the **+** button and upload the ZIP.
5. Toggle the skill on.

Trigger phrases the skill responds to:

- `reason-council this`
- `is this hallucination`
- `is this grounded`
- `fact-check this`
- `epistemic audit this`
- `verify this claim`
- `sanity-check this`

### Claude Code

```bash
mkdir -p ~/.claude/skills/reason-council
cp SKILL.md ~/.claude/skills/reason-council/
```

The skill becomes available as `/reason-council` and Claude can also invoke it automatically when relevant.

### Other LLMs

The protocol is documented in plain prose in `SKILL.md`. It can be adapted to any model that handles long instructions reliably. The five-investigator structure, the peer review, and the Phase 2 lenses are all expressed as prompt templates you can run sequentially.

---

## what a verdict looks like

After running the full protocol on a claim, the output is a structured Reason Verdict under 400 words. The shape:

```
## Reason Verdict: [3 to 5 word label]

### Risk Level
LOW / MEDIUM / HIGH / UNVERIFIABLE, with a one-sentence justification.

### Dominant Hallucination Type
factual / reasoning / faithfulness / confabulation / temporal.

### Where the Investigators Agree
Convergence points across multiple investigators.

### Where the Investigators Clash
Genuine tensions in the epistemic assessment, presented without being smoothed over.

### Tail-Distribution Insight
The most non-obvious confabulation signal from Adversarial Perspective Forcing.
If none emerged, the verdict says so.

### The Blind Spot
The collective epistemic gap from peer review: what the entire council failed to test.

### Warranted Confidence
The confidence range actually defensible for this claim. If the original framing
was overconfident, the verdict states so.

### Required External Verification
ONE specific external action: a search query, a document, a person, a dataset.
This is mandatory at every risk level, including LOW.
```

There is no PROCEED label and no DISCARD label. The skill produces an epistemic state, not an instruction. The action is yours.

---

## when to use the full council

The full council evaluates claims, not decisions. Use it when:

- An AI made a claim you want pressure-tested before relying on it
- A plan you are developing rests on an assumption you cannot easily verify
- You are about to publish, propose, or commit to a strategic framing and want it stress-tested for factual grounding
- The simpler alternative produced an undifferentiated or evasive answer
- The cost of acting on a confabulation is high

Do not use it for:

- Decisions between options (use the [llm-council](https://github.com/cdoriga/llm-council) instead)
- Value judgments or normative claims with no fact of the matter
- Creative or hypothetical questions
- Quick checks where the simpler alternative will do

---

## the architecture, briefly

The skill runs five parallel investigators on a single claim. Each is derived from a thinking style adapted from the llm-council and recast as an epistemic role:

- **The Falsifier** tries to break the claim
- **The Claim Decomposer** strips the claim to its irreducible commitments
- **The Alternative Generator** asks what else could produce this output
- **The Naive Verifier** reads only what is on the page
- **The Implication Tester** traces what would happen if someone acted on the claim

Each investigator runs a three-candidate Adversarial Perspective Forcing protocol and selects the third (a hostile peer reviewer's analysis of the first two), forcing non-obvious findings.

Five anonymized peer reviewers then evaluate the investigator outputs against four criteria: diagnostic precision, verification gap, convergence signal, and collective epistemic gap.

Four fixed Phase 2 lenses follow: Semantic Entropy Test, Chain-of-Verification Pass, Calibration Check, and Source Grounding. These are research-backed structured tests, not analytical perspectives.

A chairman synthesizes everything into the Reason Verdict above, under 400 words, with a real risk level and one mandatory external verification step.

The full protocol, with prompt templates, is in `SKILL.md`.

---

## known limitations

Read these honestly. The skill is more useful when its limits are understood.

**Self-verification loop.** The CoVe and Semantic Entropy approximations operate within the same model that generated the original claim. This is mitigated but not eliminated by independence constraints, by Adversarial Perspective Forcing, and by the mandatory external verification step. The skill reduces the probability of acting on confabulations; it does not reduce it to zero.

**Adversarial Perspective Forcing is a prompt-level mechanism, not probabilistic sampling.** The original Verbalized Sampling research accessed the model's output distribution directly. Candidate 3 here is selected by enforced perspective adversarialism. It reliably surfaces less obvious findings; it does not guarantee they represent the tail of the probability distribution.

**The counterfactual is built into first use, not pre-validated.** This skill has not been systematically compared against simpler alternatives in a controlled study. The simpler two-prompt alternative above is provided so each adopter can run the comparison on their own claims and calibrate. If the full protocol does not produce meaningfully better signal than the simpler version on your claim types, default to the simpler version. This is by design.

**Structured verdicts can increase overtrust.** A formal risk rating with a structured layout produces more user confidence than an informal analysis, even when both rest on the same foundation. The mandatory external verification step is the architectural defense against this. If a verdict is read and acted on without running that step, the protocol has failed regardless of what the verdict said.

---

## relationship to the llm-council

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

Run the llm-council first if you are deciding. Run the reason-council on the claims that decision depends on.

---

## sources

- Farquhar, S., Kossen, J., Kuhn, L., Gal, Y. "Detecting Hallucinations in Large Language Models Using Semantic Entropy." *Nature*, 2024.
- Dhuliawala, S. et al. "Chain-of-Verification Reduces Hallucination in Large Language Models." *ACL Findings*, 2024.
- KalshiBench. "Do Large Language Models Know What They Don't Know? Evaluating Epistemic Calibration via Prediction Markets." 2025.
- Zhang, T. et al. "Verbalized Sampling." Stanford, 2024.
- Comprehensive survey literature on LLM hallucination detection and mitigation, 2024 to 2025.

---

## contributing

The skill is shared as-is, with the explicit invitation to test it against simpler alternatives on your own claims and calibrate. If you find the full protocol does not earn its overhead in your context, that is useful information; an issue or pull request reporting it is welcome. If you find a structural failure mode beyond the four already documented, that is more useful still.

## license

[choose one and add: MIT, Apache 2.0, CC BY 4.0]
