# Fitness AI App Design

Branch: `spec/fitness-mvp-v1`
Repo: `qinze889/fitness-ai-app`
Status: page design and interaction contract

## 1. Design Purpose

This document defines the page design, component behavior, user flow, and visual direction for the Fitness AI App MVP.

Codex should use this document when implementing UI screens. The goal is to avoid random UI decisions and keep the app clear, consistent, and usable.

## 2. Design Principles

1. Training first: the app should help the user quickly know what to train today.
2. Clear actions: every primary button should tell the user what happens next.
3. Local-first: screens should work even without remote AI.
4. Low friction: common actions should take as few taps as possible.
5. Card layout: use clear cards for workouts, exercises, scores, AI suggestions, and diet logs.
6. Minimal animation: prioritize reliability over fancy effects.
7. Mobile-first: layout should be comfortable on Android phones.

## 3. Navigation Structure

Bottom navigation tabs:

```text
Home
Calendar
Workout
Diet
Settings
```

Secondary screens:

```text
Exercise Detail
Daily Check-in
AI Review
Analytics
```

Navigation rules:

- Bottom tabs should always be easy to return to.
- Secondary screens should have a clear back action.
- No screen should become a dead end.

## 4. Visual Style

Use Material 3 style with a clean fitness dashboard feel.

Recommended visual direction:

- rounded cards;
- clear typography hierarchy;
- strong primary action buttons;
- enough spacing between workout cards;
- simple icon usage if available;
- light theme first, dark theme optional later.

Avoid:

- overly complex animations;
- decorative UI that reduces readability;
- dense tables on mobile;
- too many controls on one screen.

## 5. Home Screen

### Purpose

Show the user today's training status and the next best action.

### Main content

- Greeting or app title.
- Today's workout summary card.
- Current weekly plan type.
- Latest daily score.
- Latest AI suggestion preview.
- Quick action button: Start Today's Workout.

### Empty state

If no workout plan exists, show:

- short explanation;
- button to create or select a plan.

### Primary action

```text
Start Today's Workout -> Workout screen
```

## 6. Workout Screen

### Purpose

Let the user execute today's workout.

### Main content

- Training theme, such as Chest and Shoulder.
- Exercise list.
- Exercise cards.
- Progress summary.
- Finish workout button.

### Exercise card fields

- exercise name;
- target muscle;
- sets and reps;
- status;
- optional weight;
- RPE input if available;
- buttons: Complete, Skip, Detail.

### Status labels

```text
Not started
In progress
Completed
Skipped
```

### Primary actions

```text
Detail -> Exercise Detail
Complete -> mark exercise completed
Skip -> mark exercise skipped
Finish Workout -> Daily Check-in
```

## 7. Exercise Detail Screen

### Purpose

Explain how to perform an exercise safely and why it matters.

### Main content

- exercise name;
- target muscle group;
- training effect;
- steps;
- common mistakes;
- safety notes;
- default sets and reps.

### Primary action

```text
Back to Workout
```

## 8. Daily Check-in Screen

### Purpose

Collect the user's workout summary and body feedback.

### Main content

- text input for daily summary;
- quick prompts:
  - what did I complete?
  - what did I skip?
  - how did my body feel?
  - any pain or discomfort?
  - diet notes.
- save button;
- generate AI review button after saving.

### Primary actions

```text
Save Summary -> persist daily summary
Generate AI Review -> AI Review screen
```

## 9. AI Review Screen

### Purpose

Show a short daily review and tomorrow adjustment suggestion.

### Main cards

- Daily Review.
- Risk Notes.
- Next Workout Adjustment.
- Diet Tip.
- Motivation.

### Safety rule

If the summary mentions pain, injury, dizziness, chest tightness, numbness, or severe discomfort, the review should suggest lowering intensity, resting, or seeking professional advice.

## 10. Calendar Screen

### Purpose

Show workout schedule and completion history.

### Main content

- week or month calendar;
- day status markers;
- selected date summary.

### Day states

```text
Planned
Completed
Partial
Missed
Rest
```

### Primary action

```text
Tap date -> show workout or summary for that date
```

## 11. Diet Screen

### Purpose

Let the user log simple daily diet notes.

### Main content

- today's diet suggestion card;
- breakfast input;
- lunch input;
- dinner input;
- snack input;
- save button.

### MVP rule

Do not build a full calorie database in MVP v1.

## 12. Analytics Screen

### Purpose

Show simple training progress.

### Main metrics

- weekly completion rate;
- completed workout days;
- average daily score;
- skipped exercise count;
- recent RPE or fatigue trend if available.

### Empty state

If no history exists, show a friendly empty state and guide the user to complete today's workout.

## 13. Settings Screen

### Purpose

Let the user configure the MVP.

### Main content

- weekly plan selector: 3-day or 4-day;
- reminder toggle;
- reminder time;
- AI provider placeholder;
- data reset option if implemented.

## 14. Component List

Reusable components should include:

```text
WorkoutSummaryCard
ExerciseCard
ScoreCard
AIReviewCard
DietLogCard
CalendarDayCell
PrimaryActionButton
EmptyState
StatusChip
```

## 15. Design Acceptance

A UI task is accepted only when:

1. screen purpose is clear;
2. primary action is visible;
3. empty state is handled;
4. back navigation works on secondary screens;
5. visual hierarchy is readable;
6. no page is a dead end;
7. design follows this document unless explicitly revised.
