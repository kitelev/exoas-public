---
exo__Asset_isDefinedBy: "[[be2c3f06-48db-4a38-a21d-a81ce1836815]]"
exo__Asset_uid: a6ef8fda-addb-40c3-940c-fe55fd7e8500
exo__Asset_createdAt: 2026-05-02T09:19:00
exo__Asset_updatedAt: 2026-05-22T00:49:05
exo__Asset_createdBy: "[[de20a3f1-7483-4714-ab28-b45f5cf02c76]]"
exo__Instance_class:
  - "[[11579feb-2e42-491c-af59-b89b1129a539]]"
exo__Asset_label: "Create TaskPrototype instance"
aliases:
  - "Create TaskPrototype instance"
exocmd__Grounding_type: "[[4367e2d6-6c92-450a-becb-abce1fb07682|exocmd__GroundingTypeCreateInstance]]"
exocmd__Grounding_targetFolder: "03 Knowledge/inbox"
exocmd__Grounding_targetClass: "ems__Task"
exocmd__Grounding_targetPrototype: "df7e579d-02d4-4f3a-971f-3d1d785b689b|ems__TaskPrototype"
exocmd__Grounding_inputSchema: '{"type":"object","properties":{"label":{"type":"string","title":"Task name"}},"required":["label"]}'
exocmd__Grounding_prefillLabelWithDate: true
exocmd__Grounding_propertyDefault:
  - "[[d9aa9bb8-5676-4ba2-ba5e-fc8d9df02250]]"
exocmd__Grounding_inheritanceRule:
  - "[[3f08f5a8-df11-47e7-8519-7d8d84175951]]"
  - "[[43731bae-78cb-4ffe-9eaa-4c258cb1c493]]"
  - "[[cbe000c4-b29a-4405-876d-790fb2296121]]"
---

# Create Task instance from prototype

Grounding для создания `ems__Task` instance из `ems__TaskPrototype`.

- `Grounding_type=create_instance` → `GroundingExecutor.executeCreateInstance`.
- `targetClass=ems__Task` → `exo__Instance_class: [[1b20a8f0-d745-4e93-91db-4531b3df120e]]` в новом файле.
- `targetPrototype=df7e579d-...|ems__TaskPrototype` → `exo__Asset_prototype: [[...]]` в новом файле.
- `targetFolder=03 Knowledge/inbox` — место создания файла.
- `inputSchema` — modal с полем label (Task name).

TaskPrototype variant of Create Instance Command (`[[bb00efed-7b17-42f5-a2c4-7cadf3e0ab36]]`).
