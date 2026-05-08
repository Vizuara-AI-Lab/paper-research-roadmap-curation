# Student Context — session 20260508-045419-5471

## Research domain

Human in the Loop LLM Workflow for research project roadmap curation

## Video transcripts

(none provided)

## Code repositories

(none provided)

## Free-form notes

### Raw or partial results (from Configure tab)

We conducted survey amongst 40 people with our project roadamp and ai genrated project roadmpa with three different chatgt version and labeeled it as A , B, C, D and cnaidates wehre asked to rate it based on clarity , reporducr results, flow etc and then 34 people recomemend C over all C is our one

### Existing paper draft

Abstract

Early-stage research often begins with vague ideas that are not yet executable. A student or researcher may know the broad area they want to explore, but converting that interest into a concrete project requires research questions, literature directions, datasets, baselines, evaluation metrics, ablations, milestones, risks, and a realistic contribution claim. While large language models can assist with this planning stage, unstructured prompting often produces roadmaps that are fluent but generic, difficult to verify, or poorly scoped. We present a human-in-the-loop LLM workflow for research roadmap curation. Given a raw research idea, the workflow generates a structured roadmap artifact that can be reviewed, revised, and approved by a human reviewer before use. The reviewer may be the researcher themselves, a mentor, instructor, supervisor, collaborator, or domain expert. The workflow is model-agnostic and can be implemented using different LLM backends; our prototype uses an asynchronous generation pipeline that conditions on reference roadmap templates, optionally retrieves relevant literature, generates LaTeX source, compiles a PDF, and exposes the result for review. We position the LLM as a research-planning assistant rather than an autonomous researcher. We propose a human preference study comparing multiple roadmap variants generated for the same research idea across five dimensions: completeness, feasibility, specificity, experiment design quality, and research potential.

1. Introduction

Early-stage research projects often fail before experimentation begins. The initial idea may be too broad, the dataset may be unclear, the baselines may be missing, the evaluation metrics may not match the research question, or the expected contribution may be overclaimed. This problem is especially common in research bootcamps, undergraduate research programs, industry training cohorts, and early-stage graduate projects, where participants often begin with enthusiasm but limited experience in converting a broad interest into an executable research plan.

A vague idea is not yet a research project. For example, ideas such as “I want to work on multimodal RAG,” “I want to apply reinforcement learning to retrieval,” or “I want to use scientific machine learning for physical systems” are useful starting points, but they do not specify what should be built, what should be compared, what evidence should be collected, or what claim the final paper or report could defend. To become actionable, such ideas must be transformed into structured roadmaps containing research questions, literature directions, datasets, baselines, metrics, ablations, weekly milestones, risks, deliverables, and a minimum viable claim.

This transformation is usually performed by human mentors, instructors, supervisors, or domain experts. However, human research planning does not scale easily. In a cohort with many students, mentors may need to repeatedly convert loosely stated interests into detailed project plans. The bottleneck is not merely writing a document. The harder task is producing a roadmap that is technically specific, feasible within the available time, experimentally sound, and appropriately scoped.

Large language models offer a natural opportunity to support this stage of research planning. However, one-shot prompting is often insufficient. A generic prompt such as “generate a 12-week research roadmap for this idea” can produce fluent output, but the resulting roadmap may omit baselines, suggest unrealistic datasets, fail to define measurable success criteria, or overstate the novelty of the project. The surface quality of LLM-generated text can also make weak plans appear more credible than they are.

We present a human-in-the-loop LLM workflow for research roadmap curation. Given a raw research idea, the workflow generates a structured roadmap artifact. The roadmap is not treated as final. Instead, it is routed to a human reviewer who can inspect it, request revisions, or approve it for use. The reviewer may be the original researcher, a mentor, an instructor, a supervisor, a collaborator, or a domain expert. This design keeps the human responsible for feasibility, novelty calibration, and scientific correctness, while using the LLM to reduce the burden of first-pass structuring.

Our workflow is model-agnostic. It can be instantiated using different LLM backends, including API-based models, coding agents, or local models, as long as the backend can perform structured generation and artifact creation. In our prototype, roadmap requests are processed asynchronously: a request is stored as a job, an LLM worker generates a LaTeX roadmap using reference templates and optional literature search, the PDF is compiled, and the result is shown to the reviewer for approval or revision.

This paper studies research roadmap generation not as autonomous research generation, but as a practical human-AI workflow for early-stage research planning. Our central question is:

Can a structured, human-reviewed LLM workflow help convert vague research ideas into executable research roadmaps more reliably than unstructured prompting?

We make three contributions. First, we formulate research roadmap generation as a human-in-the-loop workflow rather than a one-shot generation task. Second, we introduce a structured roadmap schema that decomposes early-stage research planning into concrete components such as questions, datasets, baselines, metrics, ablations, risks, milestones, and minimum viable claims. Third, we describe an evaluation protocol in which human reviewers compare multiple anonymized roadmap variants for the same research idea across five quality dimensions.

2. Research Roadmap Curation as a Workflow

We define a research roadmap as an execution-oriented planning artifact that transforms a broad research idea into a concrete project plan. A good roadmap should help a researcher understand what to read, what to implement, what to compare, how to evaluate progress, and what claim the final project may support.

Unlike a literature review or a project proposal, a roadmap is explicitly operational. It should include both intellectual structure and execution structure. It should tell the researcher not only what the problem is, but also what needs to happen in Week 1, Week 2, and so on.

2.1 Input

The workflow begins with a raw research idea. The input may include:

project topic,
short description,
target duration,
researcher background,
preferred domain,
constraints on tools or datasets,
target output such as paper, report, codebase, or presentation.

The input does not need to be fully specified. In fact, the workflow is designed for the common case where the idea is incomplete.

2.2 Roadmap Schema

The LLM is not asked to freely brainstorm. Instead, it is guided by a roadmap schema. Each generated roadmap is expected to include:

Component	Purpose
Research scope	Defines the boundary of the project
Research questions	Converts a broad idea into testable questions
Literature directions	Identifies relevant prior work and reading areas
Dataset or environment plan	Specifies what evidence the project will use
Methodology	Describes the technical approach
Baselines	Prevents weak or unfair comparisons
Metrics	Defines how success will be measured
Ablations	Tests which components matter
Milestones	Converts the project into weekly execution steps
Risks and mitigations	Makes likely failure modes explicit
Deliverables	Defines expected artifacts
Minimum viable claim	Prevents overclaiming and keeps the project realistic

This schema is intended to make roadmap generation less dependent on surface fluency. A roadmap is not considered strong merely because it reads well. It must specify how the project can actually be executed and evaluated.

3. Human-in-the-Loop LLM Workflow

The central design choice is to separate generation from approval. The LLM generates a first draft, but a human reviewer decides whether the roadmap is usable.

3.1 Workflow Overview

The workflow has five stages:

Idea submission: A user submits a raw research idea and optional constraints.
Structured generation: An LLM backend generates a roadmap using a fixed schema and reference examples.
Artifact creation: The roadmap is converted into a reviewable artifact, such as a LaTeX/PDF document.
Human review: A reviewer inspects the roadmap and either approves it or requests changes.
Revision or delivery: If revisions are requested, the workflow regenerates or edits the roadmap using reviewer feedback. If approved, the roadmap is delivered or used.

The human reviewer is not only the person who writes the initial prompt. The human remains involved after generation, when quality judgments are required. This makes the workflow human-in-the-loop rather than merely human-initiated.

3.2 Model-Agnostic Backend

The workflow is independent of any single LLM provider. The backend can be implemented with different models or agents, provided that they support structured generation and optional tool use.

Our prototype uses an asynchronous worker architecture. A roadmap request is stored as a job, processed by a worker, converted into LaTeX source, compiled into a PDF, and exposed to the reviewer. The system design supports states such as queued, processing, completed, failed, revision requested, and revision processing. This makes roadmap generation robust to long-running generation and allows the reviewer to request changes after inspecting the output.

The job queue is not the scientific contribution by itself. Rather, it is an implementation mechanism that enables the research workflow to behave like a real review-and-revision system instead of a one-shot chat interaction.

3.3 Human Review

Human review is necessary because roadmap quality depends on judgments that LLMs cannot reliably make alone. A roadmap may appear polished while still being unrealistic, too broad, weakly evaluated, or scientifically shallow. The reviewer checks whether:

the research question is precise,
the proposed dataset is accessible,
the baselines are appropriate,
the metrics are measurable,
the milestones are realistic,
the risks are honestly stated,
the minimum viable claim is not overconfident.

If the roadmap fails these checks, the reviewer can request revision. This feedback becomes part of the next generation or editing cycle.

The reviewer may be a student using the tool independently, a faculty advisor, a bootcamp mentor, a teaching assistant, a lab lead, or a domain expert. The core requirement is not a specific role, but a decision point where a human accepts responsibility for the final plan.

4. Evaluation Protocol

We propose a human preference study to evaluate roadmap quality. The study compares multiple roadmap variants generated for the same research idea and same input prompt. This controls for topic preference and focuses the evaluation on roadmap quality.

4.1 Study Setup

For each research idea, we generate four anonymized roadmap versions, labeled Roadmap A, Roadmap B, Roadmap C, and Roadmap D. All four roadmaps correspond to the same topic and are generated from the same prompt or input description. Human evaluators are asked to compare the roadmaps across five quality dimensions.

The five dimensions are:

Completeness
Feasibility
Specificity / Actionability
Experiment Design Quality
Research Potential

For each dimension, the evaluator selects one roadmap out of the four.

4.2 Survey Questions

The survey asks:

Completeness. Which roadmap is the most complete?
A complete roadmap should include research questions, methodology, datasets, baselines, metrics, milestones, risks, and deliverables.

Feasibility. Which roadmap is the most realistic to execute within the proposed timeline?
A feasible roadmap should have manageable scope, accessible datasets or tools, realistic milestones, and achievable deliverables.

Specificity / Actionability. Which roadmap gives the clearest step-by-step guidance for actually starting and executing the project?
An actionable roadmap should make it clear what to read, build, test, compare, and submit.

Experiment Design Quality. Which roadmap has the strongest experimental plan?
A strong experimental plan should include suitable baselines, measurable metrics, ablations, validation strategy, and clear success criteria.

Research Potential. Which roadmap has the strongest potential to become a good research paper, workshop submission, or technical report?
A strong roadmap should have a clear problem, plausible novelty, meaningful evaluation, and a realistic minimum viable claim.

We also collect free-text feedback asking evaluators why they chose the strongest roadmap, which roadmap was weakest, and what improvements they would make before using the roadmap.

4.3 Analysis

The primary outcome is the number of votes received by each roadmap variant for each quality dimension. This allows us to measure whether a particular roadmap-generation variant is consistently preferred for completeness, feasibility, actionability, experiment design, or research potential.

We also analyze qualitative feedback to identify recurring failure modes. For example, evaluators may prefer one roadmap because it has better baselines, but reject another because it is too ambitious or vague. These comments help distinguish surface-level polish from actual research usefulness.

If the workflow logs are available, we also report operational metrics:

generation time,
number of revision requests,
time to approval,
approval rate,
common revision categories,
failure cases during generation or compilation.

These metrics connect subjective human preferences to practical workflow behavior.

5. Expected Findings

We expect structured roadmap generation to outperform unstructured prompting on completeness and specificity. A schema-constrained roadmap is more likely to include baselines, metrics, ablations, risks, and deliverables because these components are explicitly required.

However, we do not expect LLM-generated roadmaps to be reliable without human review. Human revision is likely to be most important for feasibility, novelty calibration, and domain correctness. For example, an LLM may recommend a dataset that is technically relevant but difficult to access, or it may propose a contribution that sounds publishable but is too incremental or too broad.

Thus, the expected result is not that LLMs can replace research mentors or supervisors. The expected result is that structured LLM workflows can reduce first-pass planning effort while preserving human oversight where it matters most.

6. Failure Modes

A human-in-the-loop workflow is necessary because roadmap generation has several failure modes.

First, the LLM may overclaim novelty. It may describe a project as publishable even when the proposed contribution is only a standard application of known methods.

Second, the LLM may recommend unrealistic timelines. A roadmap may appear well structured but require more data collection, engineering, or experimentation than can fit within the proposed duration.

Third, the roadmap may include weak or incomplete baselines. This is a serious issue because baselines determine whether the final research claim is meaningful.

Fourth, the LLM may suggest datasets, APIs, or tools that are difficult to access, poorly maintained, or unsuitable for the student’s skill level.

Fifth, the roadmap may be too template-driven. If every roadmap follows the same structure too rigidly, it may miss domain-specific requirements.

Finally, the roadmap may be fluent but not actionable. This is the most subtle failure: the document sounds professional, but does not tell the researcher exactly what to do next.

The review loop is designed to reduce these risks. A human reviewer can catch unrealistic scope, missing comparisons, weak evaluation plans, and overconfident claims before the roadmap is used.

7. Discussion

Research roadmap curation sits between brainstorming and execution. It is not the same as idea generation, literature review, code generation, or paper writing. Instead, it is the stage where an idea becomes operational.

This distinction matters. Many discussions of LLMs in research focus on whether models can write papers, generate hypotheses, or act as autonomous scientists. Our framing is narrower and more practical. We do not claim that an LLM can determine scientific truth or independently supervise research. We claim that an LLM, when constrained by a schema and placed inside a human review loop, can help produce better first-pass research plans.

The model-agnostic nature of the workflow is also important. The contribution is not tied to a specific backend. Different bootcamps, labs, or research programs can instantiate the same workflow using different LLMs, templates, review policies, or output formats. What matters is the structure: schema-constrained generation, reviewable artifacts, and explicit human approval.

8. Limitations

This work has several limitations. First, human preference studies measure perceived roadmap quality, not long-term project success. A roadmap that reviewers prefer may not necessarily lead to a stronger final paper.

Second, the evaluation depends on the expertise of reviewers. Beginners may prefer detailed roadmaps that experts find unrealistic, while experts may penalize roadmaps for missing domain-specific subtleties.

Third, the same roadmap schema may not work equally well across domains. A roadmap for reinforcement learning, scientific machine learning, multimodal RAG, or AI systems may require different levels of detail.

Fourth, the workflow may inherit biases from reference templates. If the examples emphasize certain styles of projects, the generated roadmaps may overproduce similar structures.

Fifth, the current study focuses on roadmap quality at generation time. Future work should track downstream outcomes such as whether students complete milestones faster, produce better experiments, or require fewer mentor interventions.

9. Conclusion

We presented a human-in-the-loop LLM workflow for research roadmap curation. The workflow converts vague research ideas into structured, reviewable execution plans containing research questions, literature directions, datasets, baselines, metrics, ablations, risks, milestones, deliverables, and minimum viable claims. Unlike one-shot prompting, the workflow separates generation from approval: the LLM drafts the roadmap, while a human reviewer remains responsible for feasibility, novelty calibration, and scientific correctness.

We argue that the value of LLMs in early-stage research planning is not autonomous research generation, but structured assistance under human oversight. By combining schema-constrained generation, model-agnostic backends, reviewable artifacts, and revision loops, research roadmap workflows can make early-stage research planning more scalable, consistent, and auditable.

### Other notes

(none)
