# Use case diagram

Mermaid has no native UML use case diagram, so it is drawn as a `flowchart`:

- stadium shape = actor, ellipse = use case
- the dashed frame = system boundary (actors are outside, use cases are inside)
- solid line = association
- dashed arrow with `«include»` = base use case → included use case
- dashed arrow with `«extend»` = extending use case → base use case
- `generalization` arrow = specialization → general actor (User is a Guest)

```mermaid
flowchart LR
    Guest([Guest])
    User([User])
    NS([NotificationService<br/>external system])

    subgraph SYS[Workout tracker]
        UC01((UC-01<br/>Register))
        UC02((UC-02<br/>Log in))
        UC03((UC-03<br/>Check access))
        UC04((UC-04<br/>Record workout))
        UC05((UC-05<br/>Add exercise<br/>to workout))
        UC06((UC-06<br/>View workout<br/>history))
        UC07((UC-07<br/>Send inactivity<br/>reminder))
    end

    %% generalization
    User -- "generalization (User is a Guest)" --> Guest

    %% associations
    Guest --- UC01
    Guest --- UC02
    User --- UC04
    User --- UC06
    UC07 --- NS

    %% include
    UC04 -. "«include»" .-> UC03
    UC06 -. "«include»" .-> UC03

    %% extend
    UC05 -. "«extend»<br/>optional step" .-> UC04

    style SYS fill:none,stroke:#999,stroke-dasharray:5 5
```

## Use cases

| ID    | Use case                 | Actor               | Requirements     |
| ----- | ------------------------ | ------------------- | ---------------- |
| UC-01 | Register                 | Guest               | R-01             |
| UC-02 | Log in                   | Guest               | R-02, R-03       |
| UC-03 | Check access             | (included)          | R-04             |
| UC-04 | Record workout           | User                | R-05, R-08, R-04 |
| UC-05 | Add exercise to workout  | User                | R-06, R-08       |
| UC-06 | View workout history     | User                | R-10, R-04       |
| UC-07 | Send inactivity reminder | NotificationService | R-11             |

A use case covers a requirement when it performs it directly or reaches it through `include` or `extend`. So one requirement can belong to several use cases: R-04 belongs to UC-03, UC-04 and UC-06, and R-08 belongs to UC-04 and UC-05.

## Why these relations

- **Generalization (User → Guest):** `spec.md` defines User as a specialization of Guest. A User can do everything a Guest can.
- **`include` UC-04, UC-06 → UC-03:** recording and viewing workouts always require the access check, so it is not optional (R-04).
- **`extend` UC-05 → UC-04:** a workout can be saved with no exercises (R-08), so adding exercises is an optional step of recording a workout.
- **NotificationService** only receives a reminder request (R-11). Delivery is outside the system.
