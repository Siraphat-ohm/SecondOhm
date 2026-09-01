---
name: arai-na
description: Re-explain your previous answer in plain Thai when you've lost the thread, restoring skipped reasoning and context instead of just rewording it.
disable-model-invocation: true
license: MIT
---

# Arai Na (อะไรนะ)

Recover comprehension when the user falls off the previous explanation. Treat "อะไรนะ", "งง", or "ตามไม่ทัน" — even alone, without naming which part — as: "ฉันหลุดจากคำอธิบายตรงนี้ และไม่เข้าใจว่าเรามาถึงข้อสรุปนี้ได้อย่างไร" Infer the gap from where the trigger fired; never ask the user to restate what they didn't understand when it can be recovered from context.

The target is not a shorter or easier-sounding answer. It is the missing path from what the user already understands to what the explanation was trying to say.

## Workflow

1. **Find the gap.** Reread the explanation immediately before the trigger. Identify the first point where a reasonable reader could no longer answer "why does this follow from what came before" — a skipped reasoning step, an assumed concept, an unexplained term, a jump straight from problem to solution, a conclusion without its premise, an unnamed reference to earlier context, an abstraction introduced before the concrete picture, a pronoun covering more than one actor, a term that changed name mid-explanation, or an implicit condition that was never stated.
   - Done when you can name the specific sentence, term, or transition that most likely broke comprehension — not "the whole answer."

2. **Choose the recovery point.** Start from the nearest point the user still understands; add only what the gap needs. Move further back only when the missing piece genuinely depends on an earlier foundation.
   - Done when the explanation opens before the gap, not unnecessarily before it.

3. **Rebuild the causal chain.** Walk only the links needed to make the original conclusion follow again: เราอยู่ตรงไหน → มีอะไรเกิดขึ้น → ทำไมมันถึงเป็นปัญหา → อะไรเป็นสาเหตุ → เพราะอย่างนั้นเราจึงทำอะไร → ผลที่ได้คืออะไร. Use `เพราะ`/`ทำให้`/`จึง` only where the relation is actually causal, not merely sequential (`จากนั้น`, `หลังจากนั้น`).
   - Done when every step follows from the one before it, with no unstated logical jump.

4. **Write in Plain Thai**, applying the rules below, and repair the specific failure found in step 1: missing reasoning gets the reasoning; missing context gets the context; an unexplained term gets explained where it's used; an early abstraction gets a concrete example first; an ambiguous reference gets a named actor.
   - Done when the new answer contains information the old one didn't say — not the same content in different words.

## Plain Thai

Plain Thai is Thai optimized for comprehension in the current working context. Prefer natural, direct wording over unnecessary formality or literal translation, while preserving the level of formality the task actually requires. Apply on every response:

- **Concrete before abstract.** Show the situation before naming its category: "component สร้าง function ตัวใหม่ทุกครั้งที่ render — นี่คือปัญหา referential equality," not the label first.
- **Name the actor** when `มัน`/`ตัวนี้`/`ตรงนี้` could mean more than one thing. A pronoun chain covering two components (`มันรับเข้ามา แล้วมันเช็ก แล้วมันส่งต่อ`) is a comprehension failure, not economy.
- **One primary term, one concept.** Establish a primary term for each concept and keep using it. A brief synonym or Thai explanation is fine when introducing the term, but don't rotate `request`/`คำขอ`/`ข้อมูลที่ส่งมา` afterward for stylistic variety. Prefer the project's vocabulary (`CONTEXT.md`, `CONTEXT-MAP.md`, code identifiers, prior conversation) when it exists; work without it when it doesn't.
- **Direct verbs over nominalized wording**, unless the longer form carries a distinction the short one loses: `ตรวจ config`, not `ทำการตรวจสอบการกำหนดค่า`.
- **Technical identifiers stay verbatim** — API names, functions, variables, commands, flags, paths, package/protocol names, config keys, error messages, code literals. Explain them in Thai around the untouched term (`useEffect` คือ hook ที่ React เรียกหลัง render), never by translating the identifier itself.
- **Explain a term for the problem at hand**, not as a glossary entry: `invariant ในที่นี้คือเงื่อนไขที่ code ด้านในถือว่าเป็นจริงเสมอ` — scoped to this explanation.
- **Keep conditions visible.** A conclusion that only holds when something else holds must say so — `ถ้า validation ผ่าน...`, not a bare `หลัง validation แล้ว...` when validation can fail.
- **Split concepts that do different jobs** (validation vs. normalization) when the reader must track them separately — not by a fixed one-idea-per-sentence rule.
- **Ground abstraction with the smallest example that exposes the relationship**, not a decorative one that introduces more concepts than it explains.

## Ordering, formatting, tone

Default order: orientation (where we are) → mechanism (what happens) → reason (why that creates the problem or supports the conclusion) → consequence (back to the original claim). Skip a stage the reader already has.

Match structure to the reasoning, not decoration: short paragraphs per step, numbered steps when order matters, arrows for short pipelines (`input → validation → normalization`), inline code for identifiers. A paraphrase-length gap doesn't need headings or bold.

Open with the gap itself — `จุดที่ข้ามไปคือ...` — not a frame implying the user should already have followed. Drop hedges that add pressure without information: `อย่างที่อธิบายไปแล้ว`, `เห็นได้ชัดว่า`, `แน่นอนว่า`, `ง่ายๆ คือ`.

## When the previous answer was wrong

Fix it instead of rationalizing it. A factual error, contradiction, invalid inference, or missing condition in the earlier answer gets named — `ตรงนี้คำตอบก่อนหน้าของผมข้ามเงื่อนไขสำคัญไป...` — then the explanation rebuilds from the corrected premise. Never construct a new justification for a conclusion that re-examination shows is wrong.

## Uncertainty and scope

When the previous explanation supports more than one reading, use the one the conversation best supports rather than asking the user to disambiguate — unless the ambiguity changes the explanation and context can't resolve it, in which case name it and cover the smallest useful set of readings.

Stop once the user can resume the original work. Test each paragraph: if it disappeared, would the original conclusion still follow? If yes, cut it.

## Non-goals

- **Summarize** — the fix may need more words, not fewer.
- **Simplify at the cost of meaning** — don't drop distinctions, conditions, or precision to sound easier.
- **Translate everything** — English terms stay English when they're the project's established or more precise term.
- **Restart from zero** — the user lost a thread, not necessarily the whole topic.
- **Swap in a different solution** — re-explain the existing answer; correct it explicitly if it was wrong, don't silently replace it.
- **Add unrelated background** — only what closes the identified gap.

## Success check

The user should now be able to answer: เรากำลังพูดถึงอะไร, ก่อนหน้านี้เกิดอะไรขึ้น, ทำไมขั้นก่อนหน้าจึงนำมาถึงขั้นนี้, คำ technical ที่จำเป็นหมายถึงอะไรในบริบทนี้, และข้อสรุปหรือสิ่งที่จะทำต่อคืออะไร. Any one still unanswerable means the gap remains, not the wording.

## Example

Before:
> เราควรย้าย validation ไป boundary เพราะตอนนี้ invariant ถูกตรวจหลัง normalization ทำให้ downstream code ต้องรองรับ invalid state ที่จริงไม่ควรเกิดขึ้น

User: อะไรนะ

Fails (same gap, different words):
> เราต้องย้ายการตรวจสอบไปที่ทางเข้า เพราะข้อมูลที่ผิดไม่ควรเข้ามา

Closes the gap:
> ตอนนี้ flow เป็นแบบนี้: `input → normalization → validation`
>
> จุดที่ข้ามไปคือ ก่อนถึง `validation` เราปล่อยข้อมูลที่อาจยัง invalid เข้า `normalization` แล้ว ทำให้ code ช่วง normalization ต้องคิดเผื่อสองกรณีตลอด: ข้อมูลที่ใช้ได้ และข้อมูลที่จริงควรถูก reject
>
> ถ้าย้าย `validation` ไปที่ boundary — จุดที่ input จากภายนอกเข้าระบบ — flow จะกลายเป็น: `input → validation → normalization → internal code`
>
> ถ้า validation ผ่าน code หลังจากนั้นจึงถือได้ว่าข้อมูลตรงตามเงื่อนไขพื้นฐานแล้ว นี่คือ `invariant` ที่ผมพูดถึง — เงื่อนไขที่ code ด้านในถือว่าเป็นจริงได้เสมอ

For more failure shapes to check a rewrite against (over-translating, over-restarting, false simplicity), see [`references/failure-patterns.md`](references/failure-patterns.md).
