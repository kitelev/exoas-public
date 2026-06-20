# exoas-public — Exocortex public TBox AssetSpace

**The shared, public ontology library for [Exocortex](https://github.com/kitelev/exocortex).** This repository is an **AssetSpace** — a git-backed package of vault assets that you mount into an Exocortex vault. It holds the *domain vocabularies* (ontologies) that the Exocortex engine itself does not ship: tasks and projects, concepts, people, time periods, strategy frameworks, and more.

> **Where this fits.** Exocortex deliberately separates **the engine** from **the data it operates on**. The engine (`exo` — the RDF parser, SPARQL engine, layout renderer, command machinery, shipped as the plugin + `@kitelev/exocortex-cli`) is domain-agnostic. Every *domain* it works with lives in a mountable AssetSpace like this one. See the engine's [Exo-as-SDK topology](https://github.com/kitelev/exocortex/blob/main/docs/explanation/assetspace-sdk-topology.md) for the full model (`exo` = platform, an AssetSpace = a library/jar, a Profile = a manifest).

---

## What's in here

`exoas-public` bundles many **namespaces** — one folder per ontology. Each namespace owns its own classes, properties, and (where relevant) command/grounding definitions. Every asset is a UID-named Markdown file (`<uuid>.md`) co-located in its namespace folder.

| Namespace | What it is | Docs |
| --- | --- | --- |
| **`ems`** | **Effort Management** — Areas, Projects, Tasks, Meetings + a status lifecycle. The Areas → Projects → Tasks model behind most of the Getting-Started walkthrough. | **[↓ documented below](#ems--effort-management-pilot)** |
| `concept` | Concepts / knowledge atoms (the successor to the legacy `ims` namespace). | stub |
| `pmi` | Project-management vocabulary (PMI/PMBOK-flavoured). | stub |
| `period` | Calendar periods — Day, Week, Month, Quarter, Year. | stub |
| `period-commands` | exocmd command definitions for period & daily notes. | stub |
| `pn` | Personal / period notes (e.g. `pn__DailyNote`). | stub |
| `person` | People. | stub |
| `place` | Places / locations. | stub |
| `ztlk` | Zettelkasten — permanent & fleeting notes. | stub |
| `strategy` | Strategy / objectives vocabulary. | stub |
| `vmost` | VMOST strategic-planning framework (Vision · Mission · Objectives · Strategy · Tactics). | stub |
| `para` | PARA method (Projects · Areas · Resources · Archives). | stub |
| `jedi` | "Jedi Techniques" productivity framework. | stub |
| `role` | Roles. | stub |
| `profession` | Professions. | stub |
| `ref` | References / bibliography. | stub |
| `metric` | Metrics. | stub |
| `agent` | Agents. | stub |
| `ui` | UI vocabulary (layouts, columns). | stub |
| `exoql` | ExoQL query vocabulary. | stub |
| `exotemplate` | Templating vocabulary (`exotemplate__Template`, `body_template` groundings). | stub |
| `w3c` | Anchors for standard W3C vocabularies. | stub |
| `ims` | **Legacy** — original IMS namespace (concepts/notes/people); being re-prefixed to `concept` / `person`. Deprecated, kept for back-references. | stub |
| `ems-commands` | EMS-specific exocmd command definitions. | stub |
| `adapter-ems-person`, `adapter-ems-place`, `adapter-exo-ims`, `exo-ims` | Cross-ontology adapter shims that bridge two namespaces. | stub |
| `tools`, `vault`, `ref`, `kf`, `kpc`, `ts`, `exoob`, `concepts-public` | Additional domain vocabularies. | stub |

> **"stub" = documentation pending.** Each namespace section will be expanded over time, following the **EMS section below as the template**. The classes themselves are live and usable today regardless of doc status — this README is reference prose, not a gate.

---

## EMS — Effort Management (pilot)

> This is the **fully-written reference section**; the other namespaces above will follow this shape. EMS is the domain behind the Areas → Projects → Tasks walkthrough in the engine's [Getting Started guide](https://github.com/kitelev/exocortex/blob/main/docs/tutorials/Getting-Started.md) — that guide teaches the *mechanism*; this section is the *domain model*.

EMS models **work** as a hierarchy of **Efforts** with a tracked **status lifecycle**.

### Core classes

| Class | Role |
| --- | --- |
| `ems__Area` | A broad domain of work (e.g. "Development", "Personal"). Areas nest into a tree. |
| `ems__Project` | A specific initiative within an Area. A project is a **parent effort** (it can contain tasks). |
| `ems__Task` | A concrete work item within a project (or a parent task, for sub-tasks). |
| `ems__Meeting` | A meeting effort (with format/online variants). |
| `ems__Effort` | The shared base class — anything with a status, timestamps, and votes. `Project`, `Task`, and `Meeting` all specialise it. |
| `ems__ParentEffort` | The marker class an effort must belong to to be a valid `ems__Effort_parent` target (Projects qualify; plain Tasks do not). |

There are also prototype classes (`ems__TaskPrototype`, `ems__ProjectPrototype`, `ems__MeetingPrototype`, `ems__RegularDailyTaskPrototype`) used to spawn recurring efforts, and refinement classes (`ems__SimpleTask`, `ems__ProjectBug`, `ems__CommunicationResponseTask`, zone/size/complexity/context enums).

### Status lifecycle

The canonical flow:

```
Draft → Backlog → Analysis → ToDo → Doing → Done
```

with side states `Trashed`, `Cancelled`, `Failed`, and waiting states (`Blocked`, `Waiting`, `Review`, `ThisWeek`, `Planned`). Each is an `ems__EffortStatus` enum individual:

| Status value | Meaning |
| --- | --- |
| `ems__EffortStatusDraft` | Initial idea, not yet committed |
| `ems__EffortStatusBacklog` | Committed, awaiting analysis |
| `ems__EffortStatusAnalysis` | Being analysed / planned |
| `ems__EffortStatusToDo` | Ready to start |
| `ems__EffortStatusDoing` | In progress |
| `ems__EffortStatusReview` | Awaiting review |
| `ems__EffortStatusBlocked` / `…Waiting` | Stalled on an external dependency |
| `ems__EffortStatusDone` | Completed |
| `ems__EffortStatusTrashed` / `…Cancelled` / `…Failed` | Terminal, not completed |

Status transitions are driven by the homoiconic command layer (the `ems-commands` namespace + the engine's `exocmd` AssetSpace), so the buttons you see on an asset's layout (Set Status Doing, Mark Done, …) come from data, not hardcoded UI.

### Key properties

| Property | On | Purpose |
| --- | --- | --- |
| `ems__Effort_area` | Project / Task | Links to the parent `ems__Area` |
| `ems__Effort_parent` | Task | Links to the parent Project (or parent Task) — target must be an `ems__ParentEffort` |
| `ems__Effort_status` | any Effort | Current lifecycle status (one of the values above) |
| `ems__Effort_plannedStartTimestamp` | any Effort | Planned start (the Daily-Note layout filters by this) |
| `ems__Effort_startTimestamp` / `…endTimestamp` | any Effort | Actual start / end — **fact overrides plan** when both are present |

### Minimal example

```yaml
---
exo__Instance_class:
  - "[[ems__Task]]"
exo__Asset_label: Set up Express server
ems__Effort_area: "[[Development]]"
ems__Effort_parent: "[[Build API Server]]"
ems__Effort_status: "[[ems__EffortStatusToDo]]"
---
```

In a vault that has this AssetSpace mounted, the plugin renders this note's layout (properties, status/creation/planning buttons, related-asset tables) in Reading Mode. The recommended way to create efforts is the in-layout **Create Project / Create Task** buttons, which write the correct frontmatter (canonical UID-form class refs) for you.

---

## Using this AssetSpace

You don't clone this repo into your vault by hand — you **mount** it through Exocortex:

- **CLI:** `npx @kitelev/exocortex-cli assetspace-add --vault ~/vault --url https://github.com/kitelev/exoas-public`
- **Plugin:** command palette → **Exocortex: Add a knowledge pack** → paste the repo URL. Or apply a Profile that includes it (`Exocortex: Apply profile`), which also resolves its dependencies.

Once mounted, its namespaces appear under `assetspaces/kitelev/exoas-public/<namespace>/` and the classes resolve in SPARQL, layouts, and commands.

## Conventions (for contributors)

- **UID-canon.** Every asset is a UUID-named file (`<exo__Asset_uid>.md`); the human label lives in `exo__Asset_label`. Wikilinks between assets are by UID.
- **Co-location.** An asset lives in the folder of the ontology its `exo__Asset_isDefinedBy` points at (each namespace = one folder).
- **CI.** Pushes run the shared [AssetSpace SHACL gate](https://github.com/kitelev/exoas-ci) (`validate schema --shapes-mode` over the merged dependency closure).

## See also

- [Exocortex engine repo](https://github.com/kitelev/exocortex) — the plugin + CLI.
- [Exo-as-SDK topology](https://github.com/kitelev/exocortex/blob/main/docs/explanation/assetspace-sdk-topology.md) — why the vault is split across many `exoas-*` repos.
- [Getting Started](https://github.com/kitelev/exocortex/blob/main/docs/tutorials/Getting-Started.md) — install → bootstrap → profile → apply (uses EMS as the example domain).

## License

MIT — see the [engine repo](https://github.com/kitelev/exocortex/blob/main/LICENSE).
