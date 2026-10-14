# Model Guest as an actor, not as an entity

- Status: accepted
- Date: 2026-10-15

## Context and Problem Statement

Some people use the system without being logged in: they register, log in, or try to open workout data and get denied (R-01, R-02, R-03, R-04). The Task 1 data model has only `USER`, and a person who is not logged in has no stored record. We need a way to show these people in the requirements and the use case diagram without breaking consistency with the data model.

## Considered Options

- **Option A:** add Guest as an actor only (no entity). Guest is a person who is not authenticated. User is a Guest who has an account and is logged in (generalization).
- **Option B:** use only the User actor. Registration and login are written as a step that happens before the "real" use cases.
- **Option C:** add Guest as an entity (a `GUEST` table) in the data model.

## Pros and Cons of the Options

### Option A: Guest as actor, not entity

- Good, because registration, login and "access denied" (R-01 to R-04) get a clear actor.
- Good, because the data model from Task 1 does not change: a guest has no stored record.
- Good, because the generalization (User is a Guest) is shown in the diagram, as the task requires.
- Bad, because the actor list has one actor that has no entity, so `spec.md` must explain why.

### Option B: only User

- Good, because there is one human actor and nothing to explain.
- Bad, because R-04 ("deny access while not authenticated") and R-01 (register) have no actor who can do them.
- Bad, because the diagram loses the generalization the task asks for.

### Option C: Guest as an entity

- Good, because every actor would have a matching entity.
- Bad, because it stores people with no data and no account, and adds a table that has no fields of its own.
- Bad, because it changes the Task 1 model, which Task 2 must not contradict.

## Decision Outcome

Chosen option: **Option A**, because it covers the unauthenticated flows without changing the Task 1 data model. `spec.md` defines Guest as a person who is not authenticated (with or without an account) and says that Guest is not an entity. All login and access rules use "guest" (R-02, R-03, R-04), and the diagram shows User as a specialization of Guest.

### Consequences

- Good, because every requirement about registration, login and denied access has an actor, and the actor and entity names still match Task 1.
- Good, because a reviewer can see the actor-to-entity mapping: User maps to `USER`, Guest and NotificationService have no entity.
- Bad, because "guest" can be read as "has no account", so the definition in `spec.md` has to stay (a guest may already have an account).
- Bad, because User inherits Guest's use cases, so formally a logged-in User could "register" again. The generalization is kept for the task, and this edge case is not modelled.
