# Security Test Plan: DocNow API

Based on the OWASP API Security Top 10. All tests run only in my own isolated Docker lab environment.

## Threat Model
Diagram: [threat-model.drawio](threat-model.drawio)

Data flow: Client -> API Gateway -> Service -> DB

| Attack surface | Main risks |
|---|---|
| Authentication | Token theft, weak JWT, brute force |
| Authorization | BOLA, BFLA, mass assignment |
| Data exposure | Excessive data in responses |
| Rate limits | DoS, brute force |
| Input handling | SQL injection |
| Configuration | Stack traces, CORS, missing headers |

## Test Cases

| # | OWASP Risk | Test case | Tool | Expected outcome / Success criteria |
|---|---|---|---|---|
| 1 | API1 BOLA | Use User A's token to access /documents/{User B's id} | curl, Postman | 403/404, no data returned |
| 2 | API2 Broken Authentication | Send no token, expired token, tampered JWT | curl, jwt.io | All requests return 401 |
| 3 | API2 Broken Authentication | Try 20+ wrong passwords on login | Burp Intruder, OWASP ZAP | Account lockout or 429 |
| 4 | API3 Data Exposure | Inspect responses for extra fields (password hash, internal IDs) | curl, OWASP ZAP | Only necessary fields returned |
| 5 | API4 Rate Limiting | Send 100 requests in 10 seconds to one endpoint | OWASP ZAP, hey | 429 returned, server stays stable |
| 6 | API5 BFLA | Call an admin endpoint as a normal user | Postman | 403 Forbidden |
| 7 | API8 Injection | Put a SQL injection payload in a query parameter | OWASP ZAP | 400 or sanitized, no DB error shown |
| 8 | API3 Mass Assignment | Add "role":"admin" to an update request | curl | Extra field ignored |
| 9 | API8 Misconfiguration | Check error messages, stack traces, CORS, security headers | OWASP ZAP, curl -I | No stack traces, correct headers |
| 10 | API9 Improper Inventory | Look for old /v1/ and /debug endpoints | OWASP ZAP spider | Unused endpoints disabled |

## Limitations
- This is a plan; the tests have not been executed yet.
- API7 and API10 are not covered in detail.
