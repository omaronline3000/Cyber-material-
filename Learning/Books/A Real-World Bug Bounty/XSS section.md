
- if i affected on `iframe` with the payload in the specific page, it's low severity `xss` cause the `iframe` is `sandboxed`  so the `SOP` is different so you can not access sensitive data from the real page
- you can not use the trick of `autofocus onfocus=` in the hidden fields
-  `xss` auditors which protect the user from links which execute malicious Java Script, it appear in the browser broken page with error
- after entering the payload like in potential `stored XSS` like when you inject it in `Profile Page`, you should know where the reflection or the rendering happened across all the app
	- like it might be rendered when someone enter your account `if that functionality exist` , not when in yours
	- or maybe rendered if someone sent to you a message `if that functionality exist`
- when you find `self-XSS` try to chains with `CSRF`  for example
- XSS payloads might not execute immediately
- the websites sometimes do only one time `sanitization` then save it in the data base, or then send it again without any another security constraints like `encoding or sanitizing`
	- that's dangerous because bypassing this only level `sanitization` mean I can do `XSS attak`
	- and even it was very strong `sanitization`, in the future while alternating the code or add new code it may use the in different manner was not known while making the `sanitization` so it may lean to `xss attack`
-  Note For Crafting the Payload
	- the HTML allow zero or more spaces after the equal sign to assign the value so `<inpug checked= onclick="alert()"` will not work because it will assume the event handler is the value of the `checked` attriubte
	- use it when you find a website `sanitize` just the values of the attributes and let them and equal sign
- you can use `/ /` instead of `" "` 
- try to double the attributes like injecting two `src` 
- sometimes your account logo when you click on it in the website it redirect you to your profile and also other users
	- try to inject in the `<a>` tag `javascript:alert()`
- if there is something clickable and after clicking the payload should trigger, you can try does not click with mouse try shortcuts or moving using `tab button` , because the `sanitization` may happened by mouse clicking event
- if there are any other ways to Enter inputs instead of the website itself, like uploading a file or data or image
- very nice to use `<iframe>` in websites which sanitize your payload, you can do this `<iframe src="javascript:alert(document.domain)">` and because the pseudo protocol inherits the Origin from the parent page you can reach the `info, cookies, and all things` 
- 
---

Resources:

- https://github.com/masatokinugawa/filterbypass/wiki/Browser's-XSS-Filter-Bypass-Cheat-Sheet
- https://whitton.io/articles/uber-turning-self-xss-into-good-xss/
- https://html5sec.org/
- 