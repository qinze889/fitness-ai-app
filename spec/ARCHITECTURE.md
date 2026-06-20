# Fitness AI App Architecture Spec

Branch: `spec/fitness-mvp-v1`  
Repo: `qinze889/fitness-ai-app`  
Status: Architecture contract for MVP implementation

## 1. Architecture Goal

This document defines the technical direction for the first usable Android MVP of Fitness AI App.

The goal is not to build a complex commercial platform. The goal is to build a stable, local-first Android app that can support:

1. weekly workout planning;
2. workout calendar and local reminders;
3. exercise library and exercise detail pages;
4. workout completion logging;
5. daily summary input;
6. AI-generated review and optimization suggestions;
7. simple diet logging and analytics.

The architecture should make the first implementation easy for Codex to execute step by step.

## 2. Chosen MVP Stack

Use native Android for MVP v1.

```text
Language: Kotlin
UI: Jetpack Compose
Architecture pattern: MVVM
State: ViewModel + StateFlow
Navigation: Jetpack Navigation Compose
Local database: Room
Settings storage: DataStore
Background work: WorkManager where needed
Local notifications: Android notification channel + scheduled local reminder
Networking: OkHttp or Retrofit
JSON: Kotlinx Serialization or Moshi
Minimum backend: none required for MVP v1
AI provider: hidden behind AIService abstraction
```

## 3. Why Native Android

Native Android is preferred for this MVP because the app depends on platform-level features:

- local notification reminders;
- local database persistence;
- Android speech-to-text integration later;
- permission handling;
- stable offline-first behavior;
- long-term maintainability for a personal Android app.

React Native or Expo can still be considered later for a fast cross-platform prototype, but it should not be mixed into this MVP branch.

## 4. Project Structure

Codex should create a clean Android project structure similar to:

```text
fitness-ai-app/
  README.md
  spec/
    MVP_SPEC.md
    ARCHITECTURE.md
    TASK_BREAKDOWN.md
    ACCEPTANCE.md
  app/
    build.gradle.kts
    src/main/
      AndroidManifest.xml
      java/com/qinze/fitnessai/
        MainActivity.kt
        FitnessAiApp.kt
        core/
          data/
          database/
          model/
          notification/
          network/
          settings/
          ui/
        feature/
          home/
          calendar/
          workout/
          exercise/
          checkin/
          ai/
          diet/
          analytics/
          settings/
        navigation/
          AppNavGraph.kt
```

If Gradle or Android Studio initialization creates a different valid structure, Codex may adapt, but feature boundaries must remain clear.

## 5. Module Boundaries

### 5.1 Core Layer

The `core` layer contains reusable infrastructure:

- data models;
- Room database setup;
- repository interfaces;
- notification scheduler;
- AI network client abstraction;
- app settings storage;
- shared UI components.

The core layer must not depend on feature screens.

### 5.2 Feature Layer

Each feature owns its UI, ViewModel, and screen-specific state:

- `home`: dashboard and quick status;
- `calendar`: workout calendar and date status;
- `workout`: today workout execution;
- `exercise`: exercise detail;
- `checkin`: daily text/voice summary;
- `ai`: AI review rendering and retry behavior;
- `diet`: diet logging and diet suggestion;
- `analytics`: score and completion history;
- `settings`: reminder and API settings.

Feature screens should call repositories or services through ViewModels, not directly from Composables.

## 6. Data Model Direction

The MVP should use local-first data. Room entities should map closely to the MVP spec models:

```text
WorkoutPlanEntity
WorkoutDayEntity
ExerciseEntity
WorkoutExerciseLogEntity
DailyReviewEntity
DietLogEntity
AppSettings
```

Recommended rule:

- seed the initial exercise library locally;
- store user workout logs locally;
- store AI review results locally after generation;
- do not require login or cloud sync in MVP v1.

## 7. Repository Pattern

Use repository classes to isolate data operations:

```text
WorkoutRepository
ExerciseRepository
DailyReviewRepository
DietRepository
SettingsRepository
AIReviewRepository
```

ViewModels should depend on repositories, not directly on DAO objects.

## 8. AI Integration Architecture

AI must be isolated behind a service interface.

```kotlin
interface AIService {
    suspend fun generateDailyReview(input: DailyReviewInput): Result<DailyReviewOutput>
    suspend fun generateDietSuggestion(input: DietSuggestionInput): Result<DietSuggestionOutput>
}
```

MVP should start with a mock implementation:

```text
MockAIService
```

Then add a real implementation later:

```text
RemoteAIService
```

This prevents UI development from being blocked by API keys, model selection, provider limits, or network errors.

## 9. AI Safety Rules

The app must not present AI output as medical diagnosis.

When the user mentions pain, dizziness, chest tightness, injury, numbness, or abnormal symptoms, AI review should prioritize:

1. reduce training intensity;
2. stop risky movement;
3. rest and observe;
4. seek professional advice if symptoms persist.

The UI should show AI output as guidance, not professional medical instruction.

## 10. Notification Architecture

MVP reminder behavior:

- user enables or disables workout reminders;
- user chooses reminder time;
- app schedules local notifications for planned workout days;
- notifications should not require a backend.

Recommended implementation:

```text
SettingsRepository -> ReminderScheduler -> Android NotificationManager / WorkManager
```

Codex should first implement a simple local reminder abstraction. Complex recurring rules are not required in MVP v1.

## 11. Navigation Architecture

Use a bottom navigation layout for core areas:

```text
Home
Calendar
Workout
Diet
Settings
```

Secondary screens:

```text
ExerciseDetail
DailyCheckIn
AIReview
Analytics
```

The navigation graph should be explicit and easy to test.

## 12. UI Direction

The MVP UI should be clean, practical, and fast.

Avoid heavy animation or overly complex visual systems in v1.

Design style:

- dark or light theme is acceptable;
- use Material 3 components;
- use clear cards for workout, exercise, score, and AI review;
- prioritize tap reliability over fancy motion;
- keep forms simple.

## 13. Testing Direction

Minimum test expectations:

- unit test daily score calculation;
- unit test workout completion rate calculation;
- unit test AI output parsing if real API is added;
- manual test all acceptance flows in `spec/ACCEPTANCE.md`.

Automated UI tests are nice but not required for the first MVP implementation.

## 14. Codex Guardrails

Codex must follow these rules:

1. Do not replace the chosen stack without explicit instruction.
2. Do not build login, subscription, social features, wearable sync, or cloud backend in MVP v1.
3. Do not hard-code AI provider logic inside UI components.
4. Do not block local workout completion when AI API fails.
5. Do not store API keys in source code.
6. Keep implementation local-first.
7. Prefer small commits aligned with `spec/TASK_BREAKDOWN.md`.
8. After each task, run available build/test commands and report what passed or failed.

## 15. Implementation Philosophy

This branch is a specification branch. It should guide implementation, not contain experimental half-built product code.

Recommended workflow:

```text
spec/fitness-mvp-v1 -> feat/fitness-mvp-v1
```

The feature branch should implement tasks one by one. The spec branch should remain the contract.
