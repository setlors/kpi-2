# Audit of AI output

## Mistake 1: inconsistent actor names

Actor names. spec.md and the diagram use Guest (logs in) / User / NotificationService, but R-02, R-03, US-02, SC-02, SC-03 said "user" logs in, and R-04 and SC-04 said "visitor", which is not in the actor list.
Fix: changed R-02, R-03, R-04, SC-02, SC-03, SC-04 to "guest".

## Mistake 2: ambiguous requirement R-11 (reminder)

It was not clear if the reminder is sent once or every day after day 7, what happens to a user with no workouts.
Fix: R-11 now sends exactly one request, only for users with at least one WORKOUT, and not again until a new workout is saved.

## Mistake 3: R-09 gives an action to an actor that does not exist

R-09 said "a user requests deletion of an EXERCISE", but spec.md puts exercise catalog management out of scope, so no actor may delete exercises.  
Fix: R-09 now does not name any actor and says "at least one WORKOUT_EXERCISE (of any user) references it".
