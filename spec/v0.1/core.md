# OLE Core v0.1 Draft

**Status:** v0.1 — Published  
**Published by:** Open Legal Exchange (openlegalexchange.org)  
**Author:** Philippe Chaunu, CyVine LLC  
**Date:** May 2026  
**Maturity:** v0.1 foundation — production schemas in future versions

---

## Status of This Document

This document defines the shared conventions for all OLE modules: the resource envelope, identifier policy, visibility scope, consent model, audit model, external identifier support, and extension policy.

These conventions are stable for v0.1 and open for implementation. Final normative JSON Schemas, machine-testable conformance requirements, and a production conformance test suite will be published in a future Core specification.

---

## Normative Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

Because this v0.1 document is a founding draft, most language describes intended behavior. Normative requirements will be finalized in a future Core v1.0 specification.

---

## Purpose

OLE Core defines the shared conventions, resource types, envelope fields, identifiers, visibility scopes, consent model, audit model, external IDs, and extension policy used across all OLE modules.

Every OLE module depends on Core. Core depends on nothing else.

---

## Core Primitives

The six governance primitives are the conceptual foundation of OLE. They are defined in full in the Founding Architecture document. This specification defines the data conventions they share.

| Primitive | Description |
|-----------|-------------|
| `Rule` | An authoritative directive that creates, constrains, or authorizes governance activity. |
| `Institution` | A body authorized by one or more rules to perform governance functions. |
| `Actor` | Any participant — human, organizational, or artificial — operating within a governance system. |
| `Action` | Any event within a governance system: referral, filing, vote, inspection, decision. |
| `Impact` | Aggregated, anonymized, privacy-preserving signals derived from governance actions. |
| `Feedback` | Structured signals flowing from impact data back to rulemaking bodies. |

---

## Shared Supporting Resources

The following supporting resources are defined in Core and shared across all modules:

| Resource | Description |
|----------|-------------|
| `Membership` | Association between an Actor and an Institution, with a defined role and status. |
| `Credential` | A verifiable credential held by an Actor, issued by a credentialing authority. |
| `ConsentRecord` | Explicit documented consent for personal data exchange in an OLE transaction. |
| `AuditEvent` | A structured log record of a consequential OLE action. |
| `Visibility` | Access-scope declaration on an OLE resource. |
| `JurisdictionContext` | Geographic and regulatory jurisdiction descriptor. |
| `PlatformContext` | Originating platform identity and conformance profile. |
| `ExternalId` | An identifier from a non-OLE system linked to an OLE resource. |
| `Extension` | Namespaced implementer-specific fields attached to any OLE resource. |

---

## Resource Envelope

Every OLE resource MUST use a common envelope. The envelope provides the minimum metadata needed to identify, route, audit, validate, and extend a resource.

A module may add fields to the envelope but MUST NOT remove or redefine envelope fields.

### Required Envelope Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `resourceType` | string | **MUST** | OLE resource type, e.g., `Actor`, `Referral`, `AuditEvent`. |
| `id` | string | **MUST** | Stable identifier assigned by the issuing system. MUST NOT be reassigned. |
| `oleVersion` | string | **MUST** | OLE version used, e.g., `"0.1"`. |
| `module` | string | RECOMMENDED | Module namespace, e.g., `core`, `clear`. |
| `createdAt` | string (date-time) | **MUST** | ISO 8601 timestamp when the resource was created. |
| `updatedAt` | string (date-time) | RECOMMENDED | ISO 8601 timestamp when the resource was last updated. |
| `createdBy` | string | RECOMMENDED | OLE Actor ID of the actor that created the resource. |
| `owningInstitution` | string | RECOMMENDED | OLE Institution ID of the institution responsible for the resource. |
| `jurisdiction` | object | RECOMMENDED | Jurisdictional context. |
| `visibility` | object | **MUST** | Visibility and intended access scope. |
| `platformContext` | object | RECOMMENDED | Originating platform and conformance profile. |
| `externalIds` | array | OPTIONAL | Identifiers from other systems. |
| `extensions` | object | **MUST** | Namespaced implementer-specific fields (may be empty `{}`). |

### Example Envelope

```json
{
  "resourceType": "Referral",
  "id": "ole_ref_2026_0522_001",
  "oleVersion": "0.1",
  "module": "clear",
  "createdAt": "2026-05-22T16:45:00-04:00",
  "updatedAt": "2026-05-22T16:45:00-04:00",
  "createdBy": "ole_actor_roxana_tejeda",
  "owningInstitution": "ole_inst_tejeda_law",
  "jurisdiction": {
    "country": "US",
    "state": "FL",
    "county": "Miami-Dade"
  },
  "visibility": {
    "scope": "participant_visible"
  },
  "platformContext": {
    "originatingPlatform": "certusgate",
    "conformanceProfile": "ole-clear-exchange-draft-v0.1"
  },
  "externalIds": [],
  "extensions": {}
}
```

---

## Identifier Policy

### Format

OLE identifiers MUST be stable within the issuing system and MUST NOT be reassigned to a different resource.

The RECOMMENDED human-readable format is:

```text
ole_{resource_abbreviation}_{context}_{unique_suffix}
```

Examples:

```text
ole_actor_roxana_tejeda
ole_inst_tejeda_law
ole_ref_2026_0522_001
ole_rule_fl_bar_4_1_5
ole_audit_2026_0522_001
ole_consent_2026_0522_001
```

Systems MAY use UUIDs, ULIDs, or URIs instead of the human-readable format. Implementers SHOULD choose one approach per system and use it consistently.

### Uniqueness Scope

OLE does not mandate globally unique identifiers in v0.1. However, resources that cross system boundaries MUST include sufficient context to disambiguate their origin. Using the `platformContext.originatingPlatform` field together with the resource `id` is the minimum disambiguation strategy.

### External Identifiers

External system identifiers SHOULD be represented in `externalIds` rather than replacing the OLE identifier.

---

## External IDs

OLE resources SHOULD support `externalIds` so implementers can connect OLE resources to existing systems without requiring migration.

```json
"externalIds": [
  {
    "system": "certusgate",
    "type": "matter_id",
    "value": "mat_12345"
  },
  {
    "system": "florida_bar",
    "type": "member_number",
    "value": "123456"
  },
  {
    "system": "sali",
    "type": "practice_area_code",
    "value": "SALI:HOA-LAW"
  }
]
```

| Field | Description |
|-------|-------------|
| `system` | Identifier of the external system (e.g., `certusgate`, `florida_bar`, `sali`). |
| `type` | Type of external identifier within that system. |
| `value` | The external identifier value. |

External IDs are especially important for practice management systems, bar association databases, court e-filing systems, government records systems, and credential registries.

---

## Extension Policy

Every OLE resource MUST include an `extensions` object. It MAY be empty (`{}`).

Extensions allow implementers to add local, experimental, or vendor-specific fields without breaking the base standard.

### Rules for Extensions

1. Extension fields MUST NOT redefine the meaning of core fields.
2. Extension field names SHOULD be namespaced using the pattern `{namespace}:{fieldName}`.
3. OLE-reserved field names MUST NOT appear in `extensions`.
4. Future OLE versions MAY promote widely adopted extensions into standard fields.

### Example

```json
"extensions": {
  "certusgate:intakePriority": "high",
  "certusgate:campaignSource": "google_ads",
  "florida:countyCourtDivision": "civil"
}
```

---

## Visibility Scope

Every OLE resource MUST declare a `visibility` object with a `scope` field.

Visibility is a portable declaration of the intended access scope. It does not replace authentication, authorization, attorney-client privilege, HIPAA, or other legal confidentiality obligations. Implementations MUST enforce access control independently of the `visibility` field.

| Scope | Meaning |
|-------|---------|
| `private` | Visible only to the owning actor or institution. |
| `participant_visible` | Visible to named participants in the exchange. |
| `institution_visible` | Visible within the owning institution. |
| `network_visible` | Visible to a defined referral, professional, civic, or institutional network. |
| `public` | Intended for public release. |
| `aggregate_public` | Public only after aggregation and privacy thresholds are met. Impact resources use this scope. |

The default visibility scope for CLEAR referral resources is `participant_visible`.

Impact resources MUST use `aggregate_public`. They MUST NOT contain PII.

---

## Consent Record

OLE exchanges involving personal, client, patient, citizen, or prospect data SHOULD support explicit consent records.

A `ConsentRecord` documents: who gave consent, what was consented to, which data may be shared, with whom it may be shared, and when the consent expires or is revoked.

### Required Fields

| Field | Description |
|-------|-------------|
| `subject` | OLE ID of the person whose data is covered. |
| `grantedTo` | Array of OLE Institution or Actor IDs authorized to receive data. |
| `grantedBy` | OLE ID of the person who granted consent (usually the subject). |
| `purpose` | Purpose of the consent, e.g., `legal_referral_review`. |
| `dataScopes` | Array of data categories covered, e.g., `contact_information`, `matter_summary`. |
| `status` | `active`, `revoked`, or `expired`. |
| `grantedAt` | ISO 8601 timestamp of consent. |
| `expiresAt` | ISO 8601 timestamp when consent expires. `null` if no expiry. |

### Consent Principles

- Consent SHOULD be obtained before prospect or client data is shared with any receiving participant.
- Consent records SHOULD be linked from the Referral resource via the `feeArrangement.consentRecord` field or a direct `consentRecord` field.
- Consent for fee-sharing MUST comply with applicable bar rules (e.g., Florida Bar Rule 4-1.5 for Florida attorneys). OLE structures the record — it does not create or verify the ethical obligation.
- Consent may be revoked. Implementations SHOULD support updating `status` to `revoked` and recording `revokedAt`.

### Example

```json
{
  "resourceType": "ConsentRecord",
  "id": "ole_consent_2026_0522_001",
  "oleVersion": "0.1",
  "module": "core",
  "createdAt": "2026-05-22T16:44:00-04:00",
  "updatedAt": "2026-05-22T16:44:00-04:00",
  "subject": "ole_prospect_2026_0522_001",
  "grantedTo": ["ole_inst_santos_hoa_law"],
  "grantedBy": "ole_prospect_2026_0522_001",
  "purpose": "legal_referral_review",
  "dataScopes": [
    "contact_information",
    "matter_summary",
    "uploaded_documents"
  ],
  "status": "active",
  "grantedAt": "2026-05-22T16:44:00-04:00",
  "expiresAt": "2026-06-22T16:44:00-04:00",
  "revokedAt": null,
  "evidence": {
    "method": "electronic_acknowledgment",
    "ipAddressRecorded": true,
    "userAgentRecorded": true
  },
  "visibility": {
    "scope": "participant_visible"
  },
  "extensions": {}
}
```

---

## Audit Event

Every consequential OLE action SHOULD produce an `AuditEvent` record.

Audit events support compliance documentation, dispute resolution, ethics reviews, and system debugging.

### Required Fields

| Field | Description |
|-------|-------------|
| `actor` | OLE ID of the actor who performed the action. |
| `actionType` | Enumerated action type, e.g., `referral.sent`, `consent.granted`, `status.updated`. |
| `resourceType` | OLE resource type acted upon. |
| `resourceId` | ID of the resource acted upon. |
| `timestamp` | ISO 8601 timestamp of the event. |

### AI Agent Audit Fields

When the acting `actor` is an `ai_agent`, the following additional fields SHOULD be recorded:

| Field | Description |
|-------|-------------|
| `logInputs` | Structured record of inputs provided to the AI agent for this action. |
| `logOutputs` | Structured record of outputs produced by the AI agent. |
| `logDecisionRationale` | Human-readable summary of the reasoning behind the decision (where available). |
| `logEvidenceReferences` | References to documents, rules, or data sources consulted. |
| `logModelVersion` | Version identifier of the AI model used. |
| `logPolicyVersion` | Version identifier of the policy or constraint set applied. |

These fields enable after-the-fact review of AI agent behavior without claiming real-time explainability.

### Example

```json
{
  "resourceType": "AuditEvent",
  "id": "ole_audit_2026_0522_001",
  "oleVersion": "0.1",
  "module": "clear",
  "createdAt": "2026-05-22T16:46:00-04:00",
  "actor": "ole_actor_roxana_tejeda",
  "actorType": "human",
  "actionType": "referral.sent",
  "resourceType_ref": "Referral",
  "resourceId": "ole_ref_2026_0522_001",
  "timestamp": "2026-05-22T16:46:00-04:00",
  "note": "Referral sent to Maria Santos at Santos HOA Law.",
  "visibility": {
    "scope": "participant_visible"
  },
  "extensions": {}
}
```

### AI Agent Audit Example

```json
{
  "resourceType": "AuditEvent",
  "id": "ole_audit_2026_0522_002",
  "oleVersion": "0.1",
  "module": "clear",
  "createdAt": "2026-05-22T16:45:00-04:00",
  "actor": "ole_actor_cg_receptionist_v2",
  "actorType": "ai_agent",
  "actionType": "intake.classify",
  "resourceType_ref": "Referral",
  "resourceId": "ole_ref_2026_0522_001",
  "timestamp": "2026-05-22T16:45:00-04:00",
  "aiAudit": {
    "logInputs": {
      "prospectDescription": "HOA notice and assessment dispute in Miami-Dade",
      "jurisdiction": "FL"
    },
    "logOutputs": {
      "classifiedPracticeArea": "housing.hoa_dispute",
      "confidence": 0.94,
      "routedTo": "ole_actor_roxana_tejeda"
    },
    "logDecisionRationale": "Classified as HOA dispute based on keywords and jurisdiction. Routed to attorney with active HOA practice area profile.",
    "logEvidenceReferences": ["ole_rule_fs718"],
    "logModelVersion": "example_model_family@2.1.0",
    "logPolicyVersion": "certusgate-routing-policy@1.3.0"
  },
  "visibility": {
    "scope": "institution_visible"
  },
  "extensions": {}
}
```

---

## Error Model

OLE API implementations SHOULD return structured errors using a common envelope.

```json
{
  "error": {
    "code": "ole.validation.required_field_missing",
    "message": "The field `participants` is required.",
    "resourceType": "Referral",
    "field": "participants",
    "traceId": "trace_01HXAMPLE123",
    "documentationUrl": "https://openlegalexchange.org/errors/ole.validation.required_field_missing"
  }
}
```

---

## Idempotency

OLE API implementations SHOULD support idempotency keys for create and mutation operations. This prevents duplicate referrals, status updates, and audit events during network retries.

Clients SHOULD send an `Idempotency-Key` header with mutation requests. Servers SHOULD return the original response when a duplicate request arrives with the same key and body.

---

## Webhook Signing

OLE webhooks SHOULD be signed so receiving systems can verify authenticity.

Recommended headers:

```text
OLE-Signature: sha256=<hex_signature>
OLE-Timestamp: 2026-05-22T16:45:00Z
OLE-Event-Id: ole_evt_2026_0522_001
```

Receiving systems SHOULD reject events with stale timestamps (beyond a configurable window), invalid signatures, or duplicate event IDs.

---

## Scope of Future Versions

The following are planned for future Core specifications:

- Final normative JSON Schemas for all Core resources
- ConsentRecord lifecycle and revocation API
- AuditEvent retention and deletion rules
- Membership resource schema
- Credential verification protocol
- JurisdictionContext enumerated values
- PlatformContext conformance declaration format
- Core test suite and conformance sandbox
