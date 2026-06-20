# Fitness AI App

Personal Android fitness guidance app MVP.

This repository is currently in the specification stage. The MVP contract lives in the `spec/` directory.

## Spec Documents

- `spec/MVP_SPEC.md` — product scope and MVP definition
- `spec/ARCHITECTURE.md` — Android technical architecture and Codex guardrails
- `spec/TASK_BREAKDOWN.md` — step-by-step Codex implementation plan
- `spec/ACCEPTANCE.md` — MVP verification and acceptance checklist

## Recommended Workflow

```text
main
└── spec/fitness-mvp-v1      # specification contract
└── feat/fitness-mvp-v1      # implementation branch
```

Codex should read all spec documents before writing implementation code.

Recommended first implementation task:

```text
Implement Task 01 from spec/TASK_BREAKDOWN.md only: initialize a native Android project using Kotlin and Jetpack Compose with a placeholder Home screen.
```
