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
Authorization: Bearer <token>
Content-Type: application/json
Idempotency-Key: <uuid> # POST only


---

## 📥 Create Referral (Request Example)

```json
{
  "referralId": "EXT-REF-893452",
  "partnerClinicId": "CLINIC-1039",
  "patient": {
    "mrn": "A1234567",
    "firstName": "Jamie",
    "lastName": "Nguyen",
    "dob": "1986-04-03"
  },
  "referral": {
    "type": "DENTURE_FIT",
    "priority": "ROUTINE",
    "diagnosisCodes": ["K08.1"]
  },
  "insurance": {
    "payerId": "HUMANA",
    "memberId": "HUM123456"
  },
  "source": {
    "system": "PartnerCareSoft",
    "submittedAt": "2025-11-02T16:42:19Z"
  }
}

## 📥 Response (202 Accepted)

{
  "referralId": "RFL-7f1a7f83",
  "status": "PENDING_ELIGIBILITY",
  "receivedAt": "2025-11-02T16:42:21Z",
  "links": {
    "self": "/v1/referrals/RFL-7f1a7f83",
    "events": "/v1/referrals/RFL-7f1a7f83/events"
  }
}

If the same Idempotency-Key is sent again, the original response is returned — preventing duplicate referrals.

{
  "event": "ELIGIBILITY_PASSED",
  "referralId": "RFL-7f1a7f83",
  "updatedAt": "2025-11-02T16:44:10Z",
  "signature": "HMAC-SHA256-hex-value"
}

Webhook deliveries include signing headers and are retried with exponential backoff on failure.

Standard Error Model

{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "diagnosisCodes[0] is not a recognized ICD-10 code",
    "correlationId": "c9b21a4f-9e67-4d8c-9f50-63f1a6d2d2b3"
  }
}


