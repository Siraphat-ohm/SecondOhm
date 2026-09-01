---
name: explain-bug
description: Explain a reported bug with an evidence-backed reproduction, causal chain, and self-contained HTML. Use for failure walkthroughs, incident explanations, unexpected behavior, or regressions; include an editorial diagram when the causal path is visual.
---

# Explain Bug

Produce one accurate, reader-facing HTML explanation of a bug. The explainer separates what was observed, what was reproduced, what the code proves, and what remains unknown. Its deliverable is the evidence-based explanation; implementation changes require a separate request.

## Outcome

Save `<bug-slug>-explanation.html` at the user-requested path. Without a requested path, save it in the current directory. The file is self-contained: embedded CSS, inline SVG, no runtime dependencies, and no network fetches.

Use **causal chain** as the organizing principle: a reader can follow the trigger, divergence from intended behavior, corrupted or missing state, visible symptom, and scope.

Before drafting, load [`references/explanation-language.md`](references/explanation-language.md) and apply its **re-pitch** rules to the title, summary, causal chain, captions, and transitions.

## Traceability

Every explainer carries a visible **Traceability** section. It is the source of truth for how a reader returns from the diagnosis to the evidence:

- **Snapshot:** repository-relative source root, full `git rev-parse HEAD` commit, current branch, and whether relevant working-tree edits were present. Outside Git, state `Git snapshot unavailable` and name the source root.
- **Evidence map:** every causal-chain link names a repository-relative `path#Lx-Ly` and enclosing symbol, or the reproduction command, input, log, or artifact that supports it. Label each item **Observed**, **Reproduced**, **Code**, **Inference**, or **Unknown**.
- **Scope:** list the trigger, entry point, state/data owner, external dependency, and output artifact examined. Mark every unavailable source or missing artifact.

The snapshot is captured before investigation and rendered with the explanation. Never replace an unavailable commit, source path, or evidence artifact with a guess.

## Workflow

1. **Frame the report.** Read the relevant `CONTEXT.md` and any routing `CONTEXT-MAP.md`, then state the expected behavior, observed behavior, affected user or system, and known trigger. Preserve the reporter's exact observation as evidence rather than translating it into a diagnosis.
   - Done when the bug boundary, reader, and vocabulary are explicit and observed facts are distinguished from assumptions.

2. **Make the failure concrete.** Treat the report's observed behavior as evidence. When a reachable environment permits, reproduce the reported path to capture the smallest trigger and trace causality—not to validate the report. Record inputs or preconditions, observed output, error, log, or state change, and the expected result. If that path is unavailable, identify the missing prerequisite and retain the report as unconfirmed only beyond its stated observation.
   - Done when the explainer has a minimal reproduction or states precisely why one is unavailable.

3. **Trace the causal chain.** Capture the Traceability snapshot, then read the entry point, every branch that can produce the divergence, the state/data owner, and each effect between trigger and symptom. Follow actual symbols and call sites. Compare the failing path with the expected path or an adjacent control case.
   - Done when every claimed link in the causal chain has an evidence-map item or is labeled as an inference.

4. **Choose the explanation shape.** Include only information that changes the reader's diagnosis:
   - expected versus observed behavior;
   - minimal reproduction and environment/preconditions;
   - numbered causal chain with source locations;
   - faulting condition and propagation path;
   - scope: affected inputs, users, versions, and unaffected control case when known;
   - short code excerpts for non-obvious mechanisms;
   - confidence and open questions.
   - Done when the normal/control path and failing path can be compared without reproducing the entire codebase.

5. **Draw when causality is visual.** For a request path, state transition, race, boundary crossing, dependency chain, or data corruption path that is harder to retain as prose, load `skill://diagram-design`, select its type, and use its design rules. Put the resulting accessible inline SVG directly in the explainer; do not iframe or link another generated HTML file. Omit the diagram when the numbered causal chain communicates the same information more clearly.
   - Done when the figure makes the divergence and its consequence clearer, with `<title>` and `<desc>` describing the bug rather than the geometry.

6. **Build the HTML.** Make the page quiet and scannable: title, severity/scope only when evidenced, expected versus observed comparison, reproduction, causal chain, optional figure, Traceability section, evidence-backed excerpts, and open questions. Apply `explanation-language.md`: context precedes mechanism; prose uses the repository's terms and ASD-STE100 Simplified Technical English. Use semantic HTML, readable system typography, sufficient contrast, and responsive single-column layout. Escape code, logs, and untrusted strings.

7. **Check the reader path.** Open the saved HTML in a browser. Confirm the page is legible at desktop and narrow widths, each section supplies context before its mechanism, evidence labels are visible, every evidence-map location or artifact resolves against the captured source snapshot, the causal chain matches the observed failure, and any diagram is visible and accessible.

## Learning boundary

This skill diagnoses one bug in its local code context. If the reader lacks the programming concept needed to understand the causal chain—such as concurrency, promise scheduling, transaction isolation, memory ownership, or an algorithm—say so plainly and recommend `skill://teach` for a stateful lesson. Keep the bug explainer focused; do not turn it into a course.

## Delivery

Report the saved HTML path, captured commit (or unavailable Git snapshot), reproduction status, evidence locations, causal-chain confidence, and every unresolved question. State separately if the work established a symptom but not a root cause.