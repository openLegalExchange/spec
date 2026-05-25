# RFC 0001 — CLEAR Minimum Referral Workflow

## Problem

Legal referrals are often exchanged through unstructured channels such as email, text messages, phone calls, and informal notes. These channels make it difficult to track referral status, consent, fee-sharing requirements, conflict checks, and outcomes.

## Proposal

Define a minimum CLEAR referral workflow:

1. Referral created
2. Referral sent
3. Referral received
4. Referral accepted or declined
5. Referral status updated or closed

## Affected module

- CLEAR — Legal Referral Exchange

## Backward compatibility

This is the first draft workflow. No backward compatibility issue exists yet.

## Privacy and security

Prospect data should be minimized. ConsentRecord support should be included from the first implementation. Visibility scope should default to `participant_visible`.

## Reference implementation impact

CertusGate can serve as the first self-declared reference implementation by supporting the workflow and publishing example OLE packets.
