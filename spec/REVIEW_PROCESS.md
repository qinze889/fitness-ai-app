# Review Process

Use this review loop for every Codex implementation task:

```text
Task scope -> Diff -> Build -> APK -> Acceptance -> Repair notes -> Recheck
```

A task is not accepted just because files were generated. It is accepted only when the task is within scope, buildable, and verifiable.

## 1. Review Inputs

Before reviewing, collect:

1. current task number;
2. PR or branch name;
3. changed files;
4. build command and result;
5. APK path if relevant;
6. test result if available;
7. known limitations reported by Codex.

## 2. Scope Review

Check whether the implementation matches the current task in `spec/TASK_BREAKDOWN.md`.

Ask:

- Is this exactly the requested task?
- Did Codex skip ahead?
- Did Codex add unrelated features?
- Did Codex modify `spec/` files unexpectedly?

If scope is wrong, request changes.

## 3. Diff Review

Review changed files.

For Task 01, expected changes are mainly Android project setup files and a placeholder Home screen.

For Task 02, expected changes are mainly navigation files and placeholder screens.

Unexpected changes should be questioned.

## 4. Architecture Review

Compare the implementation with `spec/ARCHITECTURE.md`.

Check:

- Kotlin is used;
- Jetpack Compose is used;
- MVVM direction is preserved;
- AI logic is isolated when added;
- local-first direction is preserved;
- unnecessary backend systems are not introduced.

## 5. Build Review

Every implementation task should report a build command.

Default command:

```bash
./gradlew assembleDebug
```

Acceptable result:

- build passes; or
- failure is clearly caused by local environment and not by code, with details reported.

If build fails because of code or project configuration, request repair.

## 6. APK Review

When an APK milestone is expected, verify that Codex reports the APK path.

Expected debug APK path:

```text
app/build/outputs/apk/debug/app-debug.apk
```

For early milestones, also check:

- app launches;
- visible screen exists;
- navigation works if Task 02 is included.

## 7. Acceptance Review

Compare the result with `spec/ACCEPTANCE.md`.

For each task, review only the acceptance items relevant to that task.

Do not require full MVP acceptance during Task 01 or Task 02.

## 8. Repair Notes Format

If changes are needed, write repair notes like this:

```text
Review result: request changes
Task reviewed:
Problems:
1.
2.
3.
Required fixes:
1.
2.
3.
Do not change:
- spec files
- unrelated features
Rebuild command:
./gradlew assembleDebug
```

## 9. Approval Rule

Approve only when:

1. task scope is correct;
2. diff is reasonable;
3. build result is acceptable;
4. APK path is reported when relevant;
5. acceptance items for the task are satisfied;
6. no high-risk unrelated change is present.

## 10. Next Task Rule

After approval, the next instruction to Codex should be the next unfinished task from `spec/TASK_BREAKDOWN.md`.

Do not let Codex jump from Task 01 directly to full MVP unless explicitly approved.
