## Сутності

### USER

- id: int (PK)
- username: string
- email: string
- password_hash: string

### WORKOUT

- id: int (PK)
- user_id: int (FK)
- workout_date: date
- duration: int

### EXERCISE

- id: int (PK)
- name: string
- muscle_group: string

### WORKOUT_EXERCISE

- id: int (PK)
- workout_id: int (FK)
- exercise_id: int (FK)
- sets: int
- reps: int
- weight: decimal

## Зв'язки

### USER — WORKOUT: 1:N

Один користувач має багато тренувань; кожне тренування належить рівно одному користувачу.

### WORKOUT — WORKOUT_EXERCISE: 1:N

Одне тренування складається з кількох виконаних вправ; кожен запис WORKOUT_EXERCISE належить рівно одному тренуванню.

### EXERCISE — WORKOUT_EXERCISE: 1:N

Одна вправа може зустрічатися в багатьох виконаннях (у різних тренуваннях); кожен запис WORKOUT_EXERCISE посилається рівно на одну вправу.

## Критерії прийняття

- Кожне тренування (Workout) прив'язане рівно до одного користувача (user_id обов'язкове, FK на User.id).
- Кожен запис WORKOUT_EXERCISE прив'язаний рівно до одного тренування і рівно до однієї вправи (workout_id і exercise_id обов'язкові).
- sets, reps — цілі додатні числа; weight_kg — невід'ємне число (0 допустимо для вправ без обтяження).
- Тренування без жодного запису WORKOUT_EXERCISE — допустимий стан (наприклад, ще не заповнене), але не повинно вважатися помилкою моделі.
- Видалення EXERCISE, на яку є посилання в WORKOUT_EXERCISE, заборонене або обробляється явно (щоб не лишались "осиротілі" записи).
- Типи первинних і зовнішніх ключів узгоджені: всюди INTEGER, без змішування з string/UUID.
- Назви полів у spec.md, ER-діаграмі та коді повністю збігаються (наприклад, weight_kg — однаково всюди, а не weight в одному місці й подібна назва в іншому).
