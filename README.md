# Task 3 — Web Application Security Testing (DVWA)

## Overview
This task documents a small web-security practice session using Damn Vulnerable Web Application (DVWA) in a local lab. The screenshots show the application home page, Stored XSS guestbook, File Inclusion pages, a CSRF password-change result, and a basic HTTP response check.

All testing shown here was performed against a lab instance. DVWA is intentionally vulnerable and should only be run locally or in an isolated training environment.

## Lab setup
- Platform: Kali Linux
- Target application: DVWA
- Browser: Firefox
- Local URL used in browser: `http://127.0.0.1/DVWA/`
- HTTP check target shown in terminal: `http://10.0.3.15/`

> Note: The browser screenshots use `127.0.0.1`, while the terminal check uses `10.0.3.15`. These may represent different interfaces/instances, so confirm the target address before repeating the test.

## Work performed

### 1. DVWA application review
Opened the DVWA home page and reviewed the available practice modules.

### 2. Stored Cross-Site Scripting (XSS)
Used the Stored XSS guestbook form to add test entries. One entry contains a script-like string for demonstration. This shows why applications should validate input and encode user-supplied content before displaying it.

**Recommended mitigation**
- Encode output according to its context (HTML, attribute, JavaScript, etc.).
- Validate input on the server.
- Use a Content Security Policy as an additional layer, not as a replacement for output encoding.

### 3. File Inclusion
Opened the File Inclusion module and viewed a sample page. The page also displays a warning that `allow_url_include` is disabled in the installed PHP version, so remote-file inclusion behavior is not enabled in this setup.

**Recommended mitigation**
- Do not use raw user input as a file path.
- Map user choices to a fixed allowlist of permitted files.
- Keep PHP and dependencies updated; avoid enabling remote URL inclusion.

### 4. Cross-Site Request Forgery (CSRF)
The screenshot shows the DVWA password-change form returning “Password Changed.” This is a lab demonstration of a state-changing action. In a real application, the request should be protected against unwanted cross-site submissions.

**Recommended mitigation**
- Use unpredictable CSRF tokens and validate them server-side.
- Set cookies with appropriate `SameSite`, `Secure`, and `HttpOnly` attributes where applicable.
- Require re-authentication for sensitive account changes when appropriate.

### 5. Basic HTTP response/header check
A `curl` request was used to inspect the HTTP response. The screenshot shows an initial `302 Found` response for `/DVWA/` and a `200 OK` response for `/`. The displayed command filters for common security-header names; the screenshot does not show any matching header lines, so no conclusion about the full server configuration should be drawn from this output alone.

Command used:
```bash
curl -sIL http://10.0.3.15/ | grep -Ei 'HTTP/|X-Frame|Content-Security|X-Content-Type|Strict-Transport|Referrer-Policy'
```

The check can be repeated against the correct lab URL. To view all response headers:
```bash
curl -sI http://10.0.3.15/
```

## Findings summary

| Area | Observation | Suggested improvement |
|---|---|---|
| Stored XSS | Test content can be submitted to the guestbook | Context-aware output encoding and server-side validation |
| File Inclusion | Local sample page opens; remote URL inclusion is disabled in this PHP setup | Use an allowlist for page selection and keep PHP configuration hardened |
| CSRF | Password-change action reports success in the lab | Add and validate CSRF tokens for state-changing requests |
| HTTP headers | Response status codes were visible; the filtered output did not show matching header lines | Review the complete response and configure suitable headers such as CSP, frame protections, MIME-sniffing protection, and Referrer-Policy |

## Evidence
Screenshots are stored in the [`screenshots/`](screenshots/) folder:
1. `01_DVWA_Home.png`
2. `02_Stored_XSS_Guestbook.png`
3. `03_File_Inclusion_File1.png`
4. `04_File_Inclusion_Configuration.png`
5. `05_CSRF_Password_Change.png`
6. `06_HTTP_Security_Header_Check.png`

## Conclusion
This exercise helped me review common web-application security issues in a controlled DVWA environment and connect each issue with practical defensive measures. The screenshots are evidence of the pages and checks shown during the lab; they do not represent a full penetration test or a complete security audit.

## Scope and safety
DVWA is deliberately vulnerable. Keep it on localhost or an isolated lab network, do not expose it to the public internet, and test only systems for which you have permission.
