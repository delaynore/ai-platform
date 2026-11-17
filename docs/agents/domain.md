# Domain docs

## Before exploring

Read root `GLOSSARY.md` and the ADRs in `docs/adr/` relevant to the task.

If these files do not exist, proceed silently. The `domain-modeling` skill
creates them as terms and decisions are resolved; do not create placeholders.

## Layout

Use a single-context layout:

- Root `GLOSSARY.md` defines the project's domain terms.
- `docs/adr/` holds architectural decision records.

## Vocabulary and decisions

Use the glossary's terms in documentation, issues, and code.
If a needed term is missing, reconsider whether it belongs to the domain
and note any real gap for `domain-modeling`.

When proposing a change that contradicts an existing ADR, explicitly identify
the conflict and explain why the decision should be revisited.
