## Intent

Show the architecture of the workout tracker as a component diagram: which parts the system has, which interfaces they use, and where the system ends. The diagram must follow the data model (Task 1), requirements (Task 2), sequence diagrams (Task 3) and the process (Task 4).

## Components

Inside the system:

- UI: the client that Guest and User work with.
- AuthService: registration, login, access check (UC-01, UC-02, UC-03, R-01 to R-04).
- WorkoutService: creates workouts, adds exercises, validates values (UC-04, UC-05, R-05 to R-08).
- HistoryService: shows the workouts of one user, newest first (UC-06, R-10).
- ReminderScheduler: finds users who did not train for 7 days and asks for a reminder (UC-07, R-11).
- DataAccess: reads and writes USER, WORKOUT, EXERCISE and WORKOUT_EXERCISE.
- Database: stores the four tables from Task 1.

Outside the system:

- NotificationService: sends the reminder to the user.

## Interfaces

- Auth (`checkAccess`): provided by AuthService, required by WorkoutService and HistoryService.
- Workout (`addExercise`, `validateEntry`, `create`): provided by WorkoutService, required by UI.
- History (`getHistory`): provided by HistoryService, required by UI.
- Repository: provided by DataAccess, required by all services.
- Notification (`sendReminder`): provided by NotificationService, required by ReminderScheduler.

## Acceptance criteria

1. The diagram has a file with the source and a rendered image.
2. The system boundary is clear: everything we build is inside, NotificationService is outside.
3. Every component is linked to at least one UC or R from Task 2.
4. Every dependency goes through an interface, with a provided and a required side.
5. Method names are the same as in Tasks 3 and 4 (`addExercise`, `validateEntry`, `create`, `checkAccess`).
6. Only WorkoutService validates `sets`, `reps` and `weight_kg`.
7. Only DataAccess talks to the database.
8. Entity and field names are the same as in `task1/spec.md`.
9. No file or id from Tasks 1 to 4 is changed.
