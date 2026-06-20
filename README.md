# Fitness AI App

Personal Android fitness guidance app MVP.

This repository is currently in the specification stage. The MVP contract lives in the `spec/` directory, and page design lives in root `design.md`.

## Project Direction

Fitness AI App is a local-first personal Android app for workout planning, exercise guidance, daily training review, lightweight diet logging, and optional AI-generated advice.

MVP priorities:

- run locally as a personal APK;
- no public account system;
- no social feed;
- no cloud sync requirement;
- no paid subscription;
- no custom backend required for MVP v1;
- AI is optional and configurable inside the app.

## Spec Documents

- `design.md` — page design, components, interaction states, Material 3 visual direction, and UI acceptance
- `spec/MVP_SPEC.md` — product scope and MVP definition
- `spec/ARCHITECTURE.md` — Android technical architecture and Codex guardrails
- `spec/TASK_BREAKDOWN.md` — step-by-step Codex implementation plan
- `spec/ACCEPTANCE.md` — MVP verification and acceptance checklist
- `spec/APK_PLAN.md` — stable Debug APK delivery plan and Codex App prompts
- `spec/CODEX_WORKFLOW.md` — Codex execution workflow
- `spec/REVIEW_PROCESS.md` — review and approval process

## Recommended Workflow

```text
main
└── spec/fitness-mvp-v1      # specification, design, and engineering contract
└── feat/fitness-mvp-v1      # implementation branch
```

Codex should read all spec documents and `design.md` before writing implementation code.

## AI Integration Direction

MVP AI behavior:

```text
Default: Local Mock AI advice
Optional: user-configured DeepSeek / OpenAI-compatible provider inside app settings
Backend: none required for MVP v1
```

Rules:

- App must work without remote AI configuration.
- Local mock AI advice must work before remote AI is implemented.
- Remote AI configuration should be done in the app, under Settings / AI Settings.
- Base URL and model should be configurable.
- User-entered credentials must not be hard-coded, committed, or printed in logs.
- Remote failure must not block workout completion or delete local data.

Recommended DeepSeek defaults for the settings UI:

```text
Base URL: https://api.deepseek.com
Model: deepseek-v4-flash
```

## Recommended First Implementation Task

```text
Read README.md, design.md, spec/MVP_SPEC.md, spec/ARCHITECTURE.md, spec/TASK_BREAKDOWN.md, spec/ACCEPTANCE.md, spec/APK_PLAN.md, spec/CODEX_WORKFLOW.md, and spec/REVIEW_PROCESS.md.

Create a new branch named feat/fitness-mvp-v1 from the current spec branch or from main after the spec branch is merged.

Implement Task 01 from spec/TASK_BREAKDOWN.md only: initialize a native Android project using Kotlin, Jetpack Compose, and Material 3. Create a buildable app with a placeholder Home screen using the visual direction from design.md. Do not implement workout logic, AI integration, database, reminders, or credential storage yet.

After implementation, run ./gradlew assembleDebug if available and report changed files, build result, APK path, tests run, design compliance, and limitations.
```
