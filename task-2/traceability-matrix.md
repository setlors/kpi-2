# Traceability matrix

Links: requirement (R-xx) ↔ Gherkin scenario (SC-xx) ↔ use case (UC-xx).
The links are many-to-many, not 1:1.

## Requirement → scenarios, use cases

| Requirement | Scenarios           | Use cases           | Status        |
| ----------- | ------------------- | ------------------- | ------------- |
| R-01        | SC-01               | UC-01               | Covered       |
| R-02        | SC-02               | UC-02               | Covered       |
| R-03        | SC-03               | UC-02               | **Partial**   |
| R-04        | SC-04               | UC-03, UC-04, UC-06 | **Partial**   |
| R-05        | SC-05               | UC-04               | Covered       |
| R-06        | SC-07               | UC-05               | Covered       |
| R-07        | SC-08, SC-09        | none                | **Uncovered** |
| R-08        | SC-06               | UC-04, UC-05        | Covered       |
| R-09        | none                | none                | **Uncovered** |
| R-10        | SC-10               | UC-06               | Covered       |
| R-11        | SC-11, SC-12, SC-13 | UC-07               | Covered       |

Status: Covered = linked to a scenario and a use case, and the scenarios test the whole requirement. **Partial** = linked, but at least one case of the requirement has no scenario. **Uncovered** = the requirement has no use case or no scenario.

## Many-to-many examples

- One requirement, several use cases: R-04 → UC-03, UC-04, UC-06 (UC-04 and UC-06 include UC-03). R-08 → UC-04, UC-05.
- One use case, several requirements: UC-04 → R-05, R-08, R-04. UC-02 → R-02, R-03.
- One requirement, several scenarios: R-07 → SC-08 (invalid values) and SC-09 (zero weight is accepted). R-11 → SC-11, SC-12, SC-13.

## Uncovered requirements

**R-09** (refuse deletion of an `EXERCISE` that is referenced by a `WORKOUT_EXERCISE`) has no scenario and no use case.
Reason: managing the exercise catalog (adding, editing or deleting `EXERCISE` records) is out of scope in `spec.md`, so there is no actor flow to model yet. The rule stays because it repeats the Task 1 acceptance criterion and protects history data. It will be covered when a catalog management use case is added.

**R-07** (reject invalid `sets`, `reps` and `weight_kg`) has scenarios (SC-08, SC-09) but no use case.
Reason: validation is a rule inside "Add exercise to workout" (UC-05), not a goal of an actor, so it was not kept as a separate use case. The requirement is tested, but it is not traced to a use case. It could be covered by an `include` of UC-05 if a separate validation use case is added.

## Partially covered requirements

**R-03** (reject the login and start no session) has two cases: the email does not belong to any `USER`, or the password does not match `password_hash`. SC-03 tests only the wrong password. The case of an unknown email has no scenario yet.

**R-04** (deny access to workout data while a guest is not authenticated) is about all workout data, but SC-04 tests only opening the workout history. A guest who tries to create a workout or add an exercise is not tested yet.
