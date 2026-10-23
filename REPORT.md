# Task 3 — Web Application Security Testing (DVWA)

**Student:** Roshan Kumar  
**Environment:** Kali Linux, Firefox, DVWA  
**Scope:** Local/controlled training environment

## Objective
To explore selected web-application security weaknesses in DVWA and document practical defensive recommendations.

## Tools
- DVWA
- Firefox
- `curl` for a basic HTTP response/header check

## Results
- **Stored XSS:** Test entries were added to the guestbook, including a script-like test string. Applications should encode untrusted content before displaying it.
- **File Inclusion:** A sample File Inclusion page was opened. The application reported that `allow_url_include` was disabled, so remote URL inclusion was not enabled in this setup.
- **CSRF:** The password-change form displayed “Password Changed.” This illustrates why state-changing requests need server-side CSRF protection.
- **HTTP response check:** The terminal showed `302 Found` for `/DVWA/` and `200 OK` for `/`. The filtered output did not show matching security-header lines; a full header review is needed before drawing a final conclusion.

## Defensive recommendations
1. Use context-aware output encoding and server-side input validation.
2. Use a fixed allowlist for page/file selection; do not pass raw user input to file inclusion functions.
3. Protect state-changing actions with validated CSRF tokens and appropriate cookie settings.
4. Review and configure suitable HTTP security headers, including Content-Security-Policy, frame protection, `X-Content-Type-Options`, and `Referrer-Policy`.
5. Keep DVWA isolated because it is intentionally vulnerable.

## Evidence
The `screenshots/` folder contains six screenshots from the lab session. The README includes the command used and a short description of each screenshot.

## Conclusion
The lab provided hands-on exposure to selected web-security concepts and their mitigations. This is a learning exercise, not a full security audit.
