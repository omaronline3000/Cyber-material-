- Searching for any Input field, that reflect in HTTP response, like:
	- when you searching fro specific thing, the response in `Set-Cookie:`and inject `CRLF` `\r\n == %0d%0a` then inject anything in the response, like:
		- `\r\nSet-Cookie: csrfKey: xxxxxxxxxxxxx`
- that's consider a stand-alone vulnerability, but it also very useful to in attack chaining
- do not forget to inject after it `SameSite` attribute and assign it to None, to be able to use it in any malicious 
- ==Cookie Tossing==, when you inject a cookie which its name same as another existing one, which one will sent and with which order ?  :-
	- **Sending**
		- If the new Cookie has the same `Domain + Path(End Point)` the malicious cookie will overwrite the original one `Best-Case`
		- If there is any difference between the original one and the malicious in anything of these `Domain + Path(End Point)`, then the both will sent
	- **Ordering**
		- the browser will order them in the sending according to `mroe specific path`
	- **Parsing** 
		- there is no default behavior it various depend on the Back-end Technology 

| **Backend / Technology**  | **Behavior**                                     | **Result for Attacker**                       |
| ------------------------- | ------------------------------------------------ | --------------------------------------------- |
| **PHP / Apache**          | Usually parses the **LAST** value.               | ✅ **Vulnerable** (if injected cookie is last) |
| **Python (Django/Flask)** | Usually parses the **LAST** value.               | ✅ **Vulnerable**                              |
| **Java (Tomcat/Jetty)**   | Usually parses the **FIRST** value.              | ❌ **Fail** (unless you can force yours first) |
| **ASP.NET**               | often combines them or takes **FIRST**.          | ⚠️ **Unpredictable** (might error out)        |
| **Node.js**               | Varies by framework (Express often takes First). | ⚠️ **Unpredictable**                          |
- 






# Exploitation

- Exactly like any Injection vulnerability  
- If it was in `GET` request, so you can just send to the victim a link
	- or you can host the link in another website
- If It POST-Based, it's harder to exploit exactly like `POST-Based XSS`
- Trick in case of Cookie Tossing:
	- if you wanna to enforce the browser to give your injected malicious cookie the priority in the sending order try to add to the cookie `PATH= ` and give it more specific path than the currently browser's prioritized one 
- 



