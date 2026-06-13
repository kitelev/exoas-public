---
exo__Asset_uid: d222ddaf-0a56-4bac-91e3-6af02fafebc8
exo__Asset_label: "Set scheduled date"
exo__Asset_isDefinedBy: "[[bf44251f-f18c-42a7-b612-e1036b2b6f80]]"
exo__Asset_createdAt: "2026-04-05T12:00:00"
exo__Asset_updatedAt: "2026-04-19T16:35:31"
exo__Instance_class:
  - "[[11579feb-2e42-491c-af59-b89b1129a539]]"
exocmd__Grounding_type: "[[9bf9fc99-ac37-4e51-b9f5-bd920099947c]]"
exocmd__Grounding_serviceId: "updateProperty"
exocmd__Grounding_serviceCallPayload: '{"property":"ems__Effort_scheduledDate"}'
exocmd__Grounding_inputSchema: '{"type":"object","properties":{"value":{"type":"string","title":"Scheduled date"}},"required":["value"]}'
---

# Set scheduled date

Grounding action that calls the `updateProperty` service to set `ems__Effort_scheduledDate`.

Uses `service_call` grounding type with `updateProperty` service and user-provided date value.
