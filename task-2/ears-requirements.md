# EARS requirements

Patterns used:

- `WHEN <trigger>, the system SHALL <reaction>` for events
- `WHILE <state>, the system SHALL <reaction>` for states
- `IF <unwanted situation>, THEN the system SHALL <reaction>` for errors
- `The system SHALL <reaction>` for rules that always apply

| ID   | Requirement                                                                                                                                                                                                                                 |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R-01 | WHEN a guest submits a username, an email and a password, the system SHALL create a `USER` and store the password only as `password_hash`.                                                                                                  |
| R-02 | WHEN a user submits the email of an existing `USER` and a password that matches `password_hash`, the system SHALL start an authenticated session.                                                                                           |
| R-03 | IF the email does not belong to any `USER`, or the password does not match `password_hash`, THEN the system SHALL reject the login and SHALL NOT start a session.                                                                           |
| R-04 | WHILE a visitor is not authenticated, the system SHALL deny access to workout data.                                                                                                                                                         |
| R-05 | WHEN an authenticated user submits `workout_date` and `duration_minutes`, the system SHALL create a `WORKOUT` linked to that user through `user_id`.                                                                                        |
| R-06 | WHEN an authenticated user adds an exercise to his or her workout with `sets`, `reps` and `weight_kg`, the system SHALL create a `WORKOUT_EXERCISE` linked to that workout through `workout_id` and to that exercise through `exercise_id`. |
| R-07 | IF `sets` or `reps` is not a positive integer, or `weight_kg` is negative, THEN the system SHALL reject the entry and return a validation error.                                                                                            |
| R-08 | WHEN a user saves a `WORKOUT` that has no `WORKOUT_EXERCISE` records, the system SHALL save it without an error.                                                                                                                            |
| R-09 | IF a user requests deletion of an `EXERCISE` that is referenced by a `WORKOUT_EXERCISE`, THEN the system SHALL refuse the deletion.                                                                                                         |
| R-10 | WHEN a user opens the history, the system SHALL list only the workouts of that user, with the newest `workout_date` first.                                                                                                                  |
| R-11 | WHEN 7 days have passed since the latest `workout_date` of a user, the system SHALL send a reminder request to NotificationService.                                                                                                         |

## Notes

- `weight_kg` may be 0 (an exercise without extra weight), so R-07 rejects only negative values.
- R-07, R-08 and R-09 repeat the acceptance criteria from `task1/spec.md`.
- R-11 sends a request only. Delivering the message is the job of NotificationService.
