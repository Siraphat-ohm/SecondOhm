---
name: explain-feature
description: Explain implemented feature behavior with a code trace and self-contained HTML. Use for feature walkthroughs, request/data flow, or architecture overviews; include an editorial diagram when a relationship or sequence is visual.
---

# Explain Feature

Produce one accurate, reader-facing HTML explanation of a feature. The explainer answers how the feature works in this codebase, not how a similar feature would usually work.

## Outcome

Save `<feature-slug>-explanation.html` at the user-requested path. Without a requested path, save it in the current directory. The file is self-contained: embedded CSS, inline SVG, no runtime dependencies, and no network fetches.

Use **trace** as the organizing principle: a reader can follow the feature from its entry point through decisions, state or data changes, side effects, and visible result.

Before drafting, load [`references/explanation-language.md`](references/explanation-language.md) and apply its **re-pitch** rules to the title, summary, trace, captions, and transitions.

## Traceability

Every explainer carries a visible **Traceability** section. It is the source of truth for how a reader returns from the explanation to the code:

- **Snapshot:** repository-relative source root, full `git rev-parse HEAD` commit, current branch, and whether relevant working-tree edits were present. Outside Git, state `Git snapshot unavailable` and name the source root.
- **Claim map:** every numbered trace step and code excerpt names a repository-relative `path#Lx-Ly` and enclosing symbol. One claim may cite several locations; one location may support several claims.
- **Scope:** list the entry point, state/data owner, external dependency, and output artifact examined. Mark any unavailable source or inferred link.

The snapshot is captured before investigation and rendered with the explanation. Never replace an unavailable commit or path with a guess.

## Workflow

1. **Frame the reader.** Read the relevant `CONTEXT.md` and any routing `CONTEXT-MAP.md`, then state the feature's purpose in one sentence and name the question the explanation answers. Identify the intended audience from the request; default to an engineer new to this repository.
   - Done when the feature boundary, reader, and vocabulary are explicit.

2. **Trace the implementation.** Capture the Traceability snapshot, then read the relevant entry point, every directly involved branch, the state/data owner, and each external or persistent effect. Follow actual symbols and call sites; distinguish observed behavior from an inference.
   - Done when every step from trigger to observable result has a claim-map location or is marked as an inference.

3. **Choose the explanation shape.** Use the trace as the primary structure. Include only information that changes the reader's mental model:
   - purpose and prerequisites;
   - numbered execution trace;
   - inputs, outputs, state transitions, and side effects;
   - decisions, errors, permissions, retries, or async boundaries when they alter behavior;
   - one compact code excerpt per non-obvious mechanism, with file path and line range.
   - Done when the explanation accounts for the normal path and every material alternate path without mirroring source files.

4. **Draw when the relationship is visual.** For a flow, boundary, state, dependency, or data transformation that is harder to retain as prose, load `skill://diagram-design`, select its type, and use its design rules. Put the resulting accessible inline SVG directly in the explainer; do not iframe or link another generated HTML file. Omit the diagram when the numbered trace communicates the same information more clearly.
   - Done when the figure contributes distinct understanding and its `<title>` and `<desc>` explain the feature rather than its geometry.

5. **Build the HTML.** Make the page quiet and scannable: one title, a one-sentence summary, a trace, optional figure, code excerpts, a Traceability section, and a short "What changes if…" section for material branches. Apply `explanation-language.md`: context precedes mechanism; prose uses the repository's terms and ASD-STE100 Simplified Technical English. Use semantic HTML, readable system typography, sufficient contrast, and responsive single-column layout. Escape code and untrusted strings.

6. **Check the reader path.** Open the saved HTML in a browser. Confirm the page is legible at desktop and narrow widths, each section supplies context before its mechanism, every claim-map location resolves in the captured source snapshot, the trace order matches execution, and any diagram is visible and accessible.

## Learning boundary

This skill explains one feature in its local code context. If the reader lacks the programming concept needed to understand the trace—such as promises, dependency injection, database transactions, ownership, or an algorithm—say so plainly and recommend `skill://teach` for a stateful lesson. Keep the feature explainer focused; do not turn it into a course.

## Delivery

Report the saved HTML path, captured commit (or unavailable Git snapshot), and code locations traced. State any unresolved behavior or inference explicitly.