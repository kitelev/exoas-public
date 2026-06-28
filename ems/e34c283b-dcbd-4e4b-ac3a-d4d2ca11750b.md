---
exo__Asset_isDefinedBy: "[[f6e01f7a-d727-494a-82a3-815597d33e86|$ems]]"
exo__Asset_uid: e34c283b-dcbd-4e4b-ac3a-d4d2ca11750b
exo__Asset_createdAt: 2026-06-28T10:50:55
exo__Instance_class:
  - "[[9a1cf31c-9d41-4ef3-9023-584a8d087d16]]"
exo__Property_domain: "[[086f71fa-dd30-4284-90cf-e609f2a6c461]]"
exo__Property_range: "[[90f77d9e-27f9-48f2-ad1a-8490d99eb088]]"
exo__Property_cardinality: "[[089d1898-a61f-4ba4-b20f-a1b329be5e74]]"
exo__Asset_label: ems__Effort_trashedReason
exo__Property_displayName: trashedReason
---

# ems__Effort_trashedReason

Причина, по которой эффорт был перемещён в `Trashed` (а не доведён до `Done`). Значения — инстансы enum-класса `ems__EffortTrashReason` (controlled vocabulary, как `ems__EffortStatus`) → агрегируется SPARQL'ом для трекинга паттернов («сколько задач я бросаю недоделанными за период»).

Domain: `ems__Effort`. Range: `ems__EffortTrashReason`. Cardinality: single (0..1, проставляется при статусе `Trashed`).
