# Audit of AI output

## Mistake 1: interface `Storage` was in the diagram but not in spec.md

The AI drew an interface `Storage` between DataAccess and Database, but `spec.md` listed only five interfaces (Auth, Workout, History, Repository, Notification). Without `Storage`, the dependency DataAccess → Database would have no interface, which breaks criterion 4 (every dependency goes through an interface, with a provided and a required side)
Fix: the Interfaces section of `spec.md` now has an extra line: "Storage: provided by Database, required by DataAccess". The diagram did not change. Criterion 4 now holds for every dependency in the diagram.

## Mistake 2: UI was linked to registration and login without any interface

The AI put UC-01 and UC-02 on the UI component. In task 2 these are "Register" and "Log in" (R-01, R-02, R-03). But in `spec.md` the UI requires only the Workout and History interfaces, and the Auth interface has only `checkAccess`, which is required by WorkoutService and HistoryService. So the diagram said that the UI takes part in UC-01 and UC-02, while no dependency from UI to AuthService existed.
Fix: UC-01 and UC-02 were removed from the UI label in the diagram and in `spec.md`. The UI is now linked to UC-04, UC-05 and UC-06. UC-01 and UC-02 stay on AuthService, which does registration and login.

## Mistake 3: UC and R ids on four components were not in spec.md

Criterion 3 says that every component is linked to at least one UC or R. `spec.md` gave ids only for AuthService, WorkoutService, HistoryService and ReminderScheduler. The diagram also showed ids on UI, DataAccess, Database and NotificationService, but the AI chose them without any source in the spec, so the diagram and the spec did not match.
Fix: the ids were written into `spec.md`, and each one now has a source. Every id used exists in `use-case.md` or `ears-requirements.md`, so criterion 9 is not broken.
