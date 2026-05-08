# A Human-in-the-Loop LLM Workflow for Research Roadmap Curation

A schema-constrained, human-reviewed workflow that converts vague research ideas into executable research roadmaps. Given a raw research idea, the workflow generates a structured artifact, materializes it as a reviewable LaTeX/PDF document, and routes it to a human reviewer who approves or requests revisions before the roadmap is used.

## Paper

- **PDF**: [tex/main.pdf](tex/main.pdf)
- **Source**: [tex/main.tex](tex/main.tex) — compile with `tectonic -X compile tex/main.tex`
- **Bibliography**: [tex/references.bib](tex/references.bib)

## Primary result

**Roadmap C (our schema-constrained, human-in-the-loop workflow) was selected as the strongest overall by 34 of 40 evaluators (85%) in a four-way anonymized preference study.** The remaining 6 votes (15%) were distributed across three unstructured-prompting baselines (Roadmaps A, B, D), each from a different general-purpose ChatGPT version. Under the null hypothesis of uniform preference, the binomial-tail probability of observing ≥34/40 votes for any single variant is p ≈ 2.5 × 10⁻¹⁵; the 95% Wilson confidence interval on the underlying preference proportion is [0.709, 0.929].

## How to reproduce the survey analysis

This workshop-tier paper reports an aggregate human preference number; the survey instrument and raw evaluator responses live with the authors. The statistical machinery shown in the paper (binomial tail, Wilson CI) is computed from the public count `(N=40, k=34, p_chance=0.25)` and is reproducible from any standard statistics library:

```python
from math import comb, sqrt
N, k, p = 40, 34, 0.25
print("p-value:", sum(comb(N, j) * p**j * (1-p)**(N-j) for j in range(k, N+1)))

# Wilson 95% CI
phat, z = k/N, 1.96
denom = 1 + z**2/N
center = (phat + z**2/(2*N)) / denom
margin = z * sqrt(phat*(1-phat)/N + z**2/(4*N**2)) / denom
print("Wilson 95% CI:", (center - margin, center + margin))
```

## Figures

| Figure | Description |
| --- | --- |
| [fig-workflow-overview](figures/fig-workflow-overview.png) | End-to-end human-in-the-loop workflow: idea submission → structured generation → artifact creation → human review → delivery / revision. |
| [fig-roadmap-schema](figures/fig-roadmap-schema.png) | Twelve-component roadmap schema, grouped into intellectual / evaluation / execution structure. |
| [fig-async-pipeline](figures/fig-async-pipeline.png) | Asynchronous job state machine: queued → processing → completed → revision_requested → revision_processing. |
| [fig-evaluation-protocol](figures/fig-evaluation-protocol.png) | Anonymized four-roadmap evaluation protocol: same idea, same prompt, four variants, five quality dimensions. |
| [fig-main-results](figures/fig-main-results.png) | Overall preference: 34/40 (85%) for Roadmap C vs. 6/40 (15%) for combined baselines (A, B, D). |

## Recommended venues

- **CHI Late-Breaking Work** — top venue for human-AI interaction; LBW track fits workshop-tier evidence.
- **NeurIPS Workshops** (Human-AI Collaboration / AI for Education / Foundation Models for Decision Making) — schema-constrained generation under human review fits.
- **In2Writing (ACL/EMNLP/CHI workshop on Intelligent and Interactive Writing Assistants)** — direct topical fit.
- **TMLR** — rolling submission, evaluates on correctness rather than novelty-above-SOTA.
- **AI Magazine** — accessible report style for human-AI workflow systems.

## Authors

Prathamesh Joshi, Abraar, Naman Dwivedi, Dr Raj Dandekar, Dr Rajat Dandekar, Dr Sreedath Dandekar.

## Peer review

A sealed-PDF auto-review (balanced reviewer, deep depth) is included at [review.md](review.md). Score: 7/10, recommendation: weak accept.

## Provenance

Session id: `20260508-045419-5471`. See [log.md](log.md) and [state.json](state.json) for the per-stage audit trail and full session state.
