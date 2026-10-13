---
name: lab-experiment-report
description: "Run an end-to-end lab assignment workflow: identify assignment and report documents in the current workspace, plan and implement or reuse code, verify actual results, then fill the confirmed original DOCX report in place. Use only when the user explicitly invokes $lab-experiment-report for a new lab assignment; do not use for ordinary document editing or code-only tasks."
---

# Lab Experiment Report

Use this skill only after explicit invocation. It performs an end-to-end experiment workflow and replaces one confirmed report DOCX only after a workspace-local working copy passes the required checks, so target identification and stage gates are mandatory.

Before acting, read [references/report-workflow.md](references/report-workflow.md) completely and follow it as the execution contract.

## Fixed User Rules

- Copy the assignment's experiment-purpose text into the report verbatim. Do not rewrite, summarize, correct, normalize, or re-punctuate it. If the report already supplies the section heading, copy only the purpose body and do not duplicate the heading.
- Set the experiment location to `实训楼508`.
- In newly written Chinese prose, do not insert spaces between Chinese characters and adjacent English words, Latin letters, or digits. Preserve code, formulas, paths, URLs, identifiers, and verbatim source text exactly.
- Generate and retain the code and result artifacts needed for verification. Deliver only the artifacts required by the assignment or the user's stated submission set; when the user specifies that only the report DOCX and source code are submitted, do not present internal JSON, CSV, image, animation, render, or QA paths as submission materials. Do not create a ZIP archive unless the user explicitly overrides this rule in the current request.
- Deliver the completed report at the confirmed original DOCX path. Keep the single working DOCX and QA artifacts under `.lab-work/lab-experiment-report` inside the current workspace, then remove only that task-owned directory after a successful replacement. Do not create a user-visible report copy, sibling backup, or renamed final version.

## Stage Gates

1. Inventory the workspace read-only and classify assignment sources, report target, reference reports, existing code, and existing results.
2. If more than one assignment or report candidate remains plausible, ask the user to choose before writing. A report lock does not block code work, but the user must close the report before the final replacement.
3. Read the entire assignment and produce a concise execution plan with observable acceptance criteria. Inspect and reuse correct existing code instead of rewriting it automatically.
4. Implement and run the experiment. Preserve compliant code, but create or modify the portions of experiment code and results that do not yet satisfy the assignment. Do not begin report authoring until the code and required outputs pass the assignment-derived checks.
5. Run the evidence-enhancement gate in the reference workflow. Add only evidence that the assignment or report needs and that can be produced by a controlled rerun: parameter or hyperparameter comparisons, a static view of a dynamic optimization process, a compact metrics table or confusion matrix, and method formulas or explanations. Do not infer an optimum or copy facts from a reference report.
6. Copy the confirmed report to `.lab-work/lab-experiment-report/report-working.docx`, fill that working DOCX using only verified outputs and the final source code, and preserve the template features required by the assignment.
7. Perform the focused structural checks and render the working DOCX once. Fix and rerender only when an issue is found. Then atomically replace the original report, reopen it once, and remove only the task-owned working directory after success.

Explicit invocation authorizes creating experiment outputs and modifying existing experiment code only where needed to satisfy the current assignment. It also authorizes replacing the unambiguously identified report target after the code and report gates pass. It never authorizes modification of the assignment source, reference reports, unrelated files, or other student data.

## Evidence and language rules

- For a controlled comparison, keep the data split, random seed, model, and evaluation procedure fixed; change one parameter at a time and record the actual result. State that a setting is better or optimal only when the measured comparison supports that claim.
- If the assignment requires a dynamic optimization process, make it inspectable in the report with an embedded trajectory figure or representative frames. Do not rely on a path to an unsubmitted animation as the only evidence.
- Prefer a compact table for key metrics and a confusion matrix or per-class metrics when they materially improve grading or interpretation. Add formulas when code alone does not make the method auditable.
- Match newly written source comments, docstrings, and report prose to the user's requested language or the report language. This does not change identifiers, APIs, formulas, commands, paths, or verbatim assignment text.
- Distinguish internal verification artifacts from submitted artifacts throughout the report and final handoff. Embed necessary evidence and omit internal paths unless the assignment explicitly requires them.

## Completion Status

Report one of these outcomes truthfully:

- `实验与报告已完成并通过验收`
- `代码或结果未通过，报告未修改`
- `报告工作副本未通过验收，原报告未修改`
- `报告提交异常，需检查原文件`

List the actual submitted paths first, followed by any retained result paths only when the assignment or user requested them. Do not present temporary renders, extracted assets, caches, or internal QA files as deliverables. If cleanup is blocked after a successful replacement, preserve the files, state the exact residual path, and keep it out of the submission list.
