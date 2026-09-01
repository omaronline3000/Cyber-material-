
# Authentication & Authorization & Access Control Types


## [ - ] Authentication

- It's about identifying who are you ?
- Types (Famous) :
	- `Password-Based Authentication`
	- `2FA (Two-Factor Authentication)`
		- `OTP (One-Time Password)`
	- `Biometric Authentication`
	- `MFA (Multi-Factor Authentication)`
	- `OAuth (Open Authorization)`
	- `Token-Based Authentication` 

## [ - ] Authorization

- after authenticating you, now we should know what do you should do and what you should not ?, it's about your permissions and access areas
- Types (for example) :
	- `Role-Based Authorization`
	- `Object-Based Authorization` 
	- `Attribute-Based Authorization`
	- `Time-Based Authorization`
	- `Location-Based Authorization`


## [ - ] Access Control Types

- `Vertical Access Control`
- `Horizontal Access Control`
- `Context-Based Access Control`
	- `nearest to Logic Bugs`

---

# Access Control Attacks

- Vertical Privilege Escalation mean that you found a broken Vertical Access Control
- Horizontal Privilege Escalation mean that you found a broken Horizontal Access Control
- mindset in testing, always search for No's.

## Privilege Escalation maybe [ Attack Vector ] 
## and maybe [ Vulnerability ]

- in case if the attacker found a vulnerability gave him the ability to do only one or two functions of admin functions, that mean he found a vulnerability let us say `Authorization bypass or Imporoper Access Control` and then the `Privelge Escalation` is an attack vector here

- but in case if the attacker found a vulnerability gave him a full admin account meaning he can do all actions of the admin, that's mean the vulnerability here is `Privelege Esclation`

- So you can reach Privilege escalation by Access Control Vulnerabilities, like
	- `IDOR` (Insecure Direct Object Reference)
	- Broken Access Control
- many names for it in write-ups 
	- `Priv Esc`
	- Authorization bypass
	- Improper Access Control
	- Logic Bugs
- question like from where the low privilege user will get the requests, that's may  happened by several scenarios, like:
	- the user was admin in before
	- the user created an account for organization on the website and understand how the flow and saved the requests

## IDOR (Insecure Direct Control Reference)

- which happening when you request the website to retrieve some (files , data, documents), and the server-side does not validate from the inputs (who is sent this requests and is this data belongs to him)

- ### Testing techniques:
	- Trying to find any numerical parameters which can be in `HTTP request` or in `API`  and tamper it to access another user data, you can find the parameters outright, or :
		- Encoded (`based64x` for example)
			- so you decode it and tamper its value
		- Hashed
			- so you trying to crack this hash and tamper the value
		- unpredictable
			- which mean you did not identify it as encoded value or hashed value so you tested it by `2 accouts technique` you create two accounts and swap the IDs, and try it in the request while you logged in and while not
			- you can trying to understand its logic, or its pattern and brute force it
			- one of the common name is `GUID`
			- theses parameter values for the other users may be leaked in elsewhere in the app, like messages, reviews or posts


- ### where to find it
	- it's not just exist in endpoints in the HTTP requests, you may  find it in:
		- API requests
		- AJAX requests
		- trying `parameter mining` which is finding parameter not called directly in the request, exist in another page, unused but forgot from the development phase and the app got production with it or referenced in JS files, trying it in the current request
			- `burpsuite extension:` `paramminer`
	- it not just parameters you can find it in some headers and maybe cookies and maybe in the body 

- ### Scenarios to test
	- Try to delete any authorization like `Authorization header or Cookies header` 
	- Try to change the method type of the request even it was not exist in the `UI` like `Delete`
	- in functionalities like changing email try to change the email to another one, and even it put by the website, intercept the request and change it.
	- sometimes while you tamper the parameter it redirects you to another page like `main page` , but in the response it leak sensitive data of the user may lead to `ATO`
	- 

## Generic Broken Access Control


- ### Examples of Vertical Broken Access Control
	- see if there is endpoints for admin in the `URLs` which does not require any privilege to access it like `target.tld/admin`

	-  Sometimes it used technique called `security by obsecurity` , which mean you giving an unpredictable value to something you wanna hide it to protect it from guessing
		- but sometimes the developer make the logic in `JS` code and mention the URL in the source code not in the `UI`

	- **Parameter-based access control**
		- the developer determine the access control rights or the authorization of the user while the authentication phase and save it in some user-controllable location `might be :- hidden file, cookie, preset query parameter` and the back-end manipulate with request depend on this value without any validation
	- **Method-based and URL-based `misconfiguration` at platform layer**
		- and that mean some times the developer rely on some rules and configs in  frameworks, proxy, API gateway, routing middle war or `WAF`
		- so that mean the back-end code `the logic` is not secure, the security handled at platform layer
		- that may bypassed by many ways, and for example if you use a framework support some non-standard HTTP headers like `X-Original-URL` or `X-rewrite-URL` , you can ask any allowable endpoint and then all platform layers security will pass it to logic, but when the logic resolve the headers it will change the endpoint to the sensitive one, and due to there is any security restrictions in the logic you will access it
	- **URL-matching discrepancies** 
		- some websites mapped this endpoint `/admin/delete` to this `/ADMIN/DELETE`, so if the access control on only one of them so that's mean I can access the other one and I will access it
		- at `Spring framework`, if the developer enabled `useSuffixPatternMatch`
			-  that's let file extensions in the endpoints mapped to the same endpoint without the extension, like `/admin/delete.anything` mapped to `/admin/delete`
			- Prior to Spring 5.3, this option is enabled by default.
		- and some systems consider `/admin/delete/` distinct than `/admin/delete` so you can also bypass the security and get your goal
	- 
- ### Examples of Horizontal Broken Access Control
	- `IDOR` vulnerability

- ### Horizontal to Vertical Broken Access Control
	- like when you access via `horizontal broken access control` to admin data and steal his account and login as admin so it became `Vertical broken access control`

- ### Access control vulnerabilities in multi-step processes
	- when some functions require more than one step (more than one request) to achieved 
	- sometimes the developer just secure the first step and maybe the second also, and let the other without security
		- so if the hacker skipped the first two requests and used the third one, so he bypassed it
		- like if the third one is confirmation, so you confirmed the function, without asking for it, but actually it confirmed
- ### Referer-based access control
	- which mean the request checked if it `referered` from specific endpoint like when requesting adding users endpoint, it just check if the `refered` page is the admin panel
		- so the attacker can easily tamper the `Referer` header
- ### Location-based access control
	- which can be bypassed by VPNs and Proxies or even manipulating with geographical settings in the client side


---

# Scenarios to test
- searching in `robots.txt` 
	- for every subdomain look for `robots.txt` file
- when you find a potential request for a vulnerability, delete any `unrequired` headers to simplify the request and reduce the nosy
- [Ideas from here](https://github.com/Az0x7/vulnerability-Checklist/tree/main)
