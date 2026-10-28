## Intent

Three diagrams show how UC-05 "Add exercise to workout" runs over time, one diagram per scenario: SC-07 (an exercise is added), SC-08 (invalid values are rejected) and SC-09 (a zero weight is accepted). In each one the User submits an exercise with sets, reps and weight_kg, the system validates the values, and a WORKOUT_EXERCISE is created or the entry is rejected.

## Chosen use case

**UC-05 Add exercise to workout** (from Task 2).

| Item         | Value                           |
| ------------ | ------------------------------- |
| Actor        | User                            |
| Relations    | `extend` UC-04 (Record workout) |
| Requirements | R-06, R-07                      |
| Scenarios    | SC-07, SC-08, SC-09             |

## Participants

- User (actor)
- Workout tracker (the system inside the Task 2 boundary)
- `WORKOUT`, `EXERCISE`, `WORKOUT_EXERCISE` (entities from `task1/spec.md`)
- WORKOUT_EXERCISE (entity from task1/spec.md), only in the diagrams where a record is created (SC-07, SC-09)

## Steps of the scenarios

### SC-07 Add an exercise (R-06)

1. The User calls `addExercise(workout_id, exercise_id, sets=3, reps=10, weight_kg=60)`. A note says that "Bench press" is picked by name, but the call takes `exercise_id`. No lookup step is shown.
2. The Workout tracker calls `validateEntry(sets, reps, weight_kg)` and gets `valid`.
3. In an `opt` fragment (guard: `sets > 0`, `reps > 0`, `weight_kg >= 0`) the Workout tracker calls `create(...)` and `WORKOUT_EXERCISE` returns `WORKOUT_EXERCISE created`. The record has both `workout_id` and `exercise_id` set.
4. The Workout tracker returns `exercise added` to the User.

### SC-08 Invalid values (R-07)

1. The User calls `addExercise(workout_id, exercise_id, sets, reps, weight_kg)`. A note lists the three rows of the outline: (`sets` 3, `reps` 10, `weight_kg` -5), (`sets` 0, `reps` 10, `weight_kg` 40), (`sets` 3, `reps` 0, `weight_kg` 40).
2. The Workout tracker calls `validateEntry(sets, reps, weight_kg)` and gets `invalid`.
3. A `break` fragment (guard: `sets <= 0`, `reps <= 0` or `weight_kg < 0`) ends the flow: the Workout tracker returns `validation error` to the User.
4. A note states that no `WORKOUT_EXERCISE` is created. The `WORKOUT_EXERCISE` participant is not on the diagram.

### SC-09 Zero weight (R-07)

1. The User calls `addExercise(workout_id, exercise_id, sets=3, reps=15, weight_kg=0)`.
2. The Workout tracker calls `validateEntry(sets, reps, weight_kg)` and gets `valid`, because `weight_kg` = 0 is not negative.
3. In an `opt` fragment (guard: `weight_kg = 0` is not negative, so it is accepted) the Workout tracker calls `create(...)` and `WORKOUT_EXERCISE` returns `WORKOUT_EXERCISE created`.
4. The Workout tracker returns `exercise added` to the User.

## Acceptance criteria

1. There are three diagrams written in Mermaid `sequenceDiagram`, one per scenario (SC-07, SC-08, SC-09), and their rendered images are committed.
2. The participants are exactly those listed above: User and Workout tracker in every diagram, and `WORKOUT_EXERCISE` only in SC-07 and SC-09. NotificationService, `WORKOUT` and `EXERCISE` are absent.
3. Entity and field names match `task1/spec.md` exactly (`WORKOUT_EXERCISE`, `workout_id`, `exercise_id`, `sets`, `reps`, `weight_kg`).
4. Every call has a direction arrow and a dashed return message; call names are action names (`addExercise()`, `validateEntry()`, `create()`).
5. The Workout tracker has an activation bar for the whole execution, from `addExercise()` to its final return to the User.
6. In each diagram, validation comes before any creation of `WORKOUT_EXERCISE`.
7. Each diagram has at least one `alt`, `opt`, `loop` or `break` fragment: `opt` in SC-07 and SC-09, `break` in SC-08.
8. SC-07: one `WORKOUT_EXERCISE` is created with both `workout_id` and `exercise_id` set (R-06). A note shows that the exercise is picked by name but the call takes `exercise_id`, and no lookup step is added.
9. SC-08: invalid values (`sets` ≤ 0, `reps` ≤ 0 or `weight_kg` < 0) return a validation error and no `WORKOUT_EXERCISE` is created (R-07).
10. SC-09: `weight_kg` = 0 is accepted and a `WORKOUT_EXERCISE` is created (R-07).
11. Each of R-06 and R-07 and each of SC-07, SC-08 and SC-09 appears in at least one note or fragment.
12. No id or file from Tasks 1 and 2 is renamed or changed.
13. Labels use only R-xx, SC-xx and UC-xx ids that exist in tasks 1 and 2.
