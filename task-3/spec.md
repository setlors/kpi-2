## Intent

Detail the dynamics of **UC-05 Add exercise to workout** (from `task2/use-case.md`): how a User adds one exercise with `sets`, `reps` and `weight_kg` to an existing workout, and how invalid input and access problems are handled.

## Chosen use case

UC-05 (extends UC-04, includes UC-06 Validate exercise values; the access check UC-03 comes from the base UC-04). Requirements: R-06, R-07, R-04 (via UC-03). Scenarios: SC-07, SC-08, SC-09. Story: US-04.

## Participants

| Participant        | Kind          | Maps to                                                                       |
| ------------------ | ------------- | ----------------------------------------------------------------------------- |
| User               | Actor (human) | `USER` entity; task 2 actor                                                   |
| Workout tracker    | System        | the system boundary from `task2/use-case.md`                                  |
| `WORKOUT`          | Data object   | `WORKOUT` (`id`, `user_id`)                                                   |
| `EXERCISE`         | Data object   | `EXERCISE` (`id`, `name`)                                                     |
| `WORKOUT_EXERCISE` | Data object   | `WORKOUT_EXERCISE` (`workout_id`, `exercise_id`, `sets`, `reps`, `weight_kg`) |

Not used here: Guest (not authenticated, see step 2a) and NotificationService (no reminder in this flow).

## Preconditions

The user has an existing `WORKOUT` (created by UC-04, R-05, SC-05). The `EXERCISE` record exists (the catalog is managed outside the scope, see `task2/spec.md`).

## Scenario steps

1. The User sends `addExercise(workout_id, exercise_id, sets, reps, weight_kg)`, e.g. "Bench press", 3 sets, 10 reps, 60 `weight_kg` (SC-07).
2. The system checks that the user is authenticated (UC-03, R-04).
3. The system loads the `WORKOUT` by `workout_id` and the `EXERCISE` by `exercise_id`.
4. The system checks that `WORKOUT.user_id` is the current user ("his or her workout", R-06) and that the `EXERCISE` exists.
5. The system validates the values (UC-06, R-07): `sets` and `reps` are positive integers, `weight_kg` is not negative.
6. The system creates a `WORKOUT_EXERCISE` linked through `workout_id` and `exercise_id` (R-06).
7. The system confirms. The User may repeat from step 1 or stop. Stopping with zero exercises is valid (R-08, UC-04).

Alternatives:

- 2a. Not authenticated: access is denied, nothing is created (R-04).
- 4a. The `WORKOUT` does not exist or belongs to another user (A-1), or the `EXERCISE` does not exist (A-2): the request is rejected.
- 5a. Invalid values (`sets` 0, `reps` 0 or negative `weight_kg`): validation error, no `WORKOUT_EXERCISE` is created (R-07, SC-08).
- 5b. `weight_kg` is 0: valid, continue with step 6 (SC-09).

## Acceptance criteria

- The diagram shows every step above as a message and contains `loop`, `alt` and `break` blocks.
- Participant and parameter names match task 1 exactly (`WORKOUT`, `EXERCISE`, `WORKOUT_EXERCISE`, `workout_id`, `exercise_id`, `sets`, `reps`, `weight_kg`).
- `WORKOUT_EXERCISE` is created only on the success branch.
- Every branch traces to an R-xx or SC-xx. Branches without one are marked as assumptions (A-1, A-2, and 2a for SC).
- Only actors from task 2 are used. No IDs or files of tasks 1 and 2 are changed.
