---
exo__Asset_isDefinedBy: "[[be2c3f06-48db-4a38-a21d-a81ce1836815]]"
exo__Asset_uid: bb00efed-7b17-42f5-a2c4-7cadf3e0ab36
exo__Asset_createdAt: 2026-04-27T21:31:58
exo__Asset_createdBy: "[[de20a3f1-7483-4714-ab28-b45f5cf02c76]]"
exo__Instance_class:
  - "[[790e5b16-251d-4556-96ac-e5c7f1429b2e]]"
exo__Asset_label: Create Task Instance
aliases:
  - Create Task Instance
exocmd__Command_icon: plus-square
exocmd__Command_category: creation
exocmd__Command_grounding: "[[a6ef8fda-addb-40c3-940c-fe55fd7e8500]]"
exocmd__Command_confirmMessage: Create a new task from this prototype?
exocmd__Command_successMessage: Task instance created
exocmd__Command_cliName: create-task-instance
exocmd__Command_destructive: true
exo__Asset_updatedAt: 2026-05-26T10:57:43+0500
---

# Create Task Instance

Dedicated Command for `ems__TaskPrototype` (Command-per-prototype canonical pattern). Wraps grounding `a6ef8fda` (create_instance, targetClass=ems__Task).

## History (deprecated multi-Grounding pattern)

Originally this Command was designed as a "universal Create Instance" with a 5-element `Command_grounding` array — one Grounding per prototype-class (Task / Project / Meeting / FleetingNote / Event). **That pattern never worked** in production: `CommandResolver.loadLinkedGrounding` (`packages/exocortex/src/services/CommandResolver.ts:899-924`) picks `refTriples[0]` without filtering by `Grounding_targetPrototype` — so click on any prototype always created an ems__Task. See issue [#3272](https://github.com/kitelev/exocortex/issues/3272).

**Resolution (2026-05-26):** repurposed under Task only. Each prototype now has its own dedicated Command + Binding (canonical Command-per-prototype pattern). Migration:

| Prototype | Dedicated Command | Binding |
|-----------|-------------------|---------|
| ems__TaskPrototype | this asset (bb00efed) | `[[2d157dc1-6994-4425-9700-67a41b967d85]]` |
| ems__ProjectPrototype | `[[654e2cfc-8e5b-4ad6-9e30-3ddcb673487f]]` | `[[ee9f4649-ec48-4eb6-891d-9005dd578939]]` |
| ems__MeetingPrototype | `[[bf20e11c-1c86-406a-9130-a8508848a8ea]]` | `[[22093ca1-1b6f-477f-a138-d13043b04a97]]` |
| ztlk__FleetingNotePrototype | `[[f95984f9-fc1a-4698-aa0e-096c7db038dc]]` | `[[4eea2f0b-a553-433b-a928-4149ac632735]]` |
| exo__EventPrototype | `[[6471517f-9581-49f7-9467-ddb6aacc69cb]]` | `[[54ccd669-2bb3-4b51-95fb-72f2d3857944]]` |
| kitelev__SelfObservation | `[[857ea585-2e55-4a1b-b9bd-547079729cd6]]` | `[[44130521-7bc1-4b2b-8dfc-922da72f5fde]]` |

Catch-all binding `90f56f1b` (`targetClass=exo#Prototype`) removed in the same migration.

## Related

- Issue: [#3272](https://github.com/kitelev/exocortex/issues/3272) — document Command-per-prototype canonical pattern
