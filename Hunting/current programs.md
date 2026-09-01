


| Program name | Program type | Subs | Recon |
| ------------ | ------------ | ---- | ----- |
| Inzpire      | VDP          |      |       |
| sprouts gigs | enteranmit   |      |       |

# Notes:

- After your current Target, test on `citigroup` 




---
# ==HackeroneTargets== 


## Inzpire:

- [x] prizmsupport.inzpire.com
- [x] www.inzpire.com
- [ ] insulet Corporation

Notes:

- `https://www.inzpire.com/?p=` couldn't bypass `cloud flare WAF`  : `strong potential for Reflected XSS` 


Current Notes:
- Content spoofing in registeration
- try to test forget ID trick `maybe injecting a malicious`
- error trick in `https://insulet.my.site.com/PodderCentral/s/compass-home`
- 

## Trendyol:

- Notes while browsing:
	- it use `React` `zone.js` frameworks
	- there are errors in multiple function in the website
- Interesting points:
	- `https://www.trendyol.com/en/select-country?cb=/ar/two-piece-sets-x-c83?pi=2&offset=2000004` -> I think you can inject/change the values of the countries from the request
	- look if the filters attributes can injected to it and is it client side or server side
	- in `اسأل البائع` the question appear in specific section on your account try inject anything, and if it effected try `blind xss` , it maybe `self xss` 

	- according to the language it seems that the pages design different from one to another, like `user info` , and do not forget test it
	- when you adding an address there is a problem while sending text in the last text area, try to inspect the request and look for the data type which predicted to be sent.
	- after clicking `مساعدة Trendyol`,  you will asked for rating to the website, try `blind xss`
	- there is `طلب بياناتك` which send to you mail with your data , so if you injected a payload in anyplace in your data, it may rendered in the sent mail.
	- you can bypass the `WAF` by separate the special characters using the quotes and slashes

----
# XSS BOOTCAMP

### 41 VDP  -> Finding XSS


# ==Bug crowd== 
# ThreatDesk





# ==external programs==

| program name | mail                   | Notes:                                                                             | Policy                                                          |
| ------------ | ---------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| crisp.se     | mailto:csirt@crisp.se  | -> there is subdomain `signup.crisp.se` potential for interesting things<br><br>-> |                                                                 |
| jedox.com    | mailto:trust@jedox.com |                                                                                    | https://www.jedox.com/en/trust/vulnerability-disclosure-policy/ |
- try to block JS from your browser and test on the Wikipidia `noscript tag`