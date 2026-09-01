
- One of the main purposes of business logic is to enforce the rules and constraints that were defined when designing the application or functionality
- the developers who does not fully understand the software what they build, lead to many logic flaws
- and the developers who work on functions separately without understanding when theses functions combined, what is probable cases and so on.... 
-  which mean an bug happened due to the logic of the developer code, like
	- if the developer of e-commerce app developed it to add a voucher within a specific period and after this period you tired the voucher and it worked
	
	- or if the developer developed it to add to you 50% discount of the any product, if you bought specific product, and after you added this product to the cart you got any product after that and the discount applied, then you deleted the first product from the cart if the discount still applied so that's a bug.
- sometimes there is a conflict happened between it and Context-Based Access Control Vulnerability
	- the access control vulnerability meaning the attacker can access or do action which he should not access on it or do it
	- meanwhile the logic bugs are error from the developer made the user to bypass something or do some actions which does not allowed to happened in the system at all
- it sometimes also mixed with other vulnerabilities like `2FA bypass` actually it's Authentication Vulnerability, but sometimes the `Evasion` happened because error in the logic so it consider also logic bug

- # PortSwigger Ideas

1. if the website send the price of products in the request without validating the value from the database, so I can manipulate the price
2. if I can add negative value in the quantity parameter, so you you turned the balance to minus
3. if there is no validation in the back-end on the `overflow` if the value entered bigger than the data type, so if I maximized the `quantity`, the balance may exceed the maximum range, so it will contain garbage/minus value
4. if there is two vouchers try to add the twice and then add them again in reverse order
5. if there is change-password functionality, and it require the username, current password and the new one, try to change password of another user or admin, by deleting current password parameter
6. if there is `Insufficient workflow validation` , for example if there is confirmation request sent when you buy a product that your credits can cover, try to put a product that you have not enough balance for it in your cart and send this request
7. if there is page after authentication let you select your privilege, try to drop the request of this page after the login page
	1. meaning you enter to the website without determining any role
8. try abuse gift cards, like in the pre-last lab
9. 


- # Scenarios Examples
	- bank transfer functionality transfer any amount of money without any approval while it less than 10,000$
		-  # scenario 1:  but what if you entered a huge number but with minus, so the comparison process will consider it less than 10,000$, and when it requested the minus will deleted so you bypassed the approval process
		-  # scenario 2: exactly like the previous but one, but what if the money sent with the minus sign, that mean it will abstract from the destination account
	- sometimes there is functionalities which appear for only admins and there is security issues, so if you just updated the mail to mail with the company mail server name, even if that email not actually exist, the system will appear the functionalities to you

	- if there is any grey data `meaning you can not edit it`, try to delete any client-side restrictions and manipulate it
	- `reCaptcha` test aims to prevent sending many requests, so it protect against rate limits bugs
		- so if you can bypass it or there is any logic bug in it like
			- when it just rely on mathematical operation and when it sent it does not expire so if you saved the request and send it multiple times, you bypassed the rate limit defense
	- find an API endpoint which retrieve data about some object in the system `product in case of e-commerse website` and try to change the request method to `PUT` and change the `content-type` to `json` and reassign new value to one of received values
	- [Ideas from here](https://github.com/Az0x7/vulnerability-Checklist/tree/main)
	- testing any vouchers or coupons features, try to reuse it, or if there is more than one, try to reuse but not use the siblings forwarding
	- try to increase the total number of products price, the developers may did not handle the overflow of the data type, so it may be negative
	- 