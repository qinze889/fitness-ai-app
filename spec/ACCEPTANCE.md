# Fitness AI App MVP Acceptance Criteria

Branch: `spec/fitness-mvp-v1`  
Repo: `qinze889/fitness-ai-app`  
Status: MVP verification checklist

## 1. Purpose

This document defines how to verify whether the Fitness AI App MVP is actually usable.

The MVP is not accepted just because code exists. It is accepted only when the core user loop works end to end.

Core loop:

```text
Open app -> select plan -> view today's workout -> open exercise detail -> mark completion -> submit daily summary -> generate AI review -> view history/calendar
```

## 2. General Acceptance Rules

The MVP must satisfy these rules:

1. App builds successfully.
2. App launches without crash.
3. User can navigate between main screens.
4. Workout plan data persists after app restart.
5. Workout completion data persists after app restart.
6. Daily summary data persists after app restart.
7. AI review flow works with mock AI even if no real API key exists.
8. AI/API failure must not block workout completion.
9. No login or cloud backend is required for MVP v1.
10. UI must not contain dead-end buttons for core flows.

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

## 4. Navigation Acceptance

### Required Screens

Main tabs:

- Home;
- Calendar;
- Workout;
- Diet;
- Settings.

Secondary screens:

- Exercise Detail;
- Daily Check-in;
- AI Review;
- Analytics, if implemented.

### Test Steps

1. Launch app.
2. Tap each bottom tab.
3. Open Today Workout from Home.
4. Tap an exercise card.
5. Navigate back.
6. Open Daily Check-in.

### Accepted When

- Every main tab opens.
- Back navigation works.
- No core route crashes.
- User always has a way to return to a main screen.

## 5. Workout Plan Acceptance

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

## 6. Exercise Library Acceptance

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

## 7. Today Workout Acceptance

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

## 8. Daily Score Acceptance

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

## 9. Daily Check-in Acceptance

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

## 10. AI Review Acceptance

### Required for MVP

Mock AI review must work before real AI integration.

The AI review should return:

- daily review;
- risk notes;
- next workout adjustment;
- diet tip;
- motivation.

### Test Steps

1. Submit daily summary.
2. Tap generate AI review.
3. Confirm review appears.
4. Simulate or handle AI failure path if real API exists.

### Accepted When

- Mock review works without API key.
- Result is displayed in a clear card.
- AI failure does not delete user summary.
- AI failure does not block workout completion.

## 11. AI Safety Acceptance

### Required

If user summary contains warning terms such as:

- pain;
- injury;
- dizziness;
- chest tightness;
- numbness;
- severe discomfort;

then AI review or mock review should prioritize caution.

### Accepted When

- App does not provide medical diagnosis.
- App suggests lowering intensity, resting, or seeking professional advice when symptoms are serious.
- UI communicates that AI output is guidance, not medical instruction.

## 12. Calendar Acceptance

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

## 13. Reminder Acceptance

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

## 14. Diet Acceptance

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
- AI diet tip is optional but should use mock flow if AI module exists.

## 15. Analytics Acceptance

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

## 16. Settings Acceptance

### Required

Settings should support:

- weekly plan type;
- reminder enable/disable;
- reminder time;
- AI provider placeholder or configuration;
- local-only API key warning if API input exists.

### Accepted When

- Settings persist locally.
- No secret is committed to source code.
- User can understand what settings affect.

## 17. Persistence Acceptance

### Required

The following data must persist after app restart:

- selected plan type;
- generated workout days;
- exercise completion logs;
- daily check-in summaries;
- diet logs;
- reminder settings;
- AI review result if generated.

### Accepted When

- Restarting the app does not reset the core user loop.

## 18. Out-of-Scope Rejection Criteria

The MVP should be rejected or revised if Codex adds major unnecessary scope, such as:

- user account system;
- payment or subscription;
- social community;
- cloud sync;
- wearable integration;
- computer vision posture detection;
- full calorie database;
- coach marketplace;
- medical diagnosis logic.

## 19. Final Manual MVP Test

A reviewer should be able to perform this full flow:

1. Install and open the app.
2. Select 4-day training plan.
3. Open Home.
4. Open Today Workout.
5. Tap Bench Press detail.
6. Return to workout.
7. Mark Bench Press completed.
8. Mark Dumbbell Fly skipped.
9. Submit daily summary: “今天卧推完成了，飞鸟没做完，肩膀有点酸。”
10. Generate mock AI review.
11. See caution note about shoulder discomfort.
12. Open Calendar and see today marked as partial/completed.
13. Open Diet and save a simple diet log.
14. Restart app.
15. Confirm records still exist.

## 20. MVP Accepted When

The MVP is accepted when:

- build passes or environment issues are clearly documented;
- the final manual MVP test can be completed;
- core data persists locally;
- mock AI review works;
- no major out-of-scope features were added;
- known limitations are documented.
