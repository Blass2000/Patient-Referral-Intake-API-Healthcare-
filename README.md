# Referral Intake API – Care Coordination Platform

The **Referral Intake API** enables external healthcare partners to securely submit patient referrals into the Care Coordination platform. Referrals are validated, stored, processed for eligibility, synchronized with downstream clinical systems, and made available for analytics.

This repository includes the OpenAPI specification, sample payloads, data-flow documentation, and onboarding instructions for developers integrating with the API.

---

## ✅ Key Capabilities

- Submit patient referrals via **idempotent POST** endpoint  
- Retrieve referral details, current status, and event history  
- Receive **webhook notifications** when referral statuses change  
- PHI masking via `?mask=true` and role-based scopes  
- Designed for scale: rate limits, async processing, webhook retries, audit logs  
- HIPAA-aligned security using OAuth2, mTLS, and encrypted data storage

---

## 🔌 Endpoints (Summary)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/v1/referrals` | Create a new referral (idempotent) |
| `GET`  | `/v1/referrals/{referralId}` | Retrieve referral details & status |
| `GET`  | `/v1/referrals` | List referrals with filtering + cursor pagination |

All requests require:
