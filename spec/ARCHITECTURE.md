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
6. optional AI-generated review and optimization suggestions;
7. simple diet logging and analytics.

The architecture should make the first implementation easy for Codex to execute step by step.

## 2. Chosen MVP Stack

Use native Android for MVP v1.

```text
Language: Kotlin
UI: Jetpack Compose
Design system: Material 3 + root design.md
Architecture pattern: MVVM
State: ViewModel + StateFlow
Navigation: Jetpack Navigation Compose
Local database: Room
Settings storage: DataStore
Secure local credential storage: Android Keystore + encrypted local storage if feasible
Background work: WorkManager where needed
Local notifications: Android notification channel + scheduled local reminder
Networking: OkHttp or Retrofit
JSON: Kotlinx Serialization or Moshi
Minimum backend: none required for MVP v1
AI provider: local mock by default; optional DeepSeek/OpenAI-compatible remote provider configured in app settings
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
  design.md
  spec/
    MVP_SPEC.md
    ARCHITECTURE.md
    TASK_BREAKDOWN.md
    ACCEPTANCE.md
    APK_PLAN.md
    CODEX_WORKFLOW.md
    REVIEW_PROCESS.md
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
          security/
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
- AI integration contracts and network clients;
- app settings storage;
- local credential storage helper;
- shared UI components;
- theme tokens from `design.md`.

The core layer must not depend on feature screens.

### 5.2 Feature Layer

Each feature owns its UI, ViewModel, and screen-specific state:

- `home`: dashboard and quick status;
- `calendar`: workout calendar and date status;
- `workout`: today workout execution;
- `exercise`: exercise detail;
- `checkin`: daily text/voice summary;
- `ai`: AI advice rendering, retry behavior, local/remote provider state;
- `diet`: diet logging and diet suggestion;
- `analytics`: score and completion history;
- `settings`: plan, reminder, AI provider, and credential settings.

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
AIAdviceEntity
AppSettings
AIProviderSettings
```

Recommended rules:

- seed the initial exercise library locally;
- store user workout logs locally;
- store AI advice results locally after generation;
- do not require login or cloud sync in MVP v1;
- do not require a custom backend server in MVP v1.

## 7. Repository Pattern

Use repository classes to isolate data operations:

```text
WorkoutRepository
ExerciseRepository
DailyReviewRepository
DietRepository
SettingsRepository
AIAdviceRepository
```

ViewModels should depend on repositories, not directly on DAO objects.

## 8. AI Integration Architecture

AI is optional. The app must work completely without a remote provider.

The AI module should be a real configurable integration module, not only an empty abstraction. It should include:

```text
AIProviderSettings
AIAdviceRepository
AIAdvicePromptBuilder
LocalMockAIClient
DeepSeekAIClient
OpenAICompatibleAIClient
CredentialStore
```

### 8.1 Provider Modes

Supported modes:

```text
Local Mock
DeepSeek
OpenAI Compatible
```

Default mode:

```text
Local Mock
```

The user should be able to configure remote AI inside the app, preferably in `Settings -> AI Settings`.

### 8.2 Recommended DeepSeek Defaults

```text
Base URL: https://api.deepseek.com
Model: deepseek-v4-flash
Endpoint pattern: /chat/completions
```

Codex must keep these values configurable. They must not be scattered across UI code.

### 8.3 AI Interface

Use a small interface for the app-level use cases:

```kotlin
interface AIAdviceClient {
    suspend fun generateDailyAdvice(input: DailyAdviceInput): Result<DailyAdviceOutput>
    suspend fun generateDietAdvice(input: DietAdviceInput): Result<DietAdviceOutput>
    suspend fun testConnection(settings: AIProviderSettings): Result<Unit>
}
```

The UI should not call network code directly.

### 8.4 Credential Handling

Rules:

- A real user credential must never be hard-coded into source code.
- A real user credential must never be committed to Git.
- A real user credential must never be printed in logs.
- Credential input must be optional.
- If no credential is configured, the app must fall back to Local Mock mode or show a clear not-configured state.
- Prefer Android Keystore + DataStore for local storage.
- If secure storage is postponed, mark the debug-only limitation clearly and add an upgrade task.

### 8.5 Prompt Builder

`AIAdvicePromptBuilder` should convert local data into a compact, structured request:

```text
- date
- workout theme
- planned exercises
- completed exercises
- skipped exercises
- RPE
- daily score
- user summary
- diet notes
```

The prompt should ask for concise, actionable output with these fields:

```text
daily_review
risk_notes
next_workout_adjustment
diet_tip
motivation
```

## 9. AI Safety Rules

The app must not present AI output as medical diagnosis.

When the user mentions pain, dizziness, chest tightness, injury, numbness, or abnormal symptoms, AI advice should prioritize:

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
Workout
Calendar
Diet
Settings
```

Secondary screens:

```text
ExerciseDetail
DailyCheckIn
AIAdvice
Analytics
AISettings
```

The navigation graph should be explicit and easy to test.

## 12. UI Direction

The MVP UI must follow root `design.md`.

Design style:

- Material 3;
- dark theme first is acceptable and preferred by current concept;
- clear card-based screens;
- strong primary actions;
- visible empty, completion, error, and not-configured states;
- prioritize tap reliability over fancy motion.

Avoid heavy animation or overly complex visual systems in v1.

## 13. Testing Direction

Recommended test areas:

- daily score calculation;
- plan generation;
- exercise seed data integrity;
- AI prompt builder output shape;
- local mock AI behavior;
- AI not-configured state;
- settings persistence;
- navigation smoke test if practical.

Codex should run available tests and `./gradlew assembleDebug` after each implementation task.
