# OWASP Juice Shop - IDOR Vulnerability Assessment

Web application security assessment identifying and demonstrating an
Insecure Direct Object Reference (IDOR) vulnerability in the basket
retrieval endpoint of OWASP Juice Shop, performed against a self-hosted
lab instance.

## Overview

The basket retrieval endpoint (`GET /rest/basket/:id`) returns basket
contents based solely on the numeric ID supplied in the request URL,
without verifying that the requesting user actually owns that basket.
Any authenticated user can substitute another user's basket ID and
retrieve their private basket contents.

- Severity: Medium (CVSS 3.1: 4.3 - AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:N/A:N)
- Category: Broken Access Control / IDOR (OWASP Top 10:2025 - A01)
- Scope: Read-only access was confirmed. Write access (basket
  modification, deletion, checkout manipulation) was not tested and
  is not claimed.

## Environment

- Target: OWASP Juice Shop, self-hosted via Docker (local instance only)
- Tools: Burp Suite Community Edition, Kali Linux, Oracle VirtualBox
- Method: Two standard user accounts, controlled two-account
  authorization testing with the basket ID as the only manipulated
  variable

## Repository structure

```
report/         Full vulnerability assessment report (PDF)
screenshots/    Lab setup and browser/Burp evidence, in testing order
evidence/       Plain-text request/response write-ups for the baseline
                and exploit requests, with authentication tokens redacted
```

## Key finding

A request authenticated as one user, sent to another user's basket ID,
returned that user's basket data with an HTTP 200 OK response instead
of being rejected. This confirms the endpoint performs no server-side
ownership check between the authenticated user and the requested
resource. See `evidence/03-idor-exploit.txt` for the full request and
response, and the report for complete methodology, impact analysis,
and remediation recommendations.

## Report

The full report, including proof of concept, risk rating, and
remediation guidance, is available in [`report/`](./report).

## Disclaimer

This assessment was performed exclusively against a locally hosted,
intentionally vulnerable lab instance for educational purposes. No
external, production, or third-party systems were accessed or tested.
