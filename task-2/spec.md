## Intent

Describe what the workout tracker does, who uses it and how each behavior can be checked.

## Actors

| Actor               | Type                            | Main goal                                     |
| ------------------- | ------------------------------- | --------------------------------------------- |
| Guest               | Human                           | Create an account                             |
| User                | Human (specialization of Guest) | Record workouts and follow his or her history |
| NotificationService | External system                 | Deliver reminders to the user                 |

- User maps to the `USER` entity from Task 1.
- Guest is not an entity, because a visitor without an account has no stored record.
- NotificationService is outside the system. We only send it a request, we do not build it.

## Main goals

1. A guest can register and then log in.
2. A user can record a workout and the exercises done in it (`sets`, `reps`, `weight_kg`).
3. A user can look through his or her own workout history.
4. A user who stops training gets a reminder through NotificationService.

## Scope of the scenarios

In scope: registration, login, recording workouts and exercises, validation of entered values, history, reminders.

Out of scope: managing the exercise catalog (adding or editing `EXERCISE` records), password reset, payments, personal records. Deleting an exercise is described by one requirement (R-09), but has no use case yet. This is the uncovered requirement in the traceability matrix.

## Acceptance criteria

- There are at least 2 actor types: a human and an external system.
- Every requirement is written in EARS (WHEN / WHILE / IF ... THEN ... SHALL) and has a unique ID (R-01, R-02, ...).
- Every requirement is testable: it names a trigger or state and one observable reaction.
- Every user story (US-xx) and every Gherkin scenario (SC-xx) points to at least one requirement ID.
- Entity and field names match `task1/spec.md` exactly (`USER`, `WORKOUT`, `EXERCISE`, `WORKOUT_EXERCISE`, `duration_minutes`, `weight_kg`, `password_hash`).
- The requirements for empty workouts, invalid values and deleting a used exercise repeat the acceptance criteria of Task 1.
- The traceability matrix is many-to-many, not 1:1, and marks uncovered requirements with an explanation.
- The use case diagram has at least 2 actor types, `include`, `extend` and generalization.
