## Intent

The diagram shows the business process of UC-05 "Add exercise to workout": the User submits an exercise with `sets`, `reps` and `weight_kg`, the Workout tracker checks access, validates the three values in parallel and creates a `WORKOUT_EXERCISE`, or rejects the request. It also shows that adding exercises is optional and can be repeated, because a `WORKOUT` without exercises is valid.

## Chosen use case and notation

**UC-05 Add exercise to workout** (Task 2). Why: it is the use case that the Task 3 sequence diagram already details, so the process stays consistent with it.

| Item      | Value                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------ |
| R-06, R-08 | Linked to UC-05 in the Task 2 use case table                                                          |
| R-07      | Linked to UC-05 in the Task 2 matrix note: "validation is a rule inside Add exercise to workout (UC-05)" |
| R-04      | Reached through `extend` UC-04 and `include` UC-03 (Task 2 rule: a use case covers a requirement it reaches through `include` or `extend`) |
| Scenarios | SC-06, SC-07, SC-08, SC-09                                                                             |

The project has scenarios (SC-xx) only; there are no user stories.

**Notation: activity diagram (Mermaid `flowchart`) with two swimlanes.** Why: the process is a flow of actions, decisions and a parallel branch between two roles, and swimlanes show who does each step.

Diagram source: `activity-diagram.mmd`. Rendered image: `activity-diagram.png` (also `activity-diagram.svg`).

## Roles (swimlanes)

- **User** (human actor). In Task 2, User is a specialization of Guest, so a User who is not authenticated is a guest in the sense of R-04.
- **Workout tracker** (the system, same name as in Task 3).
- `WORKOUT`, `EXERCISE`, `WORKOUT_EXERCISE` are data, not roles. NotificationService does not take part (UC-05 is not linked to R-11).

## Flow

| #   | Lane            | Type     | Step                                                                                   | Next                                                          |
| --- | --------------- | -------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| 0   | User            | Start    | A `WORKOUT` exists                                                                     | 1                                                             |
| 1   | User            | Decision | Does the User add an exercise?                                                         | **If** yes → 2. **Else** → End "workout saved" (R-08, SC-06) |
| 2   | User            | Action   | `addExercise(workout_id, exercise_id, sets, reps, weight_kg)`                          | 3                                                             |
| 3   | Workout tracker | Action   | `checkAccess()` (UC-03, R-04)                                                          | 4                                                             |
| 4   | Workout tracker | Decision | Is the User authenticated?                                                             | **If** yes → 6. **Else** → 5                                  |
| 5   | Workout tracker | Action   | `accessDenied`; nothing is created (R-04)                                              | End "access denied"                                           |
| 6   | Workout tracker | Fork     | `validateExerciseValues(sets, reps, weight_kg)` starts three parallel checks (R-07)    | 7a, 7b, 7c at the same time                                   |
| 7a  | Workout tracker | Action   | Check that `sets` is a positive integer                                                | 8                                                             |
| 7b  | Workout tracker | Action   | Check that `reps` is a positive integer                                                | 8                                                             |
| 7c  | Workout tracker | Action   | Check that `weight_kg` is not negative (0 is valid, SC-09)                             | 8                                                             |
| 8   | Workout tracker | Join     | Wait until 7a, 7b and 7c are all done                                                  | 9                                                             |
| 9   | Workout tracker | Decision | Did all three checks pass?                                                             | **If** yes → 11. **Else** → 10                                |
| 10  | Workout tracker | Action   | `validationError`; nothing is created (R-07, SC-08)                                    | 1                                                             |
| 11  | Workout tracker | Action   | `createWorkoutExercise(workout_id, exercise_id, sets, reps, weight_kg)`, then `exerciseAdded` (R-06, SC-07) | 1                                       |

**End "workout saved":** the User stops adding exercises; the `WORKOUT` stays saved without an error (R-08, SC-06).
**End "access denied":** no `WORKOUT_EXERCISE` is created (R-04). No scenario exists for this path in Task 2 (the matrix marks R-04 as partial), so it is labelled with R-04 only.

Going back to step 1 is the `loop` of the Task 3 sequence diagram; step 5 is its `break`. The access check is the UC-03 reached through UC-04 and, as in Task 3, it runs for every `addExercise`.

**Note on the fork:** the files do not say that the checks run in parallel. The fork is a modelling choice based on the three independent conditions of R-07. In Task 3 they are one message, `validateExerciseValues`, so the fork is a detail inside that message and the order (access check, validation, creation) does not change.

## Acceptance criteria

1. The diagram is a Mermaid `flowchart` and its rendered image is committed.
2. There is exactly one start node and two end nodes ("workout saved", "access denied").
3. Every node except the start is reachable from the start.
4. Every node except the end nodes has at least one outgoing flow (no dead ends).
5. Every decision (add an exercise, authenticated, all checks passed) has both an "if yes" and an "else" outcome, drawn and labelled.
6. The fork has exactly three branches (`sets`, `reps`, `weight_kg`), and all three end in one join.
7. No branch of the fork leaves the process before the join, and the "all checks passed" decision comes only after the join.
8. The order is: access check, then the fork/join validation, then creation, as in the Task 3 sequence diagram.
9. The access check runs for every `addExercise`, as in the Task 3 `loop`, and the start node does not claim that access was already checked.
10. The unauthenticated path ends in "access denied", creates no `WORKOUT_EXERCISE` and is labelled with R-04 only (no scenario exists for it).
11. The failed-validation path creates no `WORKOUT_EXERCISE` and returns to the first decision (R-07, SC-08).
12. The success path accepts `weight_kg` = 0 and creates one `WORKOUT_EXERCISE` with `workout_id` and `exercise_id` (R-06, SC-07, SC-09).
13. Choosing "else" at the first decision ends the process with the `WORKOUT` saved (R-08, SC-06).
14. Action names match the Task 3 messages; entity and field names match `task1/spec.md` exactly.
15. NotificationService does not appear in the diagram.
16. Labels use only R-xx, SC-xx and UC-xx ids that exist in Tasks 1-3, and no id or file from Tasks 1-3 is renamed or changed.
17. Each of R-04, R-06, R-07, R-08 and SC-06 to SC-09 appears in at least one node or label.
18. The lane names are the Task 3 participants "User" and "Workout tracker", and the User is treated as a Guest specialization (Task 2).
19. Every requirement linked to UC-05 names its source in Task 2 (use case table, matrix note, or `include`/`extend` rule).
