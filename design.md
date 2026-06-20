# Fitness AI App DESIGN.md

Branch: `spec/fitness-mvp-v1`  
Repo: `qinze889/fitness-ai-app`  
Status: UI design contract for MVP implementation

## 1. Purpose

This document defines how the Fitness AI App should look, feel, and behave at the UI level.

Codex must read this file before implementing any Compose screen. `design.md` is the visual and interaction contract. `spec/` files define product, architecture, task order, acceptance, APK plan, and workflow.

The design follows the DESIGN.md style used in `qinze889/awesome-design-md`: describe visual theme, color roles, typography, components, layout rules, states, do/don't rules, and agent implementation guidance in plain Markdown so AI coding agents can execute consistently.

## 2. Product Design Direction

The app is a personal Android fitness guidance tool, not a social fitness platform.

Design keywords:

```text
clean
focused
professional
local-first
card-based
training-loop-first
Material 3
personal AI coach
```

The user should immediately understand:

1. What should I train today?
2. Which exercise should I do next?
3. What did I complete or skip?
4. What should I write in my daily summary?
5. What did AI suggest for tomorrow?
6. How consistent was I this week?

## 3. Design Inspirations

Use these reference directions conceptually, but adapt them to Android Material 3 and Jetpack Compose:

| Reference style | Borrow | Avoid |
|---|---|---|
| Nike | athletic confidence, high-contrast training cards, strong action buttons | fashion-store layout |
| Apple | premium whitespace, clean typography, calm product feeling | hiding core actions |
| Linear | precise dashboards, status chips, compact progress cards | desktop-style density |
| Claude | warm AI assistant tone, readable suggestion cards | chat-heavy interface everywhere |
| Spotify | dark cards, list rhythm, progress feel | entertainment-first noise |

Final identity:

```text
Material 3 fitness dashboard + clean personal coach + structured training data
```

## 4. Core User Loop

```text
Home -> Today Workout -> Exercise Detail -> Complete / Skip / RPE -> Daily Summary -> AI Advice -> Calendar / Analytics -> Next Workout
```

No screen should be a dead end. Every screen must provide a clear next action or a clear back action.

## 5. Theme Strategy

MVP should support one stable theme first. Dark theme is preferred for the first visual implementation because it matches the approved first UI concept and feels like a focused training dashboard.

### 5.1 Dark Theme Tokens

| Role | Suggested value | Usage |
|---|---:|---|
| `background` | `#0B0F12` | App background |
| `surface` | `#12171C` | Cards and panels |
| `surfaceVariant` | `#1B2229` | Nested cards, inputs |
| `primary` | `#4CAF50` | Main training action, completion, progress |
| `primaryContainer` | `#1F3A25` | Low-emphasis green surface |
| `secondary` | `#40C4FF` | AI hints, analysis highlights |
| `tertiary` | `#FFB74D` | Warnings, fatigue, reminder highlights |
| `error` | `#EF5350` | Error state |
| `onBackground` | `#F5F7FA` | Main text |
| `onSurface` | `#E6EAEE` | Card text |
| `onSurfaceVariant` | `#AAB4BE` | Secondary text |
| `outline` | `#2A333C` | Dividers and card borders |

### 5.2 Light Theme Tokens

| Role | Suggested value | Usage |
|---|---:|---|
| `background` | `#F7F9F8` | App background |
| `surface` | `#FFFFFF` | Cards and panels |
| `surfaceVariant` | `#EEF2F0` | Inputs and secondary surfaces |
| `primary` | `#2E7D32` | Main action |
| `secondary` | `#0277BD` | AI hints |
| `tertiary` | `#EF6C00` | Warning |
| `error` | `#C62828` | Error state |
| `onBackground` | `#111418` | Main text |
| `onSurface` | `#1B1F23` | Card text |
| `onSurfaceVariant` | `#5F6B75` | Secondary text |
| `outline` | `#D8DEE4` | Dividers |

## 6. Typography Rules

Use Material 3 typography as the implementation base.

| Purpose | Style | Notes |
|---|---|---|
| Screen title | `headlineMedium` or `headlineSmall` | Clear and strong |
| Section title | `titleMedium` | Grouped content |
| Card title | `titleSmall` / `titleMedium` | Exercise names, AI card titles |
| Body | `bodyMedium` | Steps, explanations, tips |
| Secondary text | `bodySmall` | Labels and helper text |
| Big numbers | `displaySmall` or custom large `Text` | Daily score, completion %, streak |
| Button text | `labelLarge` | Short verbs only |

Rules:

- Do not use more than 3 text sizes in one card.
- Numeric metrics should be visually stronger than labels.
- Long exercise instructions must be broken into bullets or short paragraphs.
- Chinese text should remain readable.

## 7. Layout System

Spacing scale:

```text
4dp   micro spacing
8dp   chip/input inner spacing
12dp  compact card spacing
16dp  default screen padding
20dp  section spacing
24dp  large block spacing
32dp  hero spacing
```

Shape scale:

```text
8dp   chips and small controls
12dp  input fields
16dp  normal cards
24dp  hero cards and bottom sheets
```

Touch targets:

- Primary buttons should be at least 48dp high.
- Exercise card tap targets should be clear.
- RPE selector should be easy to tap with one hand.
- Destructive actions must be visually separated from primary actions.

## 8. Navigation Structure

Bottom navigation tabs:

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

Navigation rules:

- Home is the dashboard and starting point.
- Workout is the execution screen for today.
- Calendar shows history and planned days.
- Diet is lightweight logging.
- Settings contains plan, reminder, AI provider, and local data controls.
- AISettings can be a separate secondary screen from Settings.
- Back navigation must work for every secondary screen.

## 9. Screen Contracts

### 9.1 Home Dashboard

Purpose: show today's status and the next best action.

Required content:

- Greeting or product title.
- Today's training summary card.
- Current plan type: 3-day or 4-day.
- Next reminder time.
- Latest daily score.
- Latest AI advice preview.
- Primary button: `Start Today's Workout`.

Empty state:

- If no plan exists, show `No plan yet`.
- Primary button: `Create Training Plan`.

### 9.2 Today Workout Screen

Purpose: let the user execute today's workout.

Required content:

- Workout theme.
- Estimated duration.
- Exercise list.
- Per-exercise status.
- Progress summary.
- Primary button: `Finish Workout`.

Exercise card fields:

- Exercise name.
- Target muscle.
- Sets and reps.
- Optional weight.
- Status chip.
- Actions: `Detail`, `Complete`, `Skip`.
- RPE selector or entry if supported.

Status labels:

```text
Not started
In progress
Completed
Skipped
```

### 9.3 Exercise Detail Screen

Purpose: teach safe and useful exercise execution.

Required content:

- Exercise name.
- Target muscle group.
- Training effect.
- Recommended sets/reps/rest.
- Steps.
- Common mistakes.
- Safety notes.

### 9.4 Daily Check-in Screen

Purpose: collect post-workout feedback.

Required content:

- Summary text input.
- Quick prompt chips: `Completed`, `Skipped`, `Fatigue`, `Pain or discomfort`, `Diet notes`.
- Optional voice input entry point if implemented later.
- Primary button: `Save Summary`.
- Secondary button after saving: `Generate AI Advice`.

### 9.5 AI Advice Screen

Purpose: show actionable AI or local generated guidance.

Required cards:

- `Today's Review`.
- `Risk Notes`.
- `Next Workout Adjustment`.
- `Diet Tip`.
- `Motivation`.

States:

| State | UI behavior |
|---|---|
| Local mode | Show generated local guidance |
| Remote AI not configured | Show setup CTA: `Configure AI` |
| Loading | Show progress indicator and keep previous data visible if available |
| Success | Save result locally and show cards |
| Error | Show readable error and retry button; do not delete summary |
| Safety warning | Prioritize caution card at top |

AI output must be guidance, not medical diagnosis.

### 9.6 Calendar Screen

Purpose: show schedule and history.

Required content:

- Week or month view.
- Day status markers.
- Selected date summary.
- Workout completion state.

Day states:

```text
Planned
Completed
Partial
Missed
Rest
```

### 9.7 Diet Screen

Purpose: lightweight daily diet logging and advice.

Required content:

- Today's diet suggestion card.
- Breakfast input.
- Lunch input.
- Dinner input.
- Snack input.
- Save button.
- Optional AI diet tip if AI module exists.

MVP rule: do not build a full calorie database.

### 9.8 Settings Screen

Purpose: configure the local-first MVP.

Required content:

- Weekly plan selector: 3-day or 4-day.
- Reminder toggle.
- Reminder time.
- AI entry point: `AI Settings`.
- Local data reset option if implemented.

### 9.9 AI Settings Screen

Purpose: let the user optionally connect a real AI provider without requiring a server.

Required fields:

- Toggle: `Enable AI suggestions`.
- Provider selector: `Local Mock`, `DeepSeek`, `OpenAI Compatible`.
- Base URL field.
- Model field.
- Credential field with password masking.
- Button: `Test Connection`.
- Button: `Clear Credential`.
- Privacy note: training summary and diet notes are sent to the selected model provider when remote AI is enabled.

Recommended defaults:

```text
Provider: Local Mock
DeepSeek Base URL: https://api.deepseek.com
DeepSeek Model: deepseek-v4-flash
```

Credential rules:

- Never hard-code a real credential.
- Never commit a real credential.
- Never print a full credential in logs.
- Mask credentials in UI.
- Store credentials locally using Android Keystore + DataStore if feasible.
- If a first debug implementation uses DataStore without encryption, mark it as temporary and upgrade before public release.

## 10. Component Contracts

Reusable components should include:

```text
WorkoutSummaryCard
ExerciseCard
ExerciseStatusChip
ScoreCard
AIAdviceCard
DietLogCard
CalendarDayCell
PrimaryActionButton
SecondaryActionButton
EmptyState
ErrorState
LoadingState
RpeSelector
SettingsRow
CredentialInputField
```

## 11. State Rules

Every screen should define these states if applicable:

```text
Loading
Empty
Content
Error
Success / Completed
Disabled / Not configured
```

Examples:

- AI Advice: not configured, loading, success, error.
- Workout: no plan, planned workout, partially completed, completed.
- Diet: empty log, saved log, save error.
- Calendar: no plan, plan exists, selected date summary.

## 12. Do and Don't

### Do

- Prioritize the training loop.
- Make today's primary action obvious.
- Use local-first defaults.
- Keep AI optional.
- Make completion and skipped states visually distinct.
- Keep text short and actionable.
- Use Material 3 components unless there is a clear reason not to.

### Don't

- Do not add social feed.
- Do not add payments.
- Do not add marketplace/coach platform.
- Do not require login.
- Do not require cloud sync.
- Do not build a full calorie database in MVP v1.
- Do not build computer vision posture detection.
- Do not make AI suggestions look like medical diagnosis.
- Do not hard-code or leak credentials.

## 13. Compose Implementation Guidance

Rules:

- Use Jetpack Compose and Material 3.
- Use `MaterialTheme.colorScheme` and `MaterialTheme.typography`.
- Keep reusable components in shared UI packages when practical.
- Keep Composables stateless where possible; ViewModels own state.
- Avoid large Composables that mix navigation, data loading, and rendering.
- Use semantic state classes for screen state.
- Centralize theme tokens.

Recommended packages:

```text
core/ui/theme
core/ui/component
feature/home
feature/workout
feature/exercise
feature/checkin
feature/ai
feature/calendar
feature/diet
feature/settings
navigation
```

## 14. Design Acceptance

A UI task is accepted only when:

1. Screen purpose is clear.
2. Primary action is visible.
3. Empty state is handled.
4. Loading and error states are handled where relevant.
5. Back navigation works on secondary screens.
6. Visual hierarchy is readable.
7. Completion and skipped states are visually distinct.
8. AI-not-configured state is clear and recoverable.
9. No screen is a dead end.
10. The implementation follows this document unless explicitly revised.
