# CLEAR v0.1 Draft — Legal Referral Exchange

**Status:** Public Draft  
**Published by:** Open Legal Exchange (openlegalexchange.org)  
**Author:** Philippe Chaunu, CyVine LLC  
**Date:** May 2026  
**Maturity:** Draft — not a production specification  
**Module namespace:** `openlegalexchange.org/ns/clear`

---

## Status of This Document

This document is a draft specification for CLEAR, the Legal Referral Exchange module of OLE. It defines the referral workflow, resource model, status values, data minimization principles, consent requirements, and API surface for v0.1.

This specification does not define final normative JSON Schemas, production endpoint requirements, conformance tests, or certification procedures. Those artifacts will be published separately as the specification matures.

---

## Normative Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

---

## Purpose

CLEAR defines a structured model for legal referrals between attorneys, law firms, bar associations, legal aid organizations, referral services, and other authorized legal-service participants.

Legal referrals are often handled through email, text messages, phone calls, spreadsheets, and informal notes. These methods are difficult to track, difficult to audit, and poorly connected to consent, fee-sharing rules, conflict checks, status updates, and referral outcomes.

CLEAR provides the structured data needed for participating professionals and systems to document, route, track, and audit referrals according to applicable rules.

CLEAR does not determine whether a referral is ethically permitted. It does not replace conflict-check procedures, engagement letter requirements, bar ethics rules, client consent obligations, attorney judgment, or jurisdiction-specific professional responsibility obligations.

---

## Non-Goals

CLEAR v0.1 does not:

- Define legal ethics rules or unauthorized-practice-of-law determinations.
- Replace attorney-client privilege analysis or conflict-of-interest screening.
- Certify referrals as ethically compliant.
- Define fee-sharing rates or validate fee-sharing arrangements against applicable rules.
- Guarantee referral outcomes or client engagement.
- Replace practice management, CRM, case management, or billing systems.

---

## Minimum Referral Workflow

A minimum CLEAR workflow consists of five stages:

1. **Referral created** — A referring participant creates a draft referral with matter context and prospect consent.
2. **Referral sent** — The referring participant sends the referral to a receiving participant or institution.
3. **Referral received** — The receiving system acknowledges receipt.
4. **Referral accepted or declined** — The receiving participant accepts or declines the matter.
5. **Referral status updated or closed** — Status is updated as the matter progresses, ending in a closed state.

A system implementing CLEAR Tier 1 MUST support all five stages. Optional features such as fee tracking, document exchange, conflict-check integration, settlement reporting, and impact aggregation MAY be added after the minimum workflow is stable.

---

## Referral Status Model

| Status | Meaning |
|--------|---------|
| `draft` | Referral has been created but not yet sent. |
| `sent` | Referral has been transmitted to the receiving participant or institution. |
| `received` | Receiving system has acknowledged receipt. |
| `under_review` | Receiving participant is reviewing the referral. |
| `accepted` | Referral has been accepted by the receiving participant. |
| `declined` | Referral has been declined. The sender MAY redirect to another recipient. |
| `conflict_check_pending` | Acceptance is pending completion of a conflict-of-interest review. |
| `client_contacted` | The receiving participant has contacted the prospect or client. |
| `engagement_pending` | An engagement letter or agreement is being prepared or reviewed. |
| `engaged` | The receiving professional has opened the matter and an engagement exists. |
| `closed_no_engagement` | Referral was closed without the prospect or client engaging. |
| `closed_referred_elsewhere` | Referral was redirected to another professional or institution. |
| `closed_completed` | Referral lifecycle completed. |
| `cancelled` | Referral was cancelled by the sender or an authorized participant. |

### Status Transition Rules

A referral SHOULD only move forward through these transitions. Status MUST NOT move backward except for corrections explicitly documented in an `AuditEvent`.

Typical forward path:

```
draft → sent → received → [under_review] → [conflict_check_pending] → accepted → [client_contacted] → [engagement_pending] → engaged → closed_completed
```

Termination paths:

```
sent / received / under_review / conflict_check_pending → declined → closed_referred_elsewhere
any_status → cancelled
engaged → closed_no_engagement
```

---

## Minimum Resource Set

### Required Resources

| Resource | Description |
|----------|-------------|
| `Referral` | Core referral record. |
| `ReferralParticipant` | Inline object within `Referral.participants`. |
| `Prospect` | Minimum data about the person or matter being referred. |
| `MatterSummary` | Inline summary of the legal matter, practice area, and jurisdiction. |
| `ConsentRecord` | Documented consent for data sharing. |
| `StatusUpdate` | Status change event with actor and timestamp. |
| `AuditEvent` | Audit log record of consequential actions. |

### Optional Resources

| Resource | Description |
|----------|-------------|
| `ReferralSource` | How the referring attorney learned about the referral. |
| `FeeArrangement` | Fee-sharing structure and rule basis. |
| `FeeSettlement` | Documented fee settlement after matter closure. |
| `DocumentReference` | Reference to a document shared as part of the referral. |
| `ConflictCheck` | Record of conflict-of-interest screening. |
| `EngagementRecord` | Record that engagement was established. |

---

## Data Minimization

CLEAR referral resources SHOULD contain the minimum data necessary for the exchange.

Sensitive prospect or client data SHOULD be referenced by ID rather than copied. Documents SHOULD be referenced through controlled links rather than embedded in the referral packet.

Practice-area classification, matter summary, and jurisdiction context are appropriate to include in a referral packet.

Full case files, prior legal history, financial records, and detailed personal information generally SHOULD NOT be included in the referral packet unless explicitly required and consented to.

---

## Consent Requirements

A CLEAR referral that involves prospect or client personal data MUST include or reference a `ConsentRecord` before that data is shared with a receiving participant.

Key consent requirements:

- Consent MUST be obtained before sharing prospect contact information, matter details, or uploaded documents.
- For fee-sharing arrangements, consent to the fee division MUST comply with applicable bar rules. OLE records the consent structure; compliance with the ethics rule is the attorney's responsibility.
- Consent records SHOULD include an expiration date.
- Consent revocation MUST be supported.

See the Core specification for the full `ConsentRecord` format.

---

## Referral Resource Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `resourceType` | string | **MUST** | Always `"Referral"`. |
| `id` | string | **MUST** | Stable referral identifier. |
| `oleVersion` | string | **MUST** | OLE version, e.g., `"0.1"`. |
| `module` | string | **MUST** | Always `"clear"`. |
| `referralType` | string | **MUST** | Type of referral, e.g., `attorney_to_attorney`. |
| `status` | string | **MUST** | Current status from the status model. |
| `createdAt` | string | **MUST** | ISO 8601 creation timestamp. |
| `participants` | array | **MUST** | Participants with roles. At least one sender and one receiver. |
| `matterSummary` | object | **MUST** | Practice area, jurisdiction, summary, urgency. |
| `visibility` | object | **MUST** | Visibility scope. Default: `participant_visible`. |
| `extensions` | object | **MUST** | Implementer extensions (may be empty `{}`). |
| `updatedAt` | string | RECOMMENDED | ISO 8601 last-updated timestamp. |
| `sentAt` | string | RECOMMENDED | ISO 8601 timestamp when referral was sent. |
| `referringInstitution` | string | RECOMMENDED | OLE Institution ID of the referring firm. |
| `receivingInstitution` | string | RECOMMENDED | OLE Institution ID of the receiving firm. |
| `feeArrangement` | object | OPTIONAL | Fee-sharing structure and consent reference. |
| `consentRecord` | string | RECOMMENDED | OLE ID of the associated `ConsentRecord`. |
| `audit` | object | RECOMMENDED | Reference to associated `AuditEvent` records. |
| `externalIds` | array | OPTIONAL | Identifiers from non-OLE systems. |

### Participant Roles

| Role | Description |
|------|-------------|
| `referring_attorney` | The attorney or legal professional sending the referral. |
| `receiving_attorney` | The attorney or legal professional receiving the referral. |
| `referring_institution` | Firm or organization sending the referral (if distinct from referring attorney). |
| `receiving_institution` | Firm or organization receiving the referral (if distinct from receiving attorney). |
| `prospect` | The individual or matter being referred. |
| `referral_coordinator` | Staff member coordinating the referral on behalf of a participant. |

### Referral Types

| Type | Description |
|------|-------------|
| `attorney_to_attorney` | One attorney refers a matter directly to another. |
| `firm_to_firm` | One firm transfers a matter to another firm. |
| `bar_to_attorney` | A bar referral service routes an inquiry to a member attorney. |
| `legal_aid_to_attorney` | A legal aid organization refers a client to a private attorney. |
| `referral_service_to_attorney` | A referral platform connects a prospect to an attorney. |

---

## Fee Arrangement Fields

| Field | Type | Description |
|-------|------|-------------|
| `feeSharingExpected` | boolean | Whether a fee-sharing arrangement is anticipated. |
| `ruleBasis` | array | OLE Rule IDs governing the fee arrangement (e.g., `ole_rule_fl_bar_4_1_5`). |
| `requiresWrittenConsent` | boolean | Whether written client consent is required by applicable rules. |
| `consentRecord` | string | OLE ID of the consent record documenting client consent to the fee division. |

Fee arrangement details are intentionally minimal in v0.1. Full fee modeling (rates, settlement, reconciliation) is deferred to a future CLEAR extension.

---

## API Surface

The following draft API surface defines the minimum endpoint set for a CLEAR Tier 1 implementation. Request and response schemas are defined in the OpenAPI draft at `openapi/ole-clear-v0.1.yaml`.

### Discovery

```text
GET /.well-known/ole
```

Returns the OLE discovery document declaring supported modules, OLE version, conformance profile, and technical contact.

### Referral Endpoints

```text
POST   /ole/v1/clear/referrals
GET    /ole/v1/clear/referrals/{id}
PATCH  /ole/v1/clear/referrals/{id}
POST   /ole/v1/clear/referrals/{id}/status-updates
```

### Webhooks

```text
POST   /ole/v1/webhooks
```

Webhook events SHOULD be signed using `OLE-Signature`, `OLE-Timestamp`, and `OLE-Event-Id` headers as defined in the Core specification.

### Minimum Webhook Events

| Event | Trigger |
|-------|---------|
| `referral.sent` | A referral is transmitted to a receiving system. |
| `referral.received` | A referral is acknowledged by the receiving system. |
| `referral.status_updated` | Referral status changes. |
| `referral.accepted` | Referral is accepted. |
| `referral.declined` | Referral is declined. |
| `referral.closed` | Referral reaches a closed status. |

---

## Security Requirements

- All CLEAR endpoints MUST use HTTPS.
- Endpoints MUST require authentication for all mutation operations.
- Bearer token authentication is RECOMMENDED as the baseline.
- Webhook deliveries MUST be signed.
- Idempotency keys SHOULD be supported for all mutation operations.
- Audit logging is REQUIRED for `sent`, `received`, `accepted`, `declined`, and `closed` events.

---

## Not Yet Defined in v0.1

The following are planned for future CLEAR specifications:

- Normative JSON Schemas for all CLEAR resources
- Fee settlement resource and workflow
- Document exchange protocol
- Conflict-check integration protocol
- Engagement record format
- Bar association reporting format
- CLEAR conformance test suite
- CLEAR sandbox environment
- Impact aggregation from CLEAR data into CIVIC module
