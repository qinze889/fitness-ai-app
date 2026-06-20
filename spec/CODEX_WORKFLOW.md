# Codex Workflow

Use a closed-loop workflow:

```text
Spec -> Design -> Task -> Build -> Report -> Review -> Repair -> Next Task
```

Codex should work on `feat/fitness-mvp-v1`, one task at a time.

## 1. Read Before Coding

Codex should read these documents before starting implementation:

1. `README.md`
2. `design.md`
3. `spec/MVP_SPEC.md`
4. `spec/ARCHITECTURE.md`
5. `spec/TASK_BREAKDOWN.md`
6. `spec/ACCEPTANCE.md`
7. `spec/APK_PLAN.md`
8. `spec/CODEX_WORKFLOW.md`
9. `spec/REVIEW_PROCESS.md`

## 2. Branch Rule

Use this branch layout:

```text
spec/fitness-mvp-v1  = planning, design, and contract
feat/fitness-mvp-v1  = implementation work
```

Implementation work should happen on `feat/fitness-mvp-v1`.

Do not implement app code directly on the spec branch.

## 3. Task Rule

Work on one task at a time.

Examples:

```text
Task 01 only
Task 02 only
Task 03 only
```

Do not combine multiple unrelated tasks in one pass.

If a later task looks easy, still do not implement it early unless explicitly instructed.

## 4. Design Rule

When a task touches UI, Codex must follow root `design.md`.

Use `design.md` for:

- visual direction;
- page structure;
- component layout;
- button behavior;
- empty states;
- completion/skipped states;
- loading/error/not-configured states;
- navigation behavior;
- design acceptance.

If a UI requirement is unclear, Codex should choose the simplest Material 3 implementation that follows `design.md` and report the assumption.

## 5. AI Rule

AI must be optional and local-first.

Required behavior:

1. App must work without remote AI configuration.
2. Local mock AI advice must work before real remote AI integration.
3. Remote AI configuration must live inside the app Settings / AI Settings flow.
4. DeepSeek and OpenAI-compatible providers should be configurable by base URL and model.
5. Remote AI failure must not block workout completion.
6. Remote AI failure must not delete local summary, workout, diet, or score data.

## 6. Credential Rule

Codex must not introduce real user credentials into source control.

Rules:

- Do not hard-code a real remote AI credential.
- Do not add real credentials to README, spec files, code comments, tests, sample data, screenshots, or logs.
- Do not print full credential values.
- Use placeholders only.
- Store user-entered credentials locally at runtime.
- Prefer Android Keystore + DataStore for local credential storage.
- If secure storage is postponed, document the limitation and keep the app in local mock mode by default.

## 7. Build Rule

After each task, run:

```bash
./gradlew assembleDebug
```

If another valid Android build command is used, report the exact command.

## 8. APK Rule

When an APK is expected, report the output path.

Expected path:

```text
app/build/outputs/apk/debug/app-debug.apk
```

If the path differs, report the actual path.

## 9. Report Format

After each task, report:

```text
Task completed:
Changed files:
Build command:
Build result:
APK path:
Tests run:
Design compliance:
AI/credential changes:
Limitations:
Next recommended task:
```

## 10. Scope Rule

Stay inside the current task scope.

For MVP v1, avoid adding major systems that are not requested by the task, such as:

- account systems;
- paid features;
- social features;
- cloud sync;
- wearable integration;
- computer vision posture detection;
- full calorie database;
- coach marketplace;
- public backend service.

## 11. Spec Rule

Implementation tasks should not edit files under `spec/` or root `design.md`.

If a spec or design change seems necessary, stop and report the reason instead of silently changing the contract.

## 12. Repair Rule

If review or build fails:

1. read the review notes;
2. fix only the listed issues;
3. rebuild;
4. commit the fix to the same branch;
5. report the result.

Do not use a repair task to add unrelated features.

## 13. Completion Rule

A task is complete only when:

1. the implementation matches the task;
2. design follows `design.md` for UI tasks;
3. build result is reported;
4. APK path is reported when relevant;
5. unrelated features are not added;
6. AI remains optional unless the current task explicitly implements remote AI;
7. no real credential is committed or logged;
8. limitations are documented;
9. the branch is ready for review.
