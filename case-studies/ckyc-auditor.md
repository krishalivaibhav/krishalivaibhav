# A KYC screener that raises 38% fewer alerts and catches more real matches

**Project:** [ckyc-auditor](https://github.com/krishalivaibhav/ckyc-auditor), a five-agent system that continuously screens a bank's customer book against sanctions lists and adverse media.
**Context:** built by a team of five for Code by Tech Mahindra (top 6 of about 30,000 registrants). I owned the entity-resolution stage and the data contract every agent shares.
**Result:** against a name-matching baseline on a synthetic 60,000-customer book, alerts fell from 474 to 293 while precision rose from 0.169 to 0.352 and recall from 0.66 to 0.84.

## The problem

Screening customers by name floods a compliance team. Thousands of people share a name with someone on a sanctions list, and every alert costs an analyst's time. Tightening the name match hides real hits along with the false ones.

## What I built

**A deterministic ladder instead of a model.** The stage that decides "is this customer the listed person?" uses hard identifiers, not judgement:

1. If the PAN (Indian tax ID) matches exactly, it is the same person.
2. If the PAN types are incompatible, or the PANs differ, it is not, and the alert is suppressed.
3. Only cases with no usable identifier go on to fuzzy and phonetic name matching.

Every suppression is recorded with its reason, so an auditor can see why an alert was not raised.

**LLMs only where judgement is needed.** A language model triages news and adjudicates the investigation step. Each LLM step has a deterministic fallback, so a failed API key degrades the output instead of stopping the pipeline.

**One frozen data contract.** All five stages exchange the same typed Pydantic models. Five people could build in parallel without the pieces drifting apart.

**Evidence that stays honest.** Each claim in a drafted Suspicious Activity Report cites its evidence, labelled confirmed, correlated or missing. Claims that cannot be verified are left out and the omission is disclosed.

## Results

| | Name-matching baseline | With the ladder |
|---|---|---|
| Alerts raised | 474 | 293 |
| Precision | 0.169 | 0.352 |
| Recall | 0.66 | 0.84 |

The comparison comes from the evaluation script in the repository, run on generated data.

## Limits

- The customer book and ground truth are synthetic. Real data would shift these numbers.
- A precision of 0.35 still means most alerts are false. The ladder halves the noise; it does not remove it.
- This is a hackathon prototype: no authentication, a single SQLite database, and news inputs that default to a bundled sample.
