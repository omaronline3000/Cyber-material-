- `CORS` it's amazing feature which allow for origin to not just sending a request but also reading a response from the server and retrieving data
- but the `Misconfiguration` in it allow Origins maybe controlled by malicious user to read and retrieve the data 

# Main Idea
- the Fault of the Developer maybe in `Regex` for Example he putted `^api.example.com$` not`^api\.example\.com$` , the normal dot in the Regex mean any character so if I rent a domain with name like `apix.example.com` that's consider allowed for the previous wrong Regex
	- it often use the Regex to create whitelist of Origin that allowed to read from this Origin 
- Simple Request Case
	-  the victim enter a malicious website of the attacker and run specific script, which send request to retrieve a data for example the data in the `/profile`, so the request will sent by the cookies of the user,then:
		- the request included `Origin: origin` which a header that contains the Origin of the current website, and then if the response contained `Access-Control-Allow-Origin: origin` , if it allowed to it so that's mean there is a `misconfiguration` in the CORS allowed me to read response, and if it contained `Access-Control-Allow-Credentials: true` that's mean the cookies included, and that's mean not just i can read response but also the request sent using the cookies, and that's mean I can read all personal info of the user from this `Endpoint`
		- and then the response will back to the attacker website
- **Intranet Pivoting**: one of the most dangerous consequences of the `CORS misconfiguration`
	- which mean if there is a website in the private network in a company, which has a firewall that has whitelist of `IPs` or geographical restrictions or anything like that, then it block the attacker from accessing the data in the website
	- so if one of the employees entered a malicious website created by the attacker then the attacker can send a Cross-Site Request to the Internal website and if there is `CORS misconfiguration`, then the attacker got the data
- 


# Testing Mindset
- search for request which fetch a sensitive data
- write `Origin: ` in the request and add the tested value
- Test all possible values
- Test null request, and see if it reflects
- Test all subdomains of this domain and see all Origins which allowed to interact with this Origin and try to find `XSS` in it, and then you can inject a script to steal the data and send it to you
	- or all `CNAME`s in the `DNS` , and try to find subdomain takeover 
- if you found `misconfiguration` like that 
	- `Access-Control-Allow-Origin: *`
	- `Access-Control-Allow-Credentials: true`
	- which allow to any Origin to read the responses from this Origin, the browser will block it by default




# Valuable Ideas

- If you found that the `CORS` allow the null, so to force the browser to send the request with `Origin: null` use the `iframe`
	-  `<iframe sandbox="allow-scripts" srcdoc="/code/" ></iframe>`
	- the `sandboxediframe with no sandbox="allow-same-origin"` force the browser to send any request inside the `iframe` with `Origin: null`  
- use `document.location` in case of `XSS injection in the specific Origin`
- If you found a Request that add or edit in a sensitive data, try to change the HTTP method to GET and change the other stuff
	- see if it accepted, and if it does, try to test on it
- 