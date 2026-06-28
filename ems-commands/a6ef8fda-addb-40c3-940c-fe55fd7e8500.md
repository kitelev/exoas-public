---
exo__Asset_isDefinedBy: "[[be2c3f06-48db-4a38-a21d-a81ce1836815]]"
exo__Asset_uid: a6ef8fda-addb-40c3-940c-fe55fd7e8500
exo__Asset_createdAt: 2026-05-02T09:19:00
exo__Asset_updatedAt: 2026-06-28T15:10:00
exo__Instance_class:
  - "[[11579feb-2e42-491c-af59-b89b1129a539]]"
exo__Asset_label: Create TaskPrototype instance
aliases:
  - Create TaskPrototype instance
exocmd__Grounding_type: "[[4367e2d6-6c92-450a-becb-abce1fb07682|exocmd__GroundingTypeCreateInstance]]"
exocmd__Grounding_targetFolder: $targetFolder
exocmd__Grounding_targetClass: ems__Task
exocmd__Grounding_targetPrototype: df7e579d-02d4-4f3a-971f-3d1d785b689b|ems__TaskPrototype
exocmd__Grounding_inputSchema: '{"type":"object","properties":{"label":{"type":"string","title":"Task name"}},"required":["label"]}'
exocmd__Grounding_inheritanceRule:
  - "[[cbe000c4-b29a-4405-876d-790fb2296121]]"
exocmd__Grounding_propertyDefault:
  - "[[d9aa9bb8-5676-4ba2-ba5e-fc8d9df02250]]"
exocmd__Grounding_prefillLabelWithDate: true
exocmd__Grounding_labelTemplate: $target.exo__Asset_label $today
---
