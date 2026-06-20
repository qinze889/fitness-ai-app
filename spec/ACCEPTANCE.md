# Fitness AI App MVP Acceptance Criteria

Branch: `spec/fitness-mvp-v1`  
Repo: `qinze889/fitness-ai-app`  
Status: MVP verification checklist

## 1. Purpose

This document defines how to verify whether the Fitness AI App MVP is actually usable.

The MVP is not accepted just because code exists. It is accepted only when the core user loop works end to end.

Core loop:

```text
Open app -> select plan -> view today's workout -> open exercise detail -> mark completion -> submit daily summary -> generate local or remote AI advice -> view history/calendar
```

## 2. General Acceptance Rules

The MVP must satisfy these rules:

1. App builds successfully.
2. App launches without crash.
3. User can navigate between main screens.
4. Workout plan data persists after app restart.
5. Workout completion data persists after app restart.
6. Daily summary data persists after app restart.
7. AI advice flow works with local mock mode even if no remote provider is configured.
8. Remote AI/API failure must not block workout completion.
9. No login or cloud backend is required for MVP v1.
10. UI must not contain dead-end buttons for core flows.
11. UI tasks must follow root `design.md`.
12. No real remote AI credential may be hard-coded, committed, or logged.

## 3. Build Acceptance

### Required

- Android project builds locally.
- Kotlin compile passes.
- Compose screens compile.
- App can be installed or launched in emulator/device.

### Verification

Run the available build command, for example:

```text
./gradlew assembleDebug
```

If the exact command differs, Codex must report the command used.

### Accepted When

- Build command exits successfully; or
- if build fails because of environment limitation, Codex must document the exact failure and confirm whether code-level compilation issues remain.

## 4. Design Acceptance

### Required

- `design.md` exists in the repository root.
- UI screens use Material 3.
- Core screen structure follows `design.md`.
- Primary actions are visible.
- Empty states are handled.
- Loading, success, error, completed, skipped, and not-configured states are handled where relevant.

### Accepted When

- Home, Workout, Exercise Detail, Daily Check-in, AI Advice, Calendar, Diet, and Settings are readable and navigable.
- Completed and skipped workout states are visually distinguishable.
- AI-not-configured state provides a clear path to Settings.
- No screen is a dead end.

## 5. Navigation Acceptance

### Required Screens

Main tabs:

- Home;
- Workout;
- Calendar;
- Diet;
- Settings.

Secondary screens:

- Exercise Detail;
- Daily Check-in;
- AI Advice;
- AI Settings;
- Analytics, if implemented.

### Test Steps

1. Launch app.
2. Tap each bottom tab.
3. Open Today Workout from Home.
4. Tap an exercise card.
5. Navigate back.
6. Open Daily Check-in.
7. Open AI Settings from Settings.

### Accepted When

- Every main tab opens.
- Back navigation works.
- No core route crashes.
- User always has a way to return to a main screen.

## 6. Workout Plan Acceptance

### Required

User can select or use a default weekly plan:

- 3-day plan; or
- 4-day plan.

MVP should support both if possible. If implementation starts with one default plan, the limitation must be documented and the plan selector must be added in a later task.

### Test Steps

1. Open Settings or Plan configuration.
2. Select 3-day or 4-day training plan.
3. Save setting.
4. Return to Home.
5. Confirm Home shows current plan type.
6. Open Workout screen.
7. Confirm Today Workout is generated.

### Accepted When

- Plan type persists after restart.
- Workout days are generated from the selected plan.
- App handles empty state if no plan is configured.

## 7. Exercise Library Acceptance

### Required Initial Exercises

The local exercise library must include at least:

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

### Required Fields

Each exercise must include:

- name;
- target muscle group;
- description;
- training effect;
- steps;
- common mistakes;
- safety notes;
- default sets;
- default reps;
- difficulty.

### Test Steps

1. Open Workout screen.
2. Tap an exercise card.
3. Confirm Exercise Detail screen shows complete information.

### Accepted When

- Exercise detail is useful enough for a beginner to understand the movement.
- Missing fields are not shown as broken placeholders.

## 8. Today Workout Acceptance

### Required

User can view and execute today’s workout.

Each exercise item should show:

- exercise name;
- sets;
- reps;
- status;
- optional weight;
- RPE selector or input if implemented.

### Test Steps

1. Open Today Workout.
2. Mark one exercise completed.
3. Mark one exercise skipped.
4. Enter or select RPE if available.
5. Leave screen.
6. Return to screen.

### Accepted When

- Exercise status persists.
- Completed and skipped states are visually distinguishable.
- Workout can proceed even if some exercises are skipped.

## 9. Daily Score Acceptance

### Required Formula

```text
Daily Score = Completion Score * 0.6 + Effort Score * 0.25 + Feedback Score * 0.15
```

Where:

- Completion Score = completed exercises / planned exercises * 100;
- Effort Score = average RPE normalized to 100;
- Feedback Score = 100 if daily summary exists, otherwise 0.

### Required Edge Cases

- No planned exercises.
- All exercises completed.
- Some exercises skipped.
- No RPE input.
- No daily summary.

### Accepted When

- Score is between 0 and 100.
- Formula is implemented in a testable function.
- At least basic unit tests or documented manual checks exist.

## 10. Daily Check-in Acceptance

### Required

User can submit post-workout feedback.

Required input:

- text summary.

Recommended prompts:

- what did I train today?
- what did I complete?
- what did I skip?
- how did I feel?
- any pain or discomfort?
- diet notes.

### Test Steps

1. Finish or partially finish a workout.
2. Open Daily Check-in.
3. Enter a summary.
4. Save summary.
5. Close app.
6. Reopen app.
7. Confirm summary remains available.

### Accepted When

- Summary is saved locally.
- Summary affects daily score if applicable.
- Empty summary is handled gracefully.

## 11. AI Advice Acceptance

### Required for MVP

Local mock AI advice must work before real remote integration.

The AI advice should return:

- daily review;
- risk notes;
- next workout adjustment;
- diet tip;
- motivation.

### Test Steps

1. Submit daily summary.
2. Tap generate AI advice.
3. Confirm advice appears.
4. Confirm app works without remote AI provider configuration.
5. Simulate or handle remote AI failure path if remote provider exists.

### Accepted When

- Local mock advice works without remote configuration.
- Result is displayed in clear cards.
- AI failure does not delete user summary.
- AI failure does not block workout completion.
- The app clearly distinguishes local mode, not-configured remote mode, loading state, success state, and error state where relevant.

## 12. AI Settings Acceptance

### Required

Settings should support optional remote AI configuration:

- enable/disable AI suggestions;
- provider selector: Local Mock, DeepSeek, OpenAI Compatible;
- base URL;
- model;
- local credential input with masked display;
- test connection action;
- clear credential action;
- privacy note explaining that workout summary and diet notes may be sent to the chosen provider when remote AI is enabled.

### Accepted When

- App works without remote AI settings.
- AI Settings values persist locally after restart.
- User can clear saved credential.
- No real credential is hard-coded in source.
- No real credential appears in README/spec files.
- Logs do not print full credential values.
- Remote failure produces readable error UI.

## 13. AI Safety Acceptance

### Required

If user summary contains warning terms such as:

- pain;
- injury;
- dizziness;
- chest tightness;
- numbness;
- severe discomfort;

then AI advice or local mock advice should prioritize caution.

### Accepted When

- App does not provide medical diagnosis.
- App suggests lowering intensity, resting, or seeking professional advice when symptoms are serious.
- UI communicates that AI output is guidance, not medical instruction.

## 14. Calendar Acceptance

### Required

Calendar should show workout schedule and completion status.

### Test Steps

1. Open Calendar.
2. Confirm workout days are marked.
3. Complete a workout.
4. Return to Calendar.
5. Confirm completed day status updates.
6. Tap a date with workout record.

### Accepted When

- Planned days are visible.
- Completed/missed/rest states are distinguishable.
- Date tap opens relevant workout or summary if available.

## 15. Reminder Acceptance

### Required

Workout reminder should be configurable locally.

### Test Steps

1. Open Settings.
2. Enable reminder.
3. Set reminder time.
4. Save settings.
5. Restart app.
6. Confirm setting persists.

### Accepted When

- Reminder setting persists.
- App requests notification permission where required.
- Local notification scheduling path exists.

Note: exact notification delivery may depend on device/emulator environment. If it cannot be fully verified, Codex must document the limitation.

## 16. Diet Acceptance

### Required

Diet module should support lightweight daily logging.

### Test Steps

1. Open Diet screen.
2. Enter breakfast, lunch, dinner, or snack text.
3. Save diet log.
4. Restart app.
5. Confirm diet log persists.

### Accepted When

- Diet log is saved locally.
- Diet screen does not require calorie database.
- AI diet tip is optional but should use local mock flow if AI module exists.

## 17. Analytics Acceptance

### Required

Analytics should show basic progress if enough local data exists.

Minimum metrics:

- weekly completion rate;
- completed workout days;
- average daily score;
- skipped exercise count.

### Accepted When

- Empty state is clear when no history exists.
- Metrics update from local workout logs.
- Display is readable and not misleading.

## 18. Settings Acceptance

### Required

Settings should support:

- weekly plan type;
- reminder enable/disable;
- reminder time;
- AI Settings entry point;
- local-only data reset warning if reset exists.

### Accepted When

- Settings persist locally.
- User can understand what settings affect.
- Settings screen links to AI Settings.

## 19. Persistence Acceptance

### Required

The following data must persist after app restart:

- selected plan type;
- generated workout days;
- exercise completion logs;
- daily check-in summaries;
- diet logs;
- reminder settings;
- AI advice result if generated;
- AI provider settings if configured.
