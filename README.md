# Fitness AI App

Personal Android fitness guidance app MVP.

This repository is currently in the specification stage. The MVP contract lives in the `spec/` directory, and page design lives in `design.md`.

## Spec Documents

- `design.md` — page design, components, interaction, and visual direction
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
└── spec/fitness-mvp-v1      # specification contract
└── feat/fitness-mvp-v1      # implementation branch
```

Codex should read all spec documents and `design.md` before writing implementation code.

Recommended first implementation task:

```text
Read design.md and spec/APK_PLAN.md, then implement Task 01 from spec/TASK_BREAKDOWN.md only: initialize a native Android project using Kotlin and Jetpack Compose with a placeholder Home screen, run ./gradlew assembleDebug, and report the Debug APK path.
```
