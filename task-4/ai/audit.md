# Audit of AI output

## Mistake 1: wrong actor name for the unauthenticated case

The AI put the unauthenticated case under the actor "User". In Task 2, R-04 says "WHILE a **guest** is not authenticated" and R-06 speaks about an "authenticated user", so the same person had two names (Guest in Task 2, User in the diagram).
Fix: spec.md now states that User is a specialization of Guest in Task 2, so a User who is not authenticated is a guest in the sense of R-04. The lane stays "User" to match Task 3. Added criterion 18: lanes are the Task 3 participants and User is treated as a Guest specialization.

## Mistake 2: start node contradicts the access check

The start node said "A `WORKOUT` is saved", which implies that access was already checked in UC-04. Then the "access denied" branch looks unreachable. Task 2 has no direct link from UC-05 to UC-03 (UC-04 and UC-06 `include` UC-03, UC-05 only `extend`s UC-04), and the AI did not explain why `checkAccess` is inside UC-05.
Fix: the start node is now "A `WORKOUT` exists" (the same as step 1 of Task 3: "an existing workout"), and the diagram is re-rendered. spec.md says the check is the UC-03 reached through UC-04 and runs for every `addExercise`, as in the Task 3 `loop`. Criterion 9 now also forbids a start node that claims access was already checked.

## Mistake 3: unsupported requirement links and a non-existent artifact

The AI linked UC-05 to R-04, R-06, R-07 and R-08 without saying where each link comes from. The Task 2 use case table gives UC-05 only R-06 and R-08, and the matrix marks R-07 as having no use case.
Fix: every link now names its source. R-06 and R-08 come from the Task 2 table, R-07 from the matrix note ("validation is a rule inside UC-05"), and R-04 from the Task 2 `include`/`extend` rule. The "access denied" path is labelled with R-04 only, because no scenario exists for it (the matrix marks R-04 partial). Added criteria 10 and 19.
