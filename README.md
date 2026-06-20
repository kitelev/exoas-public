# exoas-public — Exocortex public TBox AssetSpace

**The shared, public ontology library for [Exocortex](https://github.com/kitelev/exocortex).** This repository is an **AssetSpace** — a git-backed package of vault assets that you mount into an Exocortex vault. It holds the *domain vocabularies* (ontologies) that the Exocortex engine itself does not ship: tasks and projects, concepts, people, time periods, strategy frameworks, and more.

> **Where this fits.** Exocortex deliberately separates **the engine** from **the data it operates on**. The engine (`exo` — the RDF parser, SPARQL engine, layout renderer, command machinery, shipped as the plugin + `@kitelev/exocortex-cli`) is domain-agnostic. Every *domain* it works with lives in a mountable AssetSpace like this one. See the engine's [Exo-as-SDK topology](https://github.com/kitelev/exocortex/blob/main/docs/explanation/assetspace-sdk-topology.md) for the full model (`exo` = platform, an AssetSpace = a library/jar, a Profile = a manifest).

---

## What's in here

`exoas-public` bundles many **namespaces** — one folder per ontology. Each namespace owns its own classes, properties, and (where relevant) command/grounding definitions. Every asset is a UID-named Markdown file (`<uuid>.md`) co-located in its namespace folder.

| Namespace | What it is | Docs |
| --- | --- | --- |
| **`ems`** | **Effort Management** — Areas, Projects, Tasks, Meetings + a status lifecycle. The Areas → Projects → Tasks model behind most of the Getting-Started walkthrough. | **[↓ documented](#ems--effort-management-pilot)** |
| `concept` | Concepts / knowledge atoms (the successor to the legacy `ims` namespace). | [↓ documented](#concept--knowledge-atoms) |
| `person` | People — `person__Person` (legacy `ims__Person` is deprecated, being merged in). | [↓ documented](#person--people) |
| `period` | Calendar periods — Day · Week · Month · Quarter · Halfyear · Year. | [↓ documented](#period--calendar-periods) |
| `pn` | Periodic notes — DailyNote, WeeklyNote, PeriodicNote. | [↓ documented](#pn--periodic-notes) |
| `ztlk` | Zettelkasten base note (`ztlk__Note`). Permanent/Fleeting variants live in the `shared-identities` AssetSpace. | [↓ documented](#ztlk--zettelkasten-notes) |
| `strategy` | Strategy / objectives — Pillars, Initiatives, Strategy. | [↓ documented](#strategy) |
| `vmost` | VMOST strategic-planning framework (Vision · Mission · Objectives · Strategy · Tactics). | [↓ documented](#vmost) |
| `para` | PARA method (Projects · Areas · Resources · Archives). | [↓ documented](#para) |
| `jedi` | "Jedi Techniques" productivity framework. | [↓ documented](#jedi) |
| `pmi` | PMI/PMBOK study vocabulary (performance domains, principles, models, exam prep). | [↓ documented](#pmi) |
| `place` | Places / locations — `place__Place`. | [↓ documented](#place) |
| `role` | Roles — `role__Role`. | [↓ documented](#role) |
| `agent` | Agents — `ems__Agent`. | [↓ documented](#agent) |
| `ref` | References / bibliography — `ref__URLReference`. | [↓ documented](#ref) |
| `kf` | Knowledge fields — Academic Disciplines, Interdisciplinary Fields. | [↓ documented](#kf) |
| `ui` | UI vocabulary anchor (layout/column classes ship in the `exo` core AssetSpace). | [↓ documented](#ui) |
| `exoql` | ExoQL query vocabulary — `exoql__Query`, `query__NamedQuery`. | [↓ documented](#exoql) |
| `exotemplate` | Templating vocabulary (`exotemplate__Template`, `body_template` groundings). | [↓ documented](#exotemplate) |
| `exoob` | Declarative rule classes (`exoob__PrintNameRule`, `exoob__PropertyAliasFactory`). | [↓ documented](#exoob) |
| `tools` | Dashboard / utility vocabulary — `tools__DashboardUtil`. | [↓ documented](#tools) |
| `vault` | Vault structure — `vault__Vault`, `vault__Folder`. | [↓ documented](#vault) |
| `ts` | Metric / KPI tracking vocabulary — `ts__Metric`. | [↓ documented](#ts) |
| `w3c`, `ims`, `profession`, `metric`, `kpc`, `concepts-public`, `exo-ims` | Anchors, the deprecated legacy `ims` namespace, and reserved/placeholder vocabularies. | [↓ documented](#other-namespaces) |
| `ems-commands`, `period-commands` | Homoiconic exocmd command definitions (status buttons, daily-note commands) — not classes. | [↓ documented](#other-namespaces) |
| `adapter-ems-person`, `adapter-ems-place`, `adapter-exo-ims` | Cross-ontology adapter shims that bridge two namespaces. | [↓ documented](#other-namespaces) |

> The classes themselves are live and usable today regardless of doc depth — this README is reference prose, not a gate. Each section below is built from the classes that **actually exist on disk** in this repo.

---

## EMS — Effort Management (pilot)

> This is the **fully-written reference section**; the other namespaces follow this shape. EMS is the domain behind the Areas → Projects → Tasks walkthrough in the engine's [Getting Started guide](https://github.com/kitelev/exocortex/blob/main/docs/tutorials/Getting-Started.md) — that guide teaches the *mechanism*; this section is the *domain model*.

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

## concept — knowledge atoms

`concept` is the **knowledge-modelling** namespace — the successor to the legacy `ims` namespace. A `concept__Concept` is an atom of meaning (an idea, term, or entity) that you connect into a graph and reference from notes, tasks, and definitions.

### Core classes

| Class | Role |
| --- | --- |
| `concept__Concept` | The base knowledge atom. |
| `concept__CharacterizedConcept` | A concept carrying structured characteristics. |
| `concept__ConceptSchema` | A scheme that groups related terms (a SKOS-style concept scheme). |
| `concept__Term` | A lexical term within a scheme. |
| `concept__Definition` | A definition that *defines* a concept. |
| `concept__ConceptDescription` | A longer descriptive record attached to a concept. |
| `concept__Context` | A context in which a concept holds. |
| `concept__ConceptFunction` | A function mapping concept inputs to outputs. |
| `concept__System` | A system of related concepts. |
| `concept__Equality` / `concept__Reciproxity` | Reified relationship classes (equality, reciprocity). |
| `concept__Adjective` / `concept__Movie` / `concept__Video` / `concept__Series` | Typed concept refinements (a movie, a video, a series, an adjective). |
| `concept__ConceptPrototype` | Prototype used to spawn new concepts with defaults. |

### Key properties

| Property | Purpose |
| --- | --- |
| `concept__Concept_broader` | Links a concept to a broader (parent) concept — SKOS-style taxonomy. |
| `concept__Concept_related` | Associates two concepts non-hierarchically. |
| `concept__Concept_definition` | Links a concept to its `concept__Definition`. |
| `concept__Concept_schema` | Places a concept into a `concept__ConceptSchema`. |
| `concept__Term_lexicalForm` | The written form of a term. |

### Minimal example

```yaml
---
exo__Instance_class:
  - "[[concept__Concept]]"
exo__Asset_label: Spaced repetition
concept__Concept_broader: "[[Memory techniques]]"
---
```

---

## person — people

`person` models **people**. `person__Person` is the canonical class; `ims__Person` is the deprecated legacy class (marked `exo__DeprecatedClass`), kept while existing data is re-prefixed.

### Core classes

| Class | Role |
| --- | --- |
| `person__Person` | A person (canonical). |
| `ims__Person` | **Deprecated** legacy person class — being merged into `person__Person`. |
| `ims__Person_knows` | Reified "knows" relationship between two people. |

### Key properties

| Property | Purpose |
| --- | --- |
| `ims__Person_birthDayOfMonth` | The person's birthday (links to a `period__DayOfMonth`). |

### Minimal example

```yaml
---
exo__Instance_class:
  - "[[person__Person]]"
exo__Asset_label: Ada Lovelace
---
```

---

## period — calendar periods

`period` models **time**. Periods nest into a tree (a Day belongs to a Month, a Month to a Quarter, …) via `period__Period_parent`, so you can roll efforts and notes up to any granularity.

### Core classes

| Class | Role |
| --- | --- |
| `period__Period` | The shared base class for all calendar periods. |
| `period__Day` | A calendar day. |
| `period__Week` | A calendar week. |
| `period__Month` | A calendar month. |
| `period__Quarter` | A calendar quarter. |
| `period__Halfyear` | A half-year. |
| `period__Year` | A calendar year. |
| `period__DayOfMonth` | A day-of-month abstraction (e.g. for recurring birthdays). |

### Key properties

| Property | Purpose |
| --- | --- |
| `period__Period_parent` | Links a period to its enclosing period (Day → Month → Quarter → Year). |
| `period__Month_year` / `period__Quarter_year` / `period__Halfyear_year` | Link a sub-period to its year. |
| `period__DayOfMonth_day` / `period__DayOfMonth_month` | Decompose a day-of-month. |

---

## pn — periodic notes

`pn` provides the note classes that the calendar/periodic-note workflow uses. They pair with the `period` classes and the `period-commands` command definitions.

### Core classes

| Class | Role |
| --- | --- |
| `pn__PeriodicNote` | The base class for any periodic note. |
| `pn__DailyNote` | A daily note (`YYYY-MM-DD`), linked to a `period__Day`. |
| `pn__WeeklyNote` | A weekly note (`YYYY-Www`), linked to a `period__Week`. |

### Key properties

| Property | Purpose |
| --- | --- |
| `pn__DailyNote_day` | Links a daily note to its `period__Day`. |
| `pn__WeeklyNote_week` | Links a weekly note to its `period__Week`. |
| `pn__PeriodicNote_period` | Links any periodic note to its `period__Period`. |

> Daily/Weekly note files are the two label-named exceptions to UID-canon (`YYYY-MM-DD.md` / `YYYY-Www.md`) so the Obsidian calendar plugin can recognise them by basename.

---

## ztlk — zettelkasten notes

`ztlk` carries the base Zettelkasten note class.

| Class | Role |
| --- | --- |
| `ztlk__Note` | The base Zettelkasten note. |

> The richer **Permanent** and **Fleeting** note variants (`ztlk__PermanentNote`, `ztlk__FleetingNote`) live in the **`shared-identities`** AssetSpace, not here — mount that AssetSpace if you use the full Zettelkasten workflow.

---

## Strategy and planning frameworks

These namespaces model goal-setting and strategic-planning vocabularies. They are small, self-contained class sets you can adopt independently.

### strategy

| Class | Role |
| --- | --- |
| `strategy__Strategy` | A strategy — the top container. |
| `strategy__Pillar` | A strategic pillar (a theme the strategy is organised around). |
| `strategy__Initiative` | An initiative that advances a pillar. |

### vmost

The VMOST strategic-planning framework, top-down:

| Class | Role |
| --- | --- |
| `vmost__Vision` | The long-term vision. |
| `vmost__Mission` | The mission serving the vision. |
| `vmost__Objective` | A measurable objective. |
| `vmost__Strategy` | A strategy to reach objectives. |
| `vmost__Tactics` | Concrete tactics implementing a strategy. |

### para

The PARA method (Projects · Areas · Resources · Archives). This namespace ships the `para__Resource` class; Projects and Areas reuse the `ems` namespace.

### jedi

The "Jedi Techniques" productivity framework namespace (anchor + framework individuals such as task-criticality zones). It supplies enum-style individuals consumed by EMS layouts rather than new classes.

---

## Reference and utility vocabularies

Smaller domain and utility vocabularies. Each is a single-purpose class set.

### pmi

PMI/PMBOK **study** vocabulary — built for project-management exam preparation.

| Class | Role |
| --- | --- |
| `pmi__PerformanceDomain` | A PMBOK performance domain. |
| `pmi__Principle` | A project-management principle. |
| `pmi__Model` / `pmi__Method` / `pmi__Artifact` | Models, methods, and artifacts from the PMBOK Guide. |
| `pmi__StudyPlan` | A study plan. |
| `pmi__ExamQuestion` | A practice exam question. |

### place

| Class | Role |
| --- | --- |
| `place__Place` | A place / location. |

### role

| Class | Role |
| --- | --- |
| `role__Role` | A role (e.g. a position a person holds). |

### agent

| Class | Role |
| --- | --- |
| `ems__Agent` | An agent (human or automated actor). |

### ref

| Class | Role |
| --- | --- |
| `ref__URLReference` | A URL-based reference / bibliography entry. |

### kf

Knowledge-field vocabulary.

| Class | Role |
| --- | --- |
| `kf__Academic Discipline` | An academic discipline. |
| `kf__InterdisciplinaryField` | An interdisciplinary field. |

### ui

The `ui` folder is the namespace **anchor** only. The actual UI vocabulary classes (layouts, relation-column sets) ship in the **`exo` core AssetSpace**, because the layout engine is part of the engine, not a domain. Mount `exoas-exo` for them.

### exoql

ExoQL query vocabulary — lets you store named queries as assets.

| Class | Role |
| --- | --- |
| `exoql__Query` | A stored ExoQL query. |
| `query__NamedQuery` | A named, reusable query. |

### exotemplate

| Class | Role |
| --- | --- |
| `exotemplate__Template` | A body template used by `body_template` groundings to scaffold note bodies on creation. |

### exoob

Declarative rule classes that configure engine display/alias behaviour from data (homoiconic).

| Class | Role |
| --- | --- |
| `exoob__PrintNameRule` | A rule for how an asset's name is printed. |
| `exoob__PropertyAliasFactory` | A factory for property aliases. |

### tools

| Class | Role |
| --- | --- |
| `tools__DashboardUtil` | A dashboard / utility helper. |

### vault

| Class | Role |
| --- | --- |
| `vault__Vault` | A vault. |
| `vault__Folder` | A folder within a vault. |

### ts

Metric / KPI tracking vocabulary.

| Class | Role |
| --- | --- |
| `ts__Metric` | A tracked metric or KPI. |

Plus the `ts__Metric_influences` object-property (relationship: one metric influences another).

---

## Other namespaces

These namespaces hold (essentially) no domain classes of their own — they are anchors, command definitions, adapters, or the deprecated legacy namespace:

- **`w3c`** — anchors for standard W3C vocabularies (see also the `exoas-w3c-aggregated` AssetSpace).
- **`ims`** — the **deprecated legacy** namespace (concepts/notes/people). Being re-prefixed to `concept` / `person`; kept for back-references. (Holds no `ims__` domain classes; one stray legacy `inbox__Quote` artifact remains.)
- **`ems-commands`**, **`period-commands`** — homoiconic **exocmd command** definitions (the status buttons and daily-note commands), not classes.
- **`adapter-ems-person`**, **`adapter-ems-place`**, **`adapter-exo-ims`**, **`exo-ims`** — cross-ontology **adapter shims** that bridge two namespaces.
- **`profession`** — profession anchor + individuals (e.g. `QA`); no classes.
- **`metric`**, **`kpc`**, **`concepts-public`** — reserved / placeholder namespace anchors.

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
