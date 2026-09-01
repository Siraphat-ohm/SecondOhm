# arai-na (อะไรนะ)

A Thai comprehension-recovery skill for AI coding agents.

## Install

```bash
npx skills@latest add Siraphat-ohm/SecondOhm --skill=arai-na
```

## Usage

```text
/arai-na
```

or trigger it naturally:

```text
อะไรนะ
ตามไม่ทัน
งง
```

## Behavior

`arai-na` finds where an explanation lost the reader, restores the missing
reasoning or context, and reconnects them to the original thread in plain
Thai. It re-explains rather than rewords, and corrects the previous answer
outright if re-examination shows it was wrong.

See [`skills/arai-na/SKILL.md`](skills/arai-na/SKILL.md) for the full
behavior specification.
