# APK Plan

Build the MVP in small debug APK milestones.

## Milestones

1. Task 01: empty Android shell APK.
2. Task 02: navigation skeleton APK.
3. Tasks 03-12: local workout loop APK.
4. Tasks 13-20: full MVP debug APK.

Use `./gradlew assembleDebug` and report the APK output path after each milestone.

Expected debug APK path:

`app/build/outputs/apk/debug/app-debug.apk`

## Task 01 Codex Prompt

Use this in Codex App first:

```text
Open qinze889/fitness-ai-app.
Read spec/MVP_SPEC.md, spec/ARCHITECTURE.md, spec/TASK_BREAKDOWN.md, spec/ACCEPTANCE.md, and spec/APK_PLAN.md.
Create or switch to feat/fitness-mvp-v1.
Implement Task 01 only.
Use Kotlin + Jetpack Compose.
Create a buildable Android project with MainActivity and one placeholder Home screen.
Do not implement workout, database, AI, calendar, reminders, diet, analytics, login, payment, or cloud sync.
Do not modify spec files.
Run ./gradlew assembleDebug.
Fix code or config errors if possible.
Report changed files, build result, APK path, and limitations.
Commit and push the branch.
```

## Task 02 Codex Prompt

Use this after Task 01 builds:

```text
Continue on feat/fitness-mvp-v1.
Read all spec documents.
Implement Task 02 only.
Add bottom navigation: Home, Calendar, Workout, Diet, Settings.
Add placeholder secondary screens: Exercise Detail, Daily Check-in, AI Review, Analytics.
Keep screens as placeholders.
Do not implement database, AI, workout logic, reminders, or diet persistence yet.
Run ./gradlew assembleDebug.
Report changed files, build result, APK path, and limitations.
Commit and push the branch.
```

## Repair Prompt

Use this when review or build finds problems:

```text
Continue on the current PR branch.
Only fix the listed issues.
Do not add new features.
Do not modify spec files unless explicitly requested.
Run ./gradlew assembleDebug after fixing.
Commit and push the fix.
Report changed files, commands run, build result, APK path, and remaining risks.
```
