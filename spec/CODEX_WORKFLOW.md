# Codex Workflow

Use a closed-loop workflow:

```text
Spec -> Design -> Task -> Build -> Report -> Review -> Repair -> Next Task
```

Codex should work on `feat/fitness-mvp-v1`, one task at a time.

## 1. Read Before Coding

Codex should read these documents before starting implementation:

1. `spec/MVP_SPEC.md`
2. `spec/ARCHITECTURE.md`
3. `design.md`
4. `spec/TASK_BREAKDOWN.md`
5. `spec/ACCEPTANCE.md`
6. `spec/APK_PLAN.md`
7. `spec/CODEX_WORKFLOW.md`
8. `spec/REVIEW_PROCESS.md`

## 2. Branch Rule

Use this branch layout:

```text
spec/fitness-mvp-v1  = planning and contract
feat/fitness-mvp-v1  = implementation work
```

Implementation work should happen on `feat/fitness-mvp-v1`.

## 3. Task Rule

Work on one task at a time.

Examples:

```text
Task 01 only
Task 02 only
Task 03 only
```

Do not combine multiple unrelated tasks in one pass.

## 4. Design Rule

When a task touches UI, Codex must follow `design.md`.

Use `design.md` for:

- page structure;
- component layout;
- button behavior;
- empty states;
- navigation behavior;
- visual style;
- design acceptance.

If a UI requirement is unclear, Codex should choose the simplest design that follows `design.md` and report the assumption.

## 5. Build Rule

After each task, run:

```bash
./gradlew assembleDebug
```

If another valid Android build command is used, report the exact command.

## 6. APK Rule

When an APK is expected, report the output path.

Expected path:

```text
app/build/outputs/apk/debug/app-debug.apk
```

If the path differs, report the actual path.

## 7. Report Format

After each task, report:

```text
Task completed:
Changed files:
Build command:
Build result:
APK path:
Tests run:
Limitations:
Next recommended task:
```

## 8. Scope Rule

Stay inside the current task scope.

For MVP v1, avoid adding major systems that are not requested by the task, such as account systems, paid features, social features, cloud sync, wearable integration, computer vision posture detection, full calorie database, or marketplace features.

## 9. Spec Rule

Implementation tasks should not edit files under `spec/`.

If a spec change seems necessary, stop and report the reason instead of silently changing the contract.

## 10. Repair Rule

If review or build fails:

1. read the review notes;
2. fix only the listed issues;
3. rebuild;
4. commit the fix to the same branch;
5. report the result.

## 11. Completion Rule

A task is complete only when:

1. the implementation matches the task;
2. design follows `design.md` for UI tasks;
3. build result is reported;
4. APK path is reported when relevant;
5. unrelated features are not added;
6. limitations are documented;
7. the branch is ready for review.
