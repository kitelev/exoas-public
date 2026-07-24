---
exo__Asset_uid: f7ee45c4-209d-4fb9-bf16-ccacea15ce04
exo__Asset_createdAt: 2026-07-24T23:20:11
exo__Asset_updatedAt: 2026-07-24T23:20:11
exo__Instance_class:
  - "[[ca92458c-7da6-4c24-b42d-01dc561eb873]]"
exo__Asset_createdBy: "[[4ef3962d-b8a7-42b5-bd28-88ec846f1d13]]"
exo__Asset_label: Homoiconic command
aliases:
  - Homoiconic command
exodoc__ClassConcept_describesClass: "[[790e5b16-251d-4556-96ac-e5c7f1429b2e|exocmd__Command]]"
exo__Asset_isDefinedBy: "[[a947cea4-6ce3-4e8e-b5e1-597371b6a3b2|$exodoc/docs]]"
---

**Homoiconic command** (`exocmd__Command`) — команда/кнопка, **описанная данными в vault**, а не хардкодом в TypeScript. Ядро гомоиконичности Exocortex.

## Как оживает (3 части)
1. **`exocmd__CommandBinding`** привязывает команду к классу/прототипу (`targetClass` / `targetPrototype`) → определяет, на каких ассетах показывать кнопку.
2. **`exocmd__Command_precondition`** (SPARQL-ASK / hostFunction) гейтит видимость.
3. **`exocmd__Command_grounding`** описывает ДЕЙСТВИЕ: `property_set` / `create_instance` / `composite` / `service_call` / `workflow_transition`.

## Инвариант orphan
Нет binding → команда orphan → кнопка НЕ рендерится (хотя CLI `apply <cliName>` работает и без binding — резолвит по cliName напрямую).

## Parity
- **UI ↔ CLI:** одна команда работает и кнопкой в плагине, и `exocortex-cli apply`.
- **Desktop ↔ Mobile:** команда обязана работать на обеих платформах (нет desktop-only gating).

## Композиция > конфигурация (φ/EO)
Новая семантика = композиция groundings (данные), а не флаг/if в коде.

RFC: `c78cc5c8` (Homoiconicity Invariant).

