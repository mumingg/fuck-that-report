# Experiment-to-report execution contract

This reference defines the required behavior after `$lab-experiment-report` is explicitly invoked. The workflow is domain-neutral: derive the language, framework, algorithm, data, metrics, and artifacts from the current assignment instead of carrying assumptions from an earlier experiment.

## 1. Preflight and role classification

Start with a read-only inventory of the current workspace. Include common assignment formats (`.docx`, `.pdf`, `.md`, `.txt`), report DOCX files, source code, notebooks, data, generated results, environment manifests, and hidden Word lock files.

Classify documents using both content and structure:

- **Assignment source:** usually contains task statements, experiment purpose, required environment or method, parameters, expected outputs, grading criteria, visualization requirements, or submission instructions.
- **Report target:** a DOCX with report cover fields, institutional branding, experiment metadata, score fields, and body sections intended for the current student. A filename alone is insufficient evidence.
- **Reference report:** a completed report carrying another student's identity, old code, old metrics, or old conclusions. It may guide organization or formatting but is never an editable target or factual source.
- **Lock file:** any `~$*.docx` artifact. Treat a matching or plausibly related lock file as evidence that Word may have the report open.

Record candidate paths, reasons, and confidence. Exclude lock files from deliverables. Never choose by modification time alone. If two candidates remain reasonable for either the assignment source or report target, stop and ask the user to select the exact path.

Before report authoring:

- Confirm the exact target path again.
- Confirm it is not the assignment source or a reference report.
- Record the target's path, initial SHA-256, and core structural facts once. Recheck the path, fingerprint, and lock immediately before the final replacement so concurrent user edits are not overwritten.
- Keep all task-created working files, extracted assets, and renders under `.lab-work/lab-experiment-report` inside the current workspace. Do not create task artifacts in system temporary directories or elsewhere outside the workspace.

Use the installed document or PDF workflow appropriate to each source format. Read DOCX tables, headers, footers, text boxes, fields, content controls, relationships, and images when they carry relevant requirements; `Document.paragraphs` alone is not a complete read.

## 2. Assignment extraction and planning

Read the complete assignment before changing code. Build a compact requirements matrix covering:

- the exact experiment-purpose text and its source boundaries;
- required tasks, algorithms, formulas, parameters, data, and environment;
- required interfaces or commands;
- required outputs, figures, tables, animations, metrics, and thresholds;
- report structure, identity fields, submission requirements, and prohibited approaches;
- ambiguities that cannot be resolved from the workspace.

### Verbatim experiment purpose

Identify the body governed by the assignment's `实验目的` heading, stopping at the next peer heading. Preserve the extracted Unicode text exactly, including numbering, punctuation, capitalization, spaces, symbols, and paragraph order. Do not silently repair typos or restyle the wording. If OCR is required and any character is uncertain, ask the user before treating it as verbatim.

When the target report already contains its own `一、实验目的` or equivalent heading, insert the extracted body below that heading. Do not duplicate the heading or an automatic list label. At QA time, compare the visible purpose body with the captured source text exactly, allowing only the technical normalization of line-ending representations. Do not normalize visible spaces, punctuation, capitalization, or full-width and half-width characters. If Word fields, text boxes, or automatic numbering make the visible boundary uncertain, ask the user instead of guessing.

Produce a concise execution plan and acceptance checklist before implementation. Do not pause for approval when the plan follows an unambiguous assignment and existing user instructions. Ask only when a missing choice would materially change the experiment or report.

## 3. Existing code and implementation

Inspect existing source, notebooks, requirements, data, and results before writing. Determine what is active and correct. Preserve compliant implementation and user changes; create or modify only the portions that prevent the current experiment from satisfying the assignment. Do not perform unrelated refactors or delete existing code or data unless the assignment or user explicitly requires it.

Implementation rules:

- Use the assignment-specified language, framework, algorithm, and interface. When unspecified, choose the simplest reproducible approach supported by the available environment.
- Do not claim use of packages, hardware, models, data, or methods that were not actually used.
- Keep paths and commands portable within the submitted workspace when practical.
- Generate report code excerpts or screenshots from the final source after the last functional change.
- Do not copy another student's source, metrics, results, or conclusions from a reference report.
- Before selecting an interpreter, check the available Python environments and the packages required by the assignment. If the bundled runtime lacks a required package, use an available compatible environment, record the actual interpreter and package versions, and run the experiment there. Do not claim the bundled runtime was used when it was not.

Before report editing, execute the relevant checks:

- the documented run command succeeds from the intended environment;
- every required artifact exists, is nonempty, and opens or parses correctly;
- calculated metrics meet assignment thresholds or are reported honestly when no threshold is given;
- one normal run completes successfully; repeat a run only when the assignment requires reproducibility or the observed output suggests uncontrolled variation;
- figures required by the assignment are visually inspected for readable titles, axes, units, legends, clipping, overlap, and correct data;
- code and outputs agree with the planned interfaces and report claims.

If these checks fail, continue repairing within scope. If they cannot pass, stop with `代码或结果未通过，报告未修改` and explain the failed criterion. Do not weaken criteria or fill the report with provisional results.

### Evidence-enhancement gate

After the baseline run passes and before report authoring, compare the assignment requirements and the report's current evidence against the following optional improvements:

- **Controlled parameter comparison:** use the same data split, random seed, model, and evaluation procedure; vary one parameter at a time; record measured metrics and convergence behavior; never label a setting “best” without a measured comparison.
- **Dynamic-process evidence:** when optimization or training is described as dynamic, embed a readable static trajectory plot or representative frames in the report. A file path to an unsubmitted GIF or video is not sufficient evidence.
- **Metric summary:** add a compact metrics table, confusion matrix, or per-class metrics when these make the result easier to verify or interpret. Use only values from the final run.
- **Method auditability:** add concise formulas or a method explanation when the algorithm cannot be checked reliably from the code excerpt alone.

Enable only the improvements that are relevant to the assignment or materially improve the report. Run any new comparison or visualization before writing its values into the report. Keep a small evidence map of requirement, source artifact, measured value, and report location so every claim remains traceable.

### Submission boundary

Separate internal verification artifacts from the submission set:

- Internal artifacts may include JSON, CSV, plots, animations, extracted assets, renders, and QA files needed to validate the work.
- The submitted set is determined by the assignment or the user's explicit instruction. If it is limited to the report DOCX and source code, embed the necessary figures and values in the report and do not describe internal result paths as files being submitted.
- Mention an internal artifact path in the report only when the assignment explicitly requires the reader or grader to access that artifact.
- At handoff, list submitted paths first and exclude task-owned renders, caches, and QA files from deliverables.

## 4. Workspace-local report authoring

Once the code gate passes, create the dedicated `.lab-work/lab-experiment-report` directory in the current workspace and copy the confirmed report to `.lab-work/lab-experiment-report/report-working.docx`. Edit only that working DOCX until it passes the report checks. Keep temporary renders, extracted assets, and other QA files in the same task-owned directory; they are not deliverables. If that directory already contains files not clearly created by this skill, do not delete them automatically.

Before editing the working DOCX, inspect the target and decide how each content slot maps to assignment and experiment evidence. Preserve existing template features unless the assignment or user explicitly requires a change:

- page size, orientation, margins, columns, and section breaks;
- school marks, cover tables, score fields, headers, footers, page numbers, numbering, styles, and relationships;
- existing student identity that belongs to the current report;
- date or score values explicitly supplied by the user.

Do not invent dates, grades, teachers, hardware, locations other than the fixed location, or experimental facts. Leave unspecified date and score fields blank. Set the experiment location to `实训楼508` even if the untouched template contains an older location.

Author the body around the assignment's required structure. Use actual commands, final code, generated figures, measured values, and observed behavior. Explain method, results, limitations, parameter effects, and errors only to the extent supported by evidence. Do not force a universal page count or copy the organization of an unrelated experiment.

### Chinese spacing rule

For newly written Chinese prose, do not introduce a space at either of these boundaries:

- Han character followed by `[A-Za-z0-9]`;
- `[A-Za-z0-9]` followed by a Han character.

This rule does not permit changes inside verbatim assignment text, code, formulas, commands, identifiers, file paths, URLs, citations, or required official names. Verbatim fidelity has priority.

### Reference hygiene

When a reference report or previously completed student report was used, search the visible working report for the known reference-student names, IDs, dates, locations, code, metrics, captions, and conclusions. Inspect comments, tracked changes, metadata, or hidden package content only when the source report contains them or visible evidence suggests residue. Remove reference-specific residue without deleting required template structure. Every conclusion must be supported by the current run.

Do not modify the original report while authoring or correcting the working copy. A failure before the final replacement leaves the original unchanged. Keep only the working files needed to continue the current task. After a successful final replacement, remove `.lab-work/lab-experiment-report` and remove `.lab-work` only when it is empty.

## 5. Structural and visual QA

After the final edit batch, perform the focused structural checks:

- current student identity is correct and no reference identity remains;
- the experiment-purpose body is an exact character-for-character match with the captured assignment text;
- the location is exactly `实训楼508`;
- unspecified date and score fields are blank;
- required sections, code excerpts, tables, captions, figures, metrics, and result discussions exist;
- all report numbers agree with generated output files;
- newly written Chinese prose contains no inserted Han/Latin or Han/digit boundary spaces, subject to the listed exceptions;
- no ZIP archive was created unless the user explicitly overrode that rule.

Resolve LibreOffice once by using `soffice` from `PATH` or the standard Windows installation path `C:\Program Files\LibreOffice\program\soffice.exe`. Then use the installed DOCX rendering workflow to render every page of `.lab-work/lab-experiment-report/report-working.docx` once and inspect the cover, tables, equations, images, captions, page breaks, clipping, overlap, blank pages, and missing glyphs. Rerender only after fixing an observed problem.

Immediately before committing the report, confirm that the target path is unchanged, its initial fingerprint still matches, and no Word lock or sharing violation is present. If any check fails, stop without replacing the original. Otherwise use an atomic same-volume replacement appropriate to the operating system to replace the original with the validated working DOCX. Reopen the final path once to confirm that the DOCX parses correctly, then remove only the task-owned working directory and an empty `.lab-work` parent.

If rendering is unavailable, the working DOCX fails validation, or atomic replacement cannot be completed, leave the original unchanged and state the exact problem. Never delete pre-existing user files. Deliver paths for the submitted experiment source and original report DOCX, plus required result artifacts only when the assignment or user included them in the submission set. Do not create or deliver a ZIP archive by default.

## 6. Scenario decisions

| Situation | Required behavior |
| --- | --- |
| One clear assignment and one clear report | Continue without asking. |
| Multiple plausible assignments or reports | Ask the user to select the exact path. |
| Matching Word lock or sharing violation | Continue code work if useful, but do not replace the report until it is closed. |
| Existing compliant code | Reuse it and implement only missing requirements. |
| Code or result acceptance failure | Keep repairing; if unresolved, leave the report untouched. |
| A report would benefit from stronger evidence | Run only relevant controlled comparisons or add embedded evidence that can be traced to a verified run. |
| Working report fails validation or rendering | Leave the original unchanged and repair only the working copy. |
| Original changes after staging begins | Stop before replacement and ask the user how to proceed. |
| Atomic replacement fails | Leave the original unchanged when possible and report the exact failure. |
| Cleanup is blocked after successful replacement | Preserve the residual task-owned files, report their exact path, and do not list them as deliverables. |
| Assignment requests a ZIP | Do not create one unless the user explicitly overrides the fixed no-ZIP rule for that invocation. |
