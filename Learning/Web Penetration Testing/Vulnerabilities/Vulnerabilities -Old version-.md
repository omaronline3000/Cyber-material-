
# Vulnerabilities

→ why the most kinds of vulnerabilities happening ?

→ because the developer trust in the user input and use it in multiple things like print it in the website (XSS), and use the input in a query in data base (SQLI), and so on…….

Notes:

- HTML files act with all characters normally, that’s mean even you write the payload within quotes, it will executed normally
    - tags work anywhere in HTML files (important: script tag)
- [location.search](http://location.search) → return the parameters in URL
- trick → i can encode the parameters again (double encoding)
    - the WAF will decode it ones and it will not recognize it
    - foreword the original
        - if the backend decode the parameters once (trying failed)
        - if the backend decode the parameters twice, then the payload will appear and if it will put it in the response without reencoding, the XSS will affect
- 

VULNS:

1. subdomain takeover → when the account of the company in the CNAM service provider expired, and they does not break the connection between them , so if i rent the same alias from the provider i can access and control on the original domain.

2. session fixation → the session ID does not change so , if some one hacked the session id , he hacked the account forever, due to the session will not change

3. DNS zone transfer → misconfiguration in the master file, does not restricts the IPs which can transfer the data using queries like AXFR
    - so any one can steel the zone file of the specific domain
    
4. information disclosure -> (not a vulnerability) the developer let some info like type of technology or version of something and so on in the source code or displayed in the website
	- data is not harmful, but may inform the hacker some info, which may help him in the attack

5. sensitive data exposure → also the developer forget info in the source code or displayed in the website, but here the data is sensitive and harmful like credentials or passwords and so on
	- which is vulnerability due to the hacker reached sensitive data may attack with it


6. XSS (cross site scripting)
    - explanation (why ?)
        - this vuln allow user to execute JS codes in the Infected page
        - because the developer trust in the user input and use it in the web page, like comments and posts and so on…..
    - exploitation
        - injecting JS code in input field in the page
    - Impact
        - the most dangerous scenario is stealing Cookies of the user
    - Mitigation
        - validation → refusing the input of the user if detected any malicious input (display a message for the user inform him that re enter the input)
        - sanitization → let the user enter whatever he want and after this filter the input (deleting all special characters and let just the plain text)
        - encoding → let the user enter whatever he want and after this encode all the special characters which used in the coding (so the characters will displayed as it, but in the HTML it will written in a different way (<) ==  &lt )
    - Types
        - Reflected
            - the payload being in the URL, so to exploit it you should send URL to the victim
                - the payload being value of a parameter
            - often with GET request
        - Stored (most dangerous)
            - the infected page saves the payload in the Data Base, so you just send the payload one time, and then anyone open this page will be effected
                - like comments, Facebook pages and so on
        - DOM-Based
            - it’s like Reflected, but here the sent data received by JS (client-side), but the Reflected payload received by the server-side (ex: PHP)
7. 