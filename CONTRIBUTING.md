# Contributing to Reason Council

Contributions are welcome. This is a Claude skill built on research-backed 
epistemic evaluation methods — improvements should stay grounded in that spirit.

## What you can contribute

- **Improvements to the investigator prompts** — if you find a formulation 
  that produces more diagnostic epistemic findings, open a pull request with 
  before/after examples
- **New Phase 2 lenses** — additional research-backed epistemic tests with 
  citations to the underlying research
- **Documented examples** — real cases where the reason council caught a 
  confabulation or correctly cleared a claim. Add them to an `examples/` folder
- **Translations** — adaptations of the skill for non-English contexts
- **Bug reports** — if a trigger phrase fails to activate the skill or the 
  verdict format breaks, open an issue

## What to avoid

- Changes that remove the research citations — the epistemic grounding is the point
- Replacing the fixed Phase 2 lenses with customizable ones (that is the 
  llm-council's design; reason-council lenses are fixed by design)
- Simplifications that reduce the skill to a single-pass evaluation

## How to submit

1. Fork the repository
2. Make your changes
3. Open a pull request with a clear description of what changed and why
4. If you are proposing a significant structural change, open an issue first 
   to discuss before writing anything

## Questions

Open an issue and tag it `question`.
