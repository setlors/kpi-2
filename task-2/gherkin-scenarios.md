# Gherkin scenarios

```gherkin
Feature: Register an account
  As a guest
  I want to register
  So that my workouts are stored under my own account

  # Scenario: SC-01 Register a new account
    Given a guest who is not logged in
    When the guest submits a username, an email and a password
    Then a USER is created
    And the password is stored only as password_hash

Feature: Log in and protect workout data
  As a guest with an account
  I want to log in
  So that I can see my data and nobody else can

  # Scenario: SC-02 Log in with valid credentials
    Given a guest with a registered account
    When the guest logs in with the correct email and password
    Then an authenticated session is started

  # Scenario: SC-03 Log in with a wrong password
    Given a guest with a registered account
    When the guest logs in with a wrong password
    Then the login is rejected
    And no session is started

  # Scenario: SC-04 Open workouts without logging in
    Given a guest who is not authenticated
    When the guest opens the workout history
    Then access is denied

Feature: Record a workout
  As a user
  I want to record a workout with its date and duration
  So that I know when I trained

  # Scenario: SC-05 Create a workout
    Given an authenticated user
    When the user submits workout_date "2026-10-12" and duration_minutes 60
    Then a WORKOUT is created with this user's user_id

  # Scenario: SC-06 Save a workout without exercises
    Given an authenticated user
    When the user saves a workout with no exercises
    Then the workout is saved without an error

  Feature: Add exercises to a workout
  As a user
  I want to add exercises to a workout with sets, reps and weight
  So that I can see what I actually did

  # Scenario: SC-07 Add an exercise to a workout
    Given an authenticated user with an existing workout
    When the user adds "Bench press" with 3 sets, 10 reps and 60 weight_kg
    Then a WORKOUT_EXERCISE is created
    And it is linked to this workout through workout_id
    And it is linked to "Bench press" through exercise_id

  # Scenario Outline: SC-08 Reject invalid values
    Given an authenticated user with an existing workout
    When the user adds an exercise with <sets> sets, <reps> reps and <weight_kg> weight_kg
    Then the entry is rejected with a validation error

    Examples:
      | sets | reps | weight_kg |
      | 3    | 10   | -5        |
      | 0    | 10   | 40        |
      | 3    | 0    | 40        |

  # Scenario: SC-09 Accept a zero weight
    Given an authenticated user with an existing workout
    When the user adds an exercise with 3 sets, 15 reps and 0 weight_kg
    Then a WORKOUT_EXERCISE is created


Feature: View workout history
  As a user
  I want to look through my past workouts
  So that I can compare them with new ones

  # Scenario: SC-10 View history of my own workouts
    Given an authenticated user with workouts on "2026-10-01" and "2026-10-08"
    And another user with a workout on "2026-10-05"
    When the user opens the history
    Then the list shows "2026-10-08" first and "2026-10-01" second
    And the workout of the other user is not shown

  Feature: Inactivity reminder
  As a user
  I want a reminder when I have not trained for 7 days
  So that I do not forget about training

  # Scenario: SC-11 Send a reminder after 7 days
    Given a user whose latest workout_date was 7 days ago
    And the user has no newer workout
    When 7 days have passed since that workout_date
    Then one reminder request is sent to NotificationService

```
