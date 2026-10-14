# Send one reminder per inactivity period, only to users with a workout

- Status: accepted
- Date: 2026-10-15

## Context and Problem Statement

R-11 said "WHEN 7 days have passed since the latest `workout_date`, send a reminder request", but it did not say whether the reminder is sent once or every day after day 7, or what happens to a user who has never saved a workout (there is no `workout_date` to count from). The wording was also different in different files: "7 days" in R-11 and "a week" in US-06. The requirement was therefore not fully testable.

## Considered Options

- **Option A:** send exactly one reminder request, only for a user with at least one `WORKOUT`, and do not send another until the user saves a new `WORKOUT`.
- **Option B:** send a reminder every day after day 7 until the user trains.
- **Option C:** send a reminder once per user, ever.

## Pros and Cons of the Options

### Option A: one per inactivity period

- Good, because it has one trigger and one observable reaction, so it can be tested (SC-11, SC-12, SC-13).
- Good, because the user is reminded again after a new break, not only the first time.
- Bad, because the system must remember that a reminder was already sent for the latest workout.

### Option B: every day after day 7

- Good, because the user is very unlikely to miss it.
- Bad, because it can annoy the user, and the number of requests to NotificationService is unlimited.
- Bad, because "every day" needs its own stop condition and more scenarios.

### Option C: once per user, ever

- Good, because it is the simplest rule.
- Bad, because it does not match US-06: after one break the user would never be reminded again.

## Decision Outcome

Chosen option: **Option A**, because it keeps the story (US-06) useful without spamming the user and stays simple enough to test. A new acceptance criterion says a time-based requirement must state its threshold, how often it fires and the case with no data. R-11 now sends one request, only for a user with at least one `WORKOUT`, and not again until a new workout is saved. US-06 says "7 days", SC-11 was reworded, and SC-12 (no second reminder) and SC-13 (no reminder without workouts) were added.

### Consequences

- Good, because R-11 now has exact behavior and three scenarios, and the traceability matrix links it to SC-11, SC-12 and SC-13.
- Good, because a user with no workouts is handled explicitly.
- Bad, because the design needs a "reminder already sent" state, which is a detail for the later architecture task.
- Bad, because a user who never trains after the first workout gets only one reminder.
