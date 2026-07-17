# Module: {Name}

> Common specification template — every module is documented with exactly these fields
> (per the instruction file's "common specification template" requirement).
> One source of truth: object names, state names, and terminology must match
> `product/object-model.md` and `product/product-brief.md`. Do not reinvent.

## 1. Purpose
One paragraph: why this module exists, which verified pain point(s) it serves (link to evidence ledger IDs).

## 2. User goals
Bulleted, in the user's voice ("I want to …, so that …").

## 3. Objects
The domain objects this module owns or touches (from the shared object model), with fields owned here.

## 4. Lifecycle
For each owned object: states, allowed transitions, what creates/archives/deletes it, retention.

## 5. Actions
Every user-triggerable action: name, trigger surface (button/gesture/keyboard), input, effect, undoability.

## 6. States
Screen/UI states including: empty, loading, ideal, partial, error, offline, and "returning after N weeks away".

## 7. Workflows
End-to-end flows step by step (happy path + top 2 failure paths), referencing screens by wireframe ID.

## 8. AI behavior
What the AI does here: triggers, inputs, model tier, prompt contract, output contract, guardrails,
fallback when AI unavailable/low-confidence, what is deliberately NOT AI.

## 9. Scale
Behavior at 10× and 100× data (items, projects, months of history): pagination, search, archival, performance budgets.

## 10. Errors
Failure modes and recovery: sync conflicts, AI errors, partial writes, user mistakes (undo/restore).

## 11. Permissions
Data visibility, sharing (if any), privacy notes (ADHD is health-adjacent data).

## 12. Dependencies
Other modules/objects/services this module requires; events it emits/consumes.
