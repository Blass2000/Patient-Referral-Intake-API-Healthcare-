Endpoint Summary

Create referral (idempotent)

Method/Path: POST /v1/referrals

Auth: OAuth2 client-credentials + mTLS

Headers:

Authorization: Bearer <token>

Idempotency-Key: <uuid> (required)

Content-Type: application/json

Sync/Async: Synchronous validation; referral creation is queued for async eligibility processing. Returns 202 Accepted.

Get referral by ID

Method/Path: GET /v1/referrals/{referralId}

Caching: ETag/If-None-Match supported

PII Masking: ?mask=true redacts PHI for non-privileged scopes

List referrals (paged)

Method/Path: GET /v1/referrals?status=pending&created_since=2025-10-01&limit=50&cursor=...

Pagination: cursor-based (stable under concurrent writes)

Outbound webhook (status change)

Callback URL (partner-registered): POST https://partner.example.com/webhooks/referrals.status

Signing: X-Signature (HMAC-SHA256)
