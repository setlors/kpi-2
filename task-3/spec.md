## Intent

The diagram shows how UC-05 "Add exercise to workout" runs over time: the User submits an exercise with `sets`, `reps` and `weight_kg`, the system checks access, validates the values and creates a `WORKOUT_EXERCISE`, or rejects the request. It also shows that this step is optional, because a `WORKOUT` without exercises is valid.

## Chosen use case

**UC-05 Add exercise to workout** (from Task 2).

| Item         | Value                                                                                |
| ------------ | ------------------------------------------------------------------------------------ |
| Actor        | User                                                                                 |
| Relations    | `extend` UC-04 (Record workout); the access check UC-03 reaches UC-05 through UC-04  |
| Requirements | R-06, R-07, R-04, R-08                                                               |
| Scenarios    | SC-07, SC-08, SC-09, SC-06                                                           |
| User stories | US-xx: to be filled in (no user story file was available when this spec was written) |

## Participants

- User (actor)
- Workout tracker (the system inside the Task 2 boundary)
- `WORKOUT`, `EXERCISE`, `WORKOUT_EXERCISE` (entities from `task1/spec.md`)
- NotificationService does not take part: UC-05 is not linked to R-11.

## Steps of the scenario

1. The User submits an exercise for an existing workout with `sets`, `reps` and `weight_kg` (SC-07).
2. The system checks that the User is authenticated (UC-03 via UC-04, R-04). If not, access is denied and nothing is created.
3. The system validates the values (R-07): `sets` and `reps` are positive integers, `weight_kg` is not negative. If not, it returns a validation error and nothing is created (SC-08). `weight_kg` = 0 is valid (SC-09).
4. The system creates a `WORKOUT_EXERCISE` with `workout_id`, `exercise_id`, `sets`, `reps`, `weight_kg` (R-06, SC-07).
5. The system confirms the result to the User.

Optional step: if the User adds no exercise, the `WORKOUT` stays saved without an error (R-08, SC-06).

## Acceptance criteria

1. The diagram is written in Mermaid `sequenceDiagram` and its rendered image is committed.
2. The participants are exactly those listed above; NotificationService is absent.
3. Entity and field names match `task1/spec.md` exactly (`WORKOUT_EXERCISE`, `workout_id`, `exercise_id`, `sets`, `reps`, `weight_kg`).
4. Every call has a direction arrow and a return message; call names are action names (for example `addExercise()`, `validateEntry()`).
5. The order of messages is: access check, then validation, then creation of `WORKOUT_EXERCISE`.
6. An `alt` branch shows an unauthenticated User: access is denied and no `WORKOUT_EXERCISE` is created (R-04).
7. An `alt` branch shows invalid values (`sets` ≤ 0, `reps` ≤ 0 or `weight_kg` < 0): a validation error is returned and no `WORKOUT_EXERCISE` is created (R-07, SC-08).
8. The success branch accepts `weight_kg` = 0 (SC-09).
9. The success branch creates one `WORKOUT_EXERCISE` with both `workout_id` and `exercise_id` set (R-06, SC-07).
10. An `opt` fragment shows that skipping the step leaves a saved `WORKOUT` without an error (R-08, SC-06).
11. If a `loop` is used, it repeats the whole add cycle once per exercise (`WORKOUT` — `WORKOUT_EXERCISE` is 1:N in `task1/spec.md`).
12. Each of R-04, R-06, R-07, R-08 and SC-06 to SC-09 appears in at least one message or fragment.
13. No id or file from Tasks 1 and 2 is renamed or changed.
14. Labels use only R-xx, SC-xx and UC-xx ids that exist in tasks 1 and 2.
