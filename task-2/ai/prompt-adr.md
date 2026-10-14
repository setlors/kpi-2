## User:

> 10/15/2026 1:33:24

Now write ADRs using MADR structure for the the main decisions in task 2.
Use the MADR structure for each file:

- Title (the decision, short)
- Status (accepted) and Date
- Context and Problem Statement (2-3 sentences: what was inconsistent)
- Considered Options (at least 2-3 per ADR), pros and cons of the Options
- Decision Outcome (chosen option + why)
- Consequences (good and bad)

The decisions:

### Guest actor

I needed a way to show people who are not logged in without changing the Task 1 data model, where only USER exists.
The decision made: add Guest as an actor only, not as an entity. Guest is a person who is not authenticated, and User is a Guest who has an account and is logged in (generalization).

### Ambiguous requirement R-11 (reminder)

It was not clear if the reminder is sent once or every day after day 7, what happens to a user with no workouts.
The decision made: R-11 now sends exactly one request, only for users with at least one WORKOUT, and not again until a new workout is saved.

## Assistant:

> 10/15/2026 1:33:55

I wrote two ADRs in MADR format for the main decisions in Task 2, in `adr/`:

- **`0001-model-guest-as-actor-not-entity.md`:** Guest is modelled as an actor only, not as an entity, so the Task 1 data model (only `USER`) stays unchanged. Guest is a person who is not authenticated, and User is a Guest who has an account and is logged in (generalization). The options compared are Guest as an actor only (chosen), only the User actor, and Guest as an entity.
- **`0002-send-one-reminder-per-inactivity-period.md`:** R-11 sends exactly one reminder request, only for a user with at least one `WORKOUT`, and not again until a new workout is saved. The options compared are one reminder per inactivity period (chosen), a reminder every day after day 7, and one reminder per user ever.

Each file has the structure you asked for: title, status (accepted) and date, context, options with pros and cons, decision outcome and consequences.

- **Dates.** All dates are 2026-10-15. Change them to the real dates of your commits if they differ.

> File: [/mnt/user-data/outputs/adr/0001-model-guest-as-actor-not-entity.md](https://claude.ai/api/organizations/73d8f91b-62e6-4ffb-9597-867d0aa60759/conversations/7ad8d632-520a-42a1-914c-f77baf1c1982/wiggle/download-file?path=%2Fmnt%2Fuser-data%2Foutputs%2Fadr%2F0001-model-guest-as-actor-not-entity.md)

> File: [/mnt/user-data/outputs/adr/0002-send-one-reminder-per-inactivity-period.md](https://claude.ai/api/organizations/73d8f91b-62e6-4ffb-9597-867d0aa60759/conversations/7ad8d632-520a-42a1-914c-f77baf1c1982/wiggle/download-file?path=%2Fmnt%2Fuser-data%2Foutputs%2Fadr%2F0002-send-one-reminder-per-inactivity-period.md)

Powered by Claude Exporter (https://www.ai-chat-exporter.net)
