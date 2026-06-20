# Codex Workflow

Use a closed-loop workflow:

Spec -> Task -> Build -> Report -> Review -> Repair -> Next Task

Codex should work on `feat/fitness-mvp-v1`, one task at a time.

After each task, run `./gradlew assembleDebug` and report changed files, build result, APK path, limitations, and next recommended task.
