# Traceability matrix

Links: requirement (R-xx) ↔ user story (US-xx) ↔ Gherkin scenario (SC-xx) ↔ use case (UC-xx).
The links are many-to-many, not 1:1.

## Requirement → stories, scenarios, use cases

| Requirement | User stories | Scenarios    | Use cases           | Status        |
| ----------- | ------------ | ------------ | ------------------- | ------------- |
| R-01        | US-01        | SC-01        | UC-01               | Covered       |
| R-02        | US-02        | SC-02        | UC-02               | Covered       |
| R-03        | US-02        | SC-03        | UC-02               | Covered       |
| R-04        | US-02        | SC-04        | UC-03, UC-04, UC-07 | Covered       |
| R-05        | US-03        | SC-05        | UC-04               | Covered       |
| R-06        | US-04        | SC-07        | UC-05               | Covered       |
| R-07        | US-04        | SC-08, SC-09 | UC-06, UC-05        | Covered       |
| R-08        | US-03        | SC-06        | UC-04, UC-05        | Covered       |
| R-09        | US-07        | none         | none                | **Uncovered** |
| R-10        | US-05        | SC-10        | UC-07               | Covered       |
| R-11        | US-06        | SC-11        | UC-08               | Covered       |

## Use case → requirements (reverse view)

| Use case | Requirements                                  |
| -------- | --------------------------------------------- |
| UC-01    | R-01                                          |
| UC-02    | R-02, R-03                                    |
| UC-03    | R-04                                          |
| UC-04    | R-05, R-08, R-04 (via include)                |
| UC-05    | R-06, R-07 (via include), R-08 (extend point) |
| UC-06    | R-07                                          |
| UC-07    | R-10, R-04 (via include)                      |
| UC-08    | R-11                                          |

## Many-to-many examples

- One requirement, several scenarios: R-07 → SC-08 (invalid values) and SC-09 (zero weight is accepted).
- One requirement, several use cases: R-04 → UC-03, and also UC-04 and UC-07 because both include UC-03.
- One story, several requirements: US-02 → R-02, R-03, R-04. US-04 → R-06, R-07.
- One use case, several requirements: UC-04 → R-05, R-08. UC-05 → R-06, R-07, R-08.

## Uncovered requirement

**R-09** (refuse deletion of an `EXERCISE` that is referenced by a `WORKOUT_EXERCISE`) has a user story (US-07) but no Gherkin scenario and no use case.

Reason: managing the exercise catalog (adding, editing or deleting `EXERCISE` records) is out of scope in `spec.md`, so there is no actor flow to model yet. The rule itself stays valid because it repeats the Task 1 acceptance criterion and protects history data. It will be covered when a catalog management use case is added.
