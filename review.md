# Peer Review: A Human-in-the-Loop LLM Workflow for Research Roadmap Curation

**Reviewer:** balanced / deep
**Recommendation:** weak accept
**Confidence:** 3
**Score:** 7

## Summary of contributions

The authors present a workflow for early-stage research planning in which an LLM drafts a structured, twelve-component roadmap conditioned on a fixed schema, the roadmap is materialized as a compiled PDF, and a human reviewer approves or requests revisions before the roadmap is used. The contribution is positioned as a system-and-protocol pair: the workflow itself, the schema that constrains generation, and a four-way anonymized human preference study with N=40 evaluators. The headline finding is 34 of 40 (85%) overall preference for the schema-constrained, human-reviewed variant over three unstructured ChatGPT baselines, with the chance hypothesis rejected at p ≈ 2.5 × 10^-15.

## Strengths

1. **Clear, narrow framing.** The paper is admirably focused: schema-constrained generation under explicit human review, not autonomous research. The introduction states this position cleanly and the conclusion sticks to it.
2. **The schema is concrete and reusable.** Figure 2 enumerates twelve components grouped into intellectual, evaluation, and execution structure. This is a real artifact a reader can adopt.
3. **Statistical reporting added.** The revised manuscript now includes the binomial-tail p-value (≈ 2.5 × 10^-15) immediately after the headline 34/40 figure, which forecloses the most obvious reviewer objection to a forced-choice study with N=40.
4. **Limitations are explicit and complete.** Section 6 of the Results page now enumerates three reporting limitations: per-dimension tallies absent, per-baseline counts collapsed, cohort skew. This is rare and increases trust.
5. **Methodology figures are well-crafted.** The workflow diagram, schema, asynchronous job-state machine, and evaluation protocol are visually consistent, legible at workshop sizing, and each carries its own caption-level takeaway.
6. **Section flow reads naturally.** Sections move forward without back-references; the narrative arc from motivation → workflow → protocol → result → failure modes → limitations is cleanly executed.

## Weaknesses

1. **Per-dimension breakdown still absent (severity: medium).** The protocol designs for five quality dimensions (completeness, feasibility, specificity, experiment design, research potential), but the paper still reports only the aggregate single-roadmap preference. The authors have correctly flagged this in the revised Limitations subsection, but flagging is not a substitute for reporting. A workshop reviewer's first follow-up question is: "did Roadmap C win on every dimension or only on overall feel?"
2. **Per-baseline counts (A vs. B vs. D) still collapsed (severity: medium).** The 6 non-C votes are reported as a combined total. This is honestly disclosed, but it makes the model-agnostic claim weaker: we cannot tell whether one ChatGPT version was a stronger competitor than the others.
3. **Operational metrics still deferred (severity: low).** Section 4.4 promises generation time, approval rate, revision-request count from the asynchronous backend logs, but the paper reports none of them. Even pilot numbers from the prototype run would substantially strengthen the systems contribution.
4. **Distinguishing paragraph in Related Work is generic (severity: low).** The paragraph that contrasts this work with autonomous-AI-scientist systems lists three references but does not commit to a specific point of disagreement (e.g., what an alternative system would output for the same input prompt that this workflow would not).

## Specific comments

- **Section 5.2, Table 1.** The two-row "A, B, D combined" table is a coarse summary of a four-way protocol. If the underlying response-by-response data has been lost, the paper should say so explicitly rather than leaving the impression that the survey instrument was the only obstacle.
- **Section 6.2.** The new binomial-test statement is well placed. Consider also reporting a 95% Wilson confidence interval on the 0.85 proportion; that gives readers a feel for sampling variability without requiring an extra paragraph.
- **Figure 5.** The two-bar chart restates Table 1 without adding information. If per-baseline counts cannot be recovered, consider either removing this figure (the table alone suffices) or replacing it with a cumulative-fraction comparison once per-dimension data is available.
- **Section 4.2.** The description of the survey instrument is short on operational detail (multiple-choice radio, Likert, free-text only, time-to-complete). One short paragraph on the instrument would help replication.
- **Section 7 (Failure Modes).** This section is a strength of the paper but currently reads as prose rather than empirical reporting. Anchoring each failure mode to a specific revision request observed in the prototype logs would convert it from anecdote to evidence.

## Recommendation justification

This is a well-scoped workshop submission that improved meaningfully between iterations: the binomial-test statistic now closes the most common reviewer objection to a forced-choice study, and the Limitations subsection is now explicit about what was and was not reported. The framing is appropriately humble. The remaining weaknesses are not about the framing or the workflow; they are about reporting completeness of the empirical evaluation. Two of those (per-dimension and per-baseline tallies) are flagged transparently as not-reported rather than hidden, which a fair workshop reviewer should weight in favor of the paper. I recommend weak accept at a planning- or human-AI-collaboration-themed workshop, with the per-dimension breakdown understood to be the first item on a revision pass if accepted.

## Minor issues

- Page 1: ligature substitution is on (e.g., "ﬂow", "diﬃcult"); this is intentional with `lmodern` and fine, but worth confirming for the target venue's house style.
- The author asterisk and dagger marks could use a one-line legend on page 1.
- The bibliography mixes capitalization styles in some titles ("Towards" vs. "towards"); a single bibtex post-processing pass would normalize.
