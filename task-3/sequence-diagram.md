# Sequence diagram: UC-05 Add exercise to workout

```mermaid
%%{init: {"sequence": {"mirrorActors": false}}}%%
sequenceDiagram
    actor U as User
    participant S as "Workout tracker"
    participant W as WORKOUT
    participant E as EXERCISE
    participant WE as WORKOUT_EXERCISE

    loop while the user adds exercises (zero is valid, R-08)
        U->>S: addExercise(workout_id, exercise_id, sets, reps, weight_kg)
        S->>S: checkAccess() [UC-03, R-04]
        break not authenticated
            S-->>U: accessDenied
        end
        S->>W: findWorkout(workout_id)
        W-->>S: WORKOUT(id, user_id)
        S->>E: findExercise(exercise_id)
        E-->>S: EXERCISE(id)
        break workout not the user's or exercise missing (A-1, A-2)
            S-->>U: rejected
        end
        S->>S: validateExerciseValues(sets, reps, weight_kg) [R-07, SC-08, SC-09]
        alt values valid (weight_kg 0 is allowed)
            S->>WE: createWorkoutExercise(workout_id, exercise_id, sets, reps, weight_kg)
            WE-->>S: WORKOUT_EXERCISE(id)
            S-->>U: exerciseAdded
        else sets or reps not positive, or weight_kg negative
            S-->>U: validationError
        end
    end
```

## How the diagram details UC-05

UC-05 only says that a user adds an exercise to a workout. The diagram shows the order of the work: access check (UC-03, R-04), lookup of the `WORKOUT` (owned by the user, R-06) and the `EXERCISE`, included validation (UC-06, R-07) and the creation of the `WORKOUT_EXERCISE` with `workout_id` and `exercise_id` (R-06). The `break` blocks are early exits (no access, wrong workout or exercise), and the final `alt` covers SC-08 (invalid values are rejected, nothing is created) and SC-09 (`weight_kg` 0 passes). The `loop` is the optional extension of UC-04: it can run zero times (R-08, SC-06) or many times.

## Link to the data model

Every object is a task 1 entity and every message parameter is a field of `WORKOUT_EXERCISE`. `WORKOUT.user_id` is used to check ownership, and the new row gets `workout_id` and `exercise_id` as foreign keys, matching the 1:N relations in `task1/spec.md`.
