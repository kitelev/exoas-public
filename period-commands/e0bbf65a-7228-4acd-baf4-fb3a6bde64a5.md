---
exo__Asset_uid: e0bbf65a-7228-4acd-baf4-fb3a6bde64a5
exo__Asset_label: "Set Start Timestamp"
exo__Asset_isDefinedBy: "[[bf44251f-f18c-42a7-b612-e1036b2b6f80]]"
exo__Asset_createdAt: "2026-07-28T00:03:12+0500"
exo__Asset_updatedAt: "2026-07-28T00:03:12+0500"
exo__Instance_class:
  - "[[790e5b16-251d-4556-96ac-e5c7f1429b2e]]"
exocmd__Command_icon: "clock-arrow-up"
exocmd__Command_grounding: "[[e1adb5c4-5e60-4fa6-8864-73f3c97e5388]]"
exocmd__Command_confirmMessage: "Set actual start timestamp for this effort?"
exocmd__Command_successMessage: "Start timestamp set"
exocmd__Command_category: "planning"
exocmd__Command_cliName: set-start-timestamp
exocmd__Command_destructive: true
---

# Set Start Timestamp

Dynamic command that sets `ems__Effort_startTimestamp` — the ACTUAL start — to a user-provided datetime. For digitizing an effort **post-factum** (backdate the real start when the task is logged later than it actually began). CLI/palette command, no inline button — mirrors `Set Planned Start` (6bc86da6), targeting the fact timestamp.

- **Grounding**: `Set start timestamp` (e1adb5c4) → `updateProperty` on `ems__Effort_startTimestamp`
- **Input**: `{"value":"<ISO datetime>"}`
- Issue #3970 (composable from the existing `updateProperty` service — vault-data, no engine code).
