# BF TKT Documentation

**Version:** v1.0 | **Updated:** 2026-03-26 | **Contact:** admin@billfree.in

---

## Endpoint

### Recommended (Direct 200 OK Response)
Use this endpoint to avoid handling HTTP `302 Moved Temporarily` redirects on your client:
```
POST https://billfreetech.pages.dev/api/ticket
```

### Direct Google Apps Script Endpoint (Returns 302 Redirect)
```
POST https://script.google.com/macros/s/AKfycbwJcHg5ToptJlv2OV4r3eCdOnmtzh0HC-ahvBmriI5OsnNo1eB5_PxuZGrli83Fz0s6Mw/exec
```

**Content-Type:** `application/json`

> **🔔** This API returns the result **synchronously** in the response body. No webhooks.

---

## Authentication

Every request must include an `apiKey` field. Keys are issued by BillFree.

---

## Request

```json
{
  "action": "createTicket",
  "apiKey": "bf_wa_live_9xK3pQ7Z",
  "requestedBy": "Customer Name",
  "phone": "9876543210",
  "concern": "POS machine not responding",
  "mid": "123456",
  "business": "ABC Store"
  "pos": "tally"
}
```

| Field | Required | Description |
|---|---|---|
| `action` | ✅ | Must be `"createTicket"` |
| `apiKey` | ✅ | API key provided by BillFree |
| `concern` | ✅ | Issue description |
| `mid` | ✅ | Merchant ID — numbers only |
| `business` | ✅ | Business name |
| `phone` | Recommended | Customer phone number |
| `requestedBy` | Optional | Customer name (default: `"WhatsApp User"`) |
| `pos` | Optional | POS |

---

## Response

### Success
```json
{
  "success": true,
  "data": {
    "ticketId": "BF-TKT-2026-03-2526",
    "assignedAgent": "Agent Name",
    "status": "Not Completed",
    "requestId": "req_abc123def45"
  }
}
```

### Error
```json
{
  "success": false,
  "error": "Human-readable error message",
  "code": "E004",
  "requestId": "req_abc123def45"
}
```

| Code | Meaning | Retry? |
|---|---|---|
| `E001` | Rate limit / duplicate request | Wait 60s |
| `E002` | Invalid API key | Fix key |
| `E004` | Validation error (missing concern, non-numeric MID) | Fix request |
| `E006` | Server busy | Retry after 5s |
| `E999` | Internal error | Contact BillFree |

---

## WhatsApp Response Mapping

**Success:**
> *"Your ticket {{ticketId}} has been created. Our agent {{assignedAgent}} will contact you shortly."*

**Failure:**
> *"Unable to create ticket. Please try again."*

---

## Rate Limits

- **60 requests/minute** per API key
- **Duplicate protection:** same phone + concern blocked for 5 minutes

---

## Examples

### cURL
```bash
curl -L -X POST \
  "https://script.google.com/macros/s/AKfycbwJcHg5ToptJlv2OV4r3eCdOnmtzh0HC-ahvBmriI5OsnNo1eB5_PxuZGrli83Fz0s6Mw/exec" \
  -H "Content-Type: application/json" \
  -d '{"action":"createTicket","apiKey":"bf_wa_live_9xK3pQ7Z","phone":"9876543210","concern":"POS not working","mid":"123456","business":"ABC Store"}'
```

### Python
```python
import requests

response = requests.post(
    "https://script.google.com/macros/s/AKfycbwJcHg5ToptJlv2OV4r3eCdOnmtzh0HC-ahvBmriI5OsnNo1eB5_PxuZGrli83Fz0s6Mw/exec",
    json={
        "action": "createTicket",
        "apiKey": "bf_wa_live_9xK3pQ7Z",
        "phone": "9876543210",
        "concern": "POS not working",
        "mid": "123456"
    },
    allow_redirects=True
)
print(response.json())
```

### JavaScript (Node.js)
```javascript
const response = await fetch(
  "https://script.google.com/macros/s/AKfycbwJcHg5ToptJlv2OV4r3eCdOnmtzh0HC-ahvBmriI5OsnNo1eB5_PxuZGrli83Fz0s6Mw/exec",
  {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      action: "createTicket",
      apiKey: "bf_wa_live_9xK3pQ7Z",
      phone: "9876543210",
      concern: "POS not working",
      mid: "123456"
    }),
    redirect: "follow"
  }
);
console.log(await response.json());
```

---

## Notes

1. **Follow redirects** — GAS redirects once (`-L` in cURL, `allow_redirects=True` in Python, `redirect: "follow"` in fetch)
2. **Ticket IDs are globally sequential** — sequence never resets, only the `YYYY-MM` label changes
3. **Agent assignment is automatic** — least-busy agent is assigned
4. **All times are IST** (UTC+5:30)
5. **Save `requestId`** — include it when reporting issues to BillFree
