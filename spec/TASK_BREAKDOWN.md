# Fitness AI App MVP Task Breakdown

Branch: `spec/fitness-mvp-v1`  
Repo: `qinze889/fitness-ai-app`  
Status: Codex execution plan

## 1. Purpose

This document breaks the Fitness AI App MVP into small implementation tasks suitable for Codex.

Codex should not attempt to build the whole app in one pass. Each task should produce a buildable, reviewable commit.

## 2. Execution Rules for Codex

1. Read `README.md`, `design.md`, `spec/MVP_SPEC.md`, `spec/ARCHITECTURE.md`, `spec/ACCEPTANCE.md`, `spec/APK_PLAN.md`, `spec/CODEX_WORKFLOW.md`, and this file before implementation.
2. Create implementation work on a feature branch, preferably `feat/fitness-mvp-v1`.
3. Complete one task at a time.
4. Keep each task small and testable.
5. Do not add out-of-scope features.
6. Do not introduce login, payments, social sharing, cloud sync, wearable integration, marketplace, or computer vision posture detection.
7. Prefer local mock AI first, then add configurable remote AI through Settings.
8. Do not hard-code or commit real remote AI credentials.
9. After each task, report changed files, build result, APK path if available, tests run, and remaining risks.

## 3. Recommended Branch Workflow

```text
main
└── spec/fitness-mvp-v1      # product, design, and engineering contract
└── feat/fitness-mvp-v1      # implementation branch created from spec or main after spec is merged
```

Preferred implementation path:

1. keep this spec branch as documentation contract;
2. create `feat/fitness-mvp-v1` for app code;
3. implement tasks sequentially;
4. open PR back to `main` when the MVP is buildable.

## 4. MVP Task List

## Task 01: Initialize Android Project

### Goal

Create a clean native Android project using Kotlin and Jetpack Compose.

### Requirements

- Create Gradle project structure.
- Configure Kotlin.
- Configure Jetpack Compose.
- Add Material 3 dependency.
- Add app package namespace, for example `com.qinze.fitnessai`.
- Create `MainActivity`.
- Create initial Material 3 theme tokens aligned with `design.md`.
- Show a simple placeholder Home screen.

### Done When

- Project opens as an Android project.
- App can build.
- App can launch and show placeholder screen.
- Placeholder uses Material 3 and does not ignore `design.md`.

### Do Not Do

- Do not implement business logic yet.
- Do not add AI network integration yet.

---

## Task 02: App Navigation Skeleton

### Goal

Create the app navigation foundation.

### Requirements

- Add Navigation Compose.
- Create bottom navigation with:
  - Home;
  - Workout;
  - Calendar;
  - Diet;
  - Settings.
- Add secondary placeholder routes:
  - Exercise Detail;
  - Daily Check-in;
  - AI Advice;
  - Analytics;
  - AI Settings.
- Every placeholder screen should have clear title, purpose text, and not-dead-end navigation.

### Done When

- User can switch bottom tabs.
- Each route shows a visible placeholder.
- Navigation code is centralized in `AppNavGraph` or equivalent.

---

## Task 03: Core Data Models

### Goal

Create Kotlin domain models for the MVP.

### Requirements

Create models for:

- WorkoutPlan;
- WorkoutDay;
- Exercise;
- WorkoutExerciseLog;
- DailyReview;
- AIAdvice;
- DietLog;
- AppSettings;
- AIProviderSettings.

### Done When

- Models compile.
- Models reflect `spec/MVP_SPEC.md` and `spec/ARCHITECTURE.md`.
- No UI depends directly on database entity implementation yet unless necessary.

---

## Task 04: Local Database Foundation

### Goal

Set up Room database for local-first storage.

### Requirements

- Add Room dependencies.
- Create Room entities.
- Create DAO interfaces.
- Create app database.
- Add seed mechanism for exercise library if suitable.

### Done When

- Database compiles.
- App can insert and read sample exercise data.
- No cloud backend is required.

---

## Task 05: Seed Exercise Library

### Goal

Add initial static exercise data.

### Initial Exercises

- Bench Press;
- Dumbbell Fly;
- Push-up;
- Cable Crossover or Chest Expansion equivalent;
- Shoulder Press;
- Lateral Raise;
- Rear Delt Fly;
- Lat Pulldown;
- Seated Row;
- Dumbbell Row;
- Plank;
- Squat or Bodyweight Squat.

### Each Exercise Needs

- name;
- muscle group;
- description;
- effect;
- steps;
- common mistakes;
- safety notes;
- default sets;
- default reps;
- difficulty.

### Done When

- Exercise list is available locally.
- Exercise data can be displayed by UI in later tasks.

---

## Task 06: Workout Plan Generation

### Goal

Create preset weekly plans.

### Requirements

- Support 3-day plan.
- Support 4-day plan.
- Generate WorkoutDay records from selected plan.
- Assign exercises to workout days.

### Done When

- User can select plan type in placeholder/settings area.
- Generated plan can be stored locally.
- Today’s workout can be derived from the plan.

---

## Task 07: Home Dashboard

### Goal

Show useful starting information on app launch.

### Requirements

Display:

- today’s training theme;
- next reminder time;
- current weekly plan type;
- latest daily score;
- latest AI suggestion preview or not-configured state;
- quick button to today’s workout.

Follow `design.md` Home Dashboard contract.

### Done When

- Home screen reads data from repository/ViewModel.
- Empty state is handled clearly.
- User can navigate to today’s workout.
- UI is card-based and matches `design.md`.

---

## Task 08: Today Workout Screen

### Goal

Implement workout execution flow.

### Requirements

- Show today’s workout theme.
- Show exercise cards.
- Show sets, reps, and optional weight.
- Allow status changes:
  - not started;
  - in progress;
  - completed;
  - skipped.
- Allow RPE input, 1 to 10.
- Use distinct visual states for completed and skipped exercises.

### Done When

- User can mark exercises as completed or skipped.
- Completion state persists after app restart.
- User can proceed to daily check-in.

---

## Task 09: Exercise Detail Screen

### Goal

Make exercise information useful.

### Requirements

Display:

- exercise name;
- target muscle;
- training effect;
- steps;
- common mistakes;
- safety notes;
- default sets/reps.

### Done When

- User can tap exercise from workout screen.
- Detail screen opens with correct exercise data.
- Back navigation works.

---

## Task 10: Daily Score Calculation

### Goal

Implement simple scoring.

### Formula

```text
Daily Score = Completion Score * 0.6 + Effort Score * 0.25 + Feedback Score * 0.15
```

Where:

- Completion Score = completed exercises / planned exercises * 100;
- Effort Score = average RPE normalized to 100;
- Feedback Score = 100 if user submits daily summary, otherwise 0.

### Done When

- Score calculation is isolated in a testable function.
- Unit tests cover normal, empty, and partial-completion cases.

---

## Task 11: Daily Check-in Screen

### Goal

Collect post-workout user feedback.

### Requirements

- Text summary input.
- Fields or prompts for:
  - what was completed;
  - what was skipped;
  - body feeling;
  - fatigue;
  - pain or discomfort;
  - diet notes.
- Save check-in locally.
- After save, show action to generate AI advice.

### Done When

- User can submit daily summary.
- Summary persists locally.
- Daily score updates after submission.

---

## Task 12: Local Mock AI Advice

### Goal

Add AI advice flow without depending on a remote provider.

### Requirements

- Create app-level AI advice interface.
- Create local mock implementation.
- Generate deterministic sample feedback based on completion, RPE, pain/fatigue terms, diet notes, and summary.
- Show AI advice result cards.
- Save generated advice locally if persistence is already available.

### Done When

- User can tap generate advice.
- App displays local AI-style advice.
- App does not require remote configuration.
- Warning terms produce cautious output.

---

## Task 13: AI Settings Screen and Provider Configuration

### Goal

Let the user optionally configure a real remote AI provider inside the app.

### Requirements

- Add AI Settings screen under Settings.
- Add provider selector:
  - Local Mock;
  - DeepSeek;
  - OpenAI Compatible.
- Add fields for:
  - enable remote AI toggle;
  - base URL;
  - model;
  - local credential input with masked display;
  - test connection;
  - clear credential.
- Persist provider settings locally.
- Do not hard-code or commit real credentials.
- Do not print full credentials in logs.
- If no remote provider is configured, app must still work with local mock AI.

### Recommended Defaults

```text
Provider: Local Mock
DeepSeek Base URL: https://api.deepseek.com
DeepSeek Model: deepseek-v4-flash
```

### Done When

- User can open AI Settings.
- User can save provider settings.
- User can clear local credential.
- AI-not-configured state is understandable.
- No remote provider is required for normal workout flow.

---

## Task 14: DeepSeek / OpenAI-Compatible AI Client

### Goal

Implement real remote advice generation using the configured provider.

### Requirements

- Add DeepSeek/OpenAI-compatible client using OkHttp or Retrofit.
- Send structured daily advice input.
- Parse structured output into app model.
- Handle network failure, provider error, invalid response, and missing configuration.
- Keep previous local data intact if the request fails.
- Keep the base URL and model configurable through settings.

### Done When

- Real provider can be used after user configuration.
- AI failure does not block workout completion.
- AI failure does not delete daily summary.
- A readable retry/error state is displayed.

### Optional

This task may be postponed until after the local MVP is stable.

---

## Task 15: Calendar Screen

### Goal

Show workout schedule and completion history.

### Requirements

- Show week or month calendar.
- Mark workout days.
- Mark completed, missed, planned, partial, and rest days.
- Tap date to view related workout or summary.

### Done When

- User can visually inspect training schedule.
- Completion status is reflected on calendar.

---

## Task 16: Local Reminder Settings

### Goal

Add workout reminder configuration.

### Requirements

- Enable/disable reminder.
- Set reminder time.
- Store settings in DataStore.
- Schedule local notification.

### Done When

- User can configure reminder time.
- App can schedule local reminder.
- Notification permissions are handled where required.

---

## Task 17: Diet Screen

### Goal

Implement lightweight diet planning and logging.

### Requirements

- Show today’s diet suggestion placeholder.
- Add text fields for breakfast, lunch, dinner, and snack.
- Save diet log locally.
- Add local mock or remote AI diet tip if AI module exists.

### Done When

- User can save daily diet log.
- Diet log persists locally.

---

## Task 18: Analytics Screen

### Goal

Display basic historical progress.

### Requirements

Display:

- weekly completion rate;
- workout days completed;
- average daily score;
- skipped exercise count;
- recent fatigue or RPE trend if available.

### Done When

- Analytics screen summarizes local data.
- Empty states are handled.

---

## Task 19: MVP Polish Pass

### Goal

Make the app usable end to end.

### Requirements

- Fix navigation issues.
- Improve empty states.
- Improve button labels.
- Ensure data persists.
- Ensure no screen is a dead end.
- Remove unused placeholders that confuse the user.
- Verify design consistency against `design.md`.

### Done When

- A user can complete the full core loop without developer intervention.

---

## Task 20: Final MVP Verification

### Goal

Verify the MVP against acceptance criteria.

### Requirements

- Run build.
- Run available tests.
- Manually check flows from `spec/ACCEPTANCE.md`.
- Document known limitations.
- Report Debug APK path.

### Done When

- Acceptance checklist is passed or known failures are documented.
- The app is ready for a review PR.

## 5. Suggested First Codex Prompt

Use this prompt when starting implementation:

```text
Please read README.md, design.md, spec/MVP_SPEC.md, spec/ARCHITECTURE.md, spec/TASK_BREAKDOWN.md, spec/ACCEPTANCE.md, spec/APK_PLAN.md, spec/CODEX_WORKFLOW.md, and spec/REVIEW_PROCESS.md.

Create a new branch named feat/fitness-mvp-v1 from the current spec branch or from main after the spec branch is merged.

Implement Task 01 only: initialize a native Android project using Kotlin, Jetpack Compose, and Material 3. Create a buildable app with a placeholder Home screen using the visual direction from design.md. Do not implement workout logic, AI integration, database, reminders, or credential storage yet.

After implementation, run the available build command, commit the changes, and report changed files, build result, APK path, tests run, and any limitations.
```
