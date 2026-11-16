# Documentation instructions

This page applies when you write or change documents, ADRs, research notes, glossaries, or changelogs.

## Mandatory: one home per fact

Each fact has one home.
Every other document links to that home instead of repeating the fact.
When you change behavior, update the document that owns it in the same change.

| Kind of fact | Home |
| ------------ | ---- |
| Universal agent constraint | `.rules` |
| Task-specific agent instruction | `docs/agents/<task>.md` |
| How to call a module now | That module's `HOW-TO.md` |
| A decision and its reasons | An ADR in `docs/adr/` |
| The reason behind code | Never a code comment; see [engineering.md](./engineering.md#mandatory-no-code-comments) |
| A domain term | The `CONTEXT.md` that owns it; see [domain.md](./domain.md) |
| A check's invariant | `docs/checks/<name>.md` |
| A user-visible feature's verification | Its Feature map; see [verification.md](./verification.md) |
| A finding from outside research | `docs/research/` |
| Release notes of a published package | That package's changelog |

## Mandatory: HOW-TO pages

A `HOW-TO.md` shows the current way to call a module.

- Start with a literal sample that works, then state the rules.
- Keep only current contracts.
  Move history to an ADR.
- A HOW-TO beats an older ADR on call shape.

## Mandatory: ADRs

Write an ADR when a decision is hard to reverse, would surprise a later reader, and came from a real trade-off.
Skip it when any of the three is missing.

Use [templates/adr.md](../../templates/adr.md).
An ADR holds the decision, the reasons, the options you rejected and why, and the consequences.
Number ADRs in sequence; the check spec [record-numbers](../../checks/record-numbers.md) keeps numbers unique.

To change an accepted decision, write a new ADR that names the one it supersedes.
Edit an accepted ADR in place only to fix a fact or add a small amendment, and mark the amendment with its date.

## Default: research notes

Put the result of outside research in `docs/research/<topic>.md`.
Name each source and the date you checked it.
Research notes are reference, never instructions.

## Mandatory: changelogs

Only published packages have changelogs.
Do not edit a generated changelog by hand.

## Mandatory: size budget

Each page that agents always read stays under 32,000 bytes.
The check spec [doc-size](../../checks/doc-size.md) enforces it.
Never raise the budget.
Split the page, or move detail into a reference page that agents read on a trigger.

Read a large reference page one section at a time: list its headings, then read only the section you need.

## Mandatory: archive

Move a document that only records history to an `archive/` folder.
Agents skip `archive/` unless the task is about history.

## Mandatory: writing style

- Write in Simplified Technical English: short sentences, active voice, present tense where true.
- One idea per sentence, and about 20 words at most.
- Use one word for one meaning; do not rotate synonyms.
- Write instructions as commands: "Run X", not "X should be run".
- Put each sentence on its own line in Markdown.
- Do not use the em dash.
- Use glossary terms as written.
- Show a literal sample (a command, an output, an error line) instead of describing one.

## Mandatory: guidance maintenance

When you change agent guidance:

- Put each rule in the one page that owns its trigger.
- Delete a rule that a check now enforces, except for one line that links the check spec.
- Do not add a rule for a mistake that happened once.
  A repeated mistake becomes a Correction; see [corrections.md](./corrections.md).
- Keep `.rules` for universal constraints only.
