## Сутності

### User
- id: number (PK)
- username: string
- email: string
- password_hash: string

### Workout
- id: number (PK)
- user_id: number (FK)
- workout_date: date
- duration: number

### Exercise
- id: number (PK)
- name: string
- muscle_group: string

### WorkoutExercise
- id: number (PK)
- workout_id: number (FK)
- exercise_id: number (FK)
- sets: number
- reps: number
- weight: number

## Зв'язки

### User — Workout: 1:N
Один користувач має багато тренувань; кожне тренування належить рівно одному користувачу.

### Workout — WorkoutExercise: 1:N
Одне тренування складається з кількох виконаних вправ; кожен запис WorkoutExercise належить рівно одному тренуванню.

### Exercise — WorkoutExercise: 1:N
Одна вправа може зустрічатися в багатьох виконаннях (у різних тренуваннях); кожен запис WorkoutExercise посилається рівно на одну вправу.

## Критерії прийняття
- Кожне тренування (Workout) прив'язане рівно до одного користувача (user_id обов'язкове, FK на User.id).
- Кожен запис WorkoutExercise прив'язаний рівно до одного тренування і рівно до однієї вправи (workout_id і exercise_id обов'язкові).
- sets, reps — цілі додатні числа; weight_kg — невід'ємне число (0 допустимо для вправ без обтяження).
- Тренування без жодного запису WorkoutExercise — допустимий стан (наприклад, ще не заповнене), але не повинно вважатися помилкою моделі.
- Видалення Exercise, на яку є посилання в WorkoutExercise, заборонене або обробляється явно (щоб не лишались "осиротілі" записи).
- Типи первинних і зовнішніх ключів узгоджені: всюди number, без змішування з string/UUID.
- Назви полів у spec.md, ER-діаграмі та коді повністю збігаються (наприклад, weight_kg — однаково всюди, а не weight в одному місці й подібна назва в іншому).
