# CLEAR Tier 1 Conformance Checklist — Draft

**Status:** Draft — self-declared conformance only  
**Module:** CLEAR — Legal Referral Exchange  
**Conformance profile:** `ole-clear-exchange-draft-v0.1`  
**Published by:** Open Legal Exchange (openlegalexchange.org)  
**Date:** May 2026

---

## Important Caution

This checklist describes self-declared conformance. No formal certification process exists in OLE v0.1.

A system operator may work through this checklist and declare `conformanceStatus: "self_declared"` in their OLE discovery document. Third-party validation and formal certification are not available at this stage.

Do not use `conformanceStatus: "certified"` — no certification program exists yet.

---

## How to Use This Checklist

Work through each section. Check each item your implementation supports. A minimum Tier 1 self-declaration requires passing all **Required** items. Recommended items improve interoperability but are not required for a minimum self-declaration.

---

## 1. Discovery

| # | Item | Level |
|---|------|-------|
| 1.1 | Publish `GET /.well-known/ole` returning a valid OLE discovery document. | Required |
| 1.2 | Declare `oleVersion` in the discovery document. | Required |
| 1.3 | Declare supported modules (e.g., `["clear"]`) in the discovery document. | Required |
| 1.4 | Declare `conformanceProfile: "ole-clear-exchange-draft-v0.1"` in the discovery document. | Required |
| 1.5 | Declare `conformanceStatus: "self_declared"` (or `"draft"`) in the discovery document. | Required |
| 1.6 | Provide a `technicalContact` (email or URL) in the discovery document. | Recommended |
| 1.7 | Provide `apiBaseUrl` in the discovery document. | Recommended |

---

## 2. Referral Lifecycle

A conforming system MUST support all five stages of the minimum referral workflow.

| # | Item | Level |
|---|------|-------|
| 2.1 | Create a referral (`POST /ole/v1/clear/referrals`) with status `draft`. | Required |
| 2.2 | Send a referral (status transition to `sent`). | Required |
| 2.3 | Receive a referral and acknowledge receipt (status transition to `received`). | Required |
| 2.4 | Accept a referral (status transition to `accepted`). | Required |
| 2.5 | Decline a referral (status transition to `declined`). | Required |
| 2.6 | Add a status update (`POST /ole/v1/clear/referrals/{id}/status-updates`). | Required |
| 2.7 | Close a referral (transition to any `closed_*` status or `cancelled`). | Required |
| 2.8 | Retrieve a referral by ID (`GET /ole/v1/clear/referrals/{id}`). | Required |
| 2.9 | Support `conflict_check_pending` status. | Recommended |
| 2.10 | Support `client_contacted`, `engagement_pending`, and `engaged` statuses. | Recommended |

---

## 3. Required Resources

| # | Item | Level |
|---|------|-------|
| 3.1 | `Referral` resource with all required envelope fields. | Required |
| 3.2 | `participants` array with at least one referring and one receiving participant. | Required |
| 3.3 | `matterSummary` with `practiceArea` and `jurisdiction`. | Required |
| 3.4 | `Prospect` or subject reference (by ID, not necessarily full PII). | Required |
| 3.5 | `ConsentRecord` linked from referral when prospect data is shared. | Required |
| 3.6 | `StatusUpdate` for every status transition. | Required |
| 3.7 | `AuditEvent` for `sent`, `received`, `accepted`, `declined`, and `closed` events. | Required |
| 3.8 | `visibility` field on every resource, defaulting to `participant_visible` for referrals. | Required |
| 3.9 | `extensions` field on every resource (may be empty `{}`). | Required |
| 3.10 | `externalIds` support (may be empty array). | Recommended |

---

## 4. Security and Reliability

| # | Item | Level |
|---|------|-------|
| 4.1 | All CLEAR API endpoints use HTTPS (TLS 1.2 or later). | Required |
| 4.2 | Mutation endpoints require authentication (bearer token or equivalent). | Required |
| 4.3 | Discovery endpoint (`/.well-known/ole`) is accessible without authentication. | Required |
| 4.4 | Webhook deliveries are signed using `OLE-Signature`, `OLE-Timestamp`, and `OLE-Event-Id` headers. | Recommended |
| 4.5 | Receiving system verifies webhook signatures before processing. | Recommended |
| 4.6 | `Idempotency-Key` header is supported for `POST` operations. | Recommended |
| 4.7 | Structured error responses use the OLE error envelope format. | Recommended |
| 4.8 | Stale or duplicate webhook events are rejected. | Recommended |
| 4.9 | Rate limiting is in place for API endpoints. | Recommended |

---

## 5. Audit Logging

| # | Item | Level |
|---|------|-------|
| 5.1 | Audit events are recorded for `referral.sent`. | Required |
| 5.2 | Audit events are recorded for `referral.received`. | Required |
| 5.3 | Audit events are recorded for `referral.accepted` and `referral.declined`. | Required |
| 5.4 | Audit events are recorded for all `closed_*` and `cancelled` transitions. | Required |
| 5.5 | Audit events are recorded for `consent.granted`. | Required |
| 5.6 | Audit events are recorded for document access events. | Recommended |
| 5.7 | AI agent actions include `logInputs`, `logOutputs`, `logDecisionRationale`, `logModelVersion`, and `logPolicyVersion` in audit events. | Required (if AI agents are used) |
| 5.8 | Audit records are retained for a defined period appropriate to applicable rules. | Recommended |

---

## 6. Privacy and Data Minimization

| # | Item | Level |
|---|------|-------|
| 6.1 | Prospect data is minimized: only include data necessary for the referral exchange. | Required |
| 6.2 | Consent is obtained before prospect personal data is shared with receiving participants. | Required |
| 6.3 | `ConsentRecord` includes `purpose`, `dataScopes`, `grantedTo`, and `expiresAt`. | Required |
| 6.4 | Consent revocation is supported and reflected in `ConsentRecord.status`. | Recommended |
| 6.5 | `visibility` scope defaults to `participant_visible` for referral resources. | Required |
| 6.6 | Impact data, if published, uses `aggregate_public` scope and contains no PII. | Required (if impact data is published) |
| 6.7 | Impact aggregation threshold of at least 10 records before publication. | Required (if impact data is published) |

---

## 7. Conformance Declaration

A system that passes all Required items above may declare:

```json
{
  "oleVersion": "0.1",
  "modules": ["clear"],
  "conformanceProfile": "ole-clear-exchange-draft-v0.1",
  "conformanceStatus": "self_declared"
}
```

This declaration does not represent certification, third-party validation, legal ethics compliance, or production readiness. It indicates that the system operator has reviewed this checklist and believes their implementation satisfies the required items.

---

## Items Not in Scope for Tier 1

The following are out of scope for a minimum Tier 1 self-declaration and may be addressed in future checklist versions:

- Fee settlement workflow
- Document exchange protocol
- Conflict-check integration
- Bar association reporting
- CIVIC impact aggregation
- Cross-platform referral exchange with systems outside your network
- Verifiable Credentials integration
