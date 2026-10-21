# Audit of AI output

## Mistake 1: wrong use case link

The AI labelled the validation step `[UC-06, R-07]`, but UC-06 in Task 2 is "View workout history". Task 2 has no use case for validation (the traceability matrix marks R-07 as having no use case).
Fix: removed the UC link and labelled the step with Task 2 ids only (R-07, SC-08, SC-09). Added a criterion to spec.md: labels use only ids that exist in Tasks 1 and 2.

## Mistake 2: invented steps and wrong order

The diagram looked up the `WORKOUT` and the `EXERCISE` and checked the owner before validation, with early exits labelled `A-1` and `A-2`. Task 2 has no such steps, and `A-1` and `A-2` are not defined anywhere. The order also broke criterion 5 of spec.md (access check, then validation, then creation).
Fix: deleted the lookup messages and the `A-1`/`A-2` exit.

## Mistake 3: missing return messages

The self-messages `checkAccess()` and `validateExerciseValues()` had no return message, which breaks criterion 4 of spec.md (every call has a return).
Fix: added a dashed return after each self-call (`authenticated or not` and `valid or invalid`).
