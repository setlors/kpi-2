# Gherkin scenarios

```gherkin
Feature: Accounts

  # R-01
  Scenario: SC-01 Register a new account
    Given a guest who is not logged in
    When the guest submits a username, an email and a password
    Then a USER is created
    And the password is stored only as password_hash

  # R-02
  Scenario: SC-02 Log in with valid credentials
    Given a registered user
    When the user logs in with the correct email and password
    Then an authenticated session is started

  # R-03
  Scenario: SC-03 Log in with a wrong password
    Given a registered user
    When the user logs in with a wrong password
    Then the login is rejected
    And no session is started

  # R-04
  Scenario: SC-04 Open workouts without logging in
    Given a visitor who is not authenticated
    When the visitor opens the workout history
    Then access is denied


Feature: Workouts

  # R-05
  Scenario: SC-05 Create a workout
    Given an authenticated user
    When the user submits workout_date "2026-10-12" and duration_minutes 60
    Then a WORKOUT is created with this user's user_id

  # R-08
  Scenario: SC-06 Save a workout without exercises
    Given an authenticated user
    When the user saves a workout with no exercises
    Then the workout is saved without an error

  # R-06
  Scenario: SC-07 Add an exercise to a workout
    Given an authenticated user with an existing workout
    When the user adds "Bench press" with 3 sets, 10 reps and 60 weight_kg
    Then a WORKOUT_EXERCISE is created
    And it is linked to this workout through workout_id
    And it is linked to "Bench press" through exercise_id

  # R-07
  Scenario Outline: SC-08 Reject invalid values
    Given an authenticated user with an existing workout
    When the user adds an exercise with <sets> sets, <reps> reps and <weight_kg> weight_kg
    Then the entry is rejected with a validation error

    Examples:
      | sets | reps | weight_kg |
      | 3    | 10   | -5        |
      | 0    | 10   | 40        |
      | 3    | 0    | 40        |

  # R-07
  Scenario: SC-09 Accept a zero weight
    Given an authenticated user with an existing workout
    When the user adds an exercise with 3 sets, 15 reps and 0 weight_kg
    Then a WORKOUT_EXERCISE is created


Feature: History and reminders

  # R-10
  Scenario: SC-10 View history of my own workouts
    Given an authenticated user with workouts on "2026-10-01" and "2026-10-08"
    And another user with a workout on "2026-10-05"
    When the user opens the history
    Then the list shows "2026-10-08" first and "2026-10-01" second
    And the workout of the other user is not shown

  # R-11
  Scenario: SC-11 Send a reminder after 7 days
    Given a user whose latest workout_date was 7 days ago
    When the 7th day passes
    Then a reminder request is sent to NotificationService
```
