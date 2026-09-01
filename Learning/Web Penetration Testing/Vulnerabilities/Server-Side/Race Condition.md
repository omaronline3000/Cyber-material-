
- A **race condition** is a type of vulnerability in web applications where two or more actions happen _at the same time_ and interfere with each other, causing unexpected or insecure behavior. It usually happens when the server processes multiple requests concurrently without properly coordinating them.

- This can lead to a situation where:
	- Multiple threads/processes access and modify the same data at the same time,
	- And because the application doesn’t protect against this, the result becomes unpredictable or exploitable.

- there is something called Race Window which is the time period between you sending the request and specific function in server-side validating something or blocking this request to be success again

- if you send many request in the same time in the Race Window the system may keep applying theses requests until the Race Window die, **that called `Race Condition`**, so the other requests will refused but the previous ones success multiple-times

* one of the most important newly insights discovery:
	* in the past the researcher thought that race condition exist only in `mult-step workflows` endpoint, meaning the action must happening with multiple requests to be potential for `race condition`
		* like if you want to redeem a gift card you should buy it first and then use it, so this two steps, you can do `race condition` on the first request and get more than gift card, or on the next one and activate it more than one time
	* but newly researchers discovered that even the process which done by one request, it contain multiple hidden steps in the server which called **`sub-states`** 
		* for example there is one request to get a gift card and activate it, and every step in the server side create `hidden sub-state` so, if i sent many requests to this endpoint, if the both arrive in the `race window` while the system busy with specific temporary sub-state, the second request will arrive and also manipulated

## Multiple-Endpoint Race Conditions

- meaning that if there is two different endpoint has an access to edit on the same states, what will happened if I send the parallel request to them, so while the first request manipulate the state and change in it, the second one will  assume the state is stable and work on this assumption, which lead to unpredictable behavior 
	- like if you send request to redeem gift card which is increase the balance, and in parallel send a request to checkout which decrease the balance, so what will happened ?

## Single-Endpoint Race Conditions


- ## Types of Race Conditions
	- ### "Time-of-check to Time-of-use" (TOCTOU) 
		- #### Limit Overrun
			- which mean sending the request multiple times in the same `race window`, to bypass the rate-limit on specific endpoint
			- There are many variations of this kind of attack, including:
				- Redeeming a gift card multiple times
				- Rating a product multiple times
				- Withdrawing or transferring cash in excess of your account balance
				- Reusing a single `CAPTCHA` solution
				- Bypassing an anti-brute-force rate limit
			- there are three layers of time, the packet pass across it, which are :
				- network latency
				- `jitter`
				- internal latency
			- the internal latency is the good factor for hackers in this list, because it creates the `race window` which occur `race conditions` 
			- but the network latency and network `jitter` is the bad part which affect on the time that the requests arrive to the server, so the time in the most times exceed the `race winodw` so the attack fails
				-  but the modern techniques like `signl packet attack` is mostly mitigating from this effect on the time
			- tools like `burp repeater (single-packet attack) and turbo intruder`, helping in detection of these vulnerabilities 
			- sending two request mostly enough `POC` but sending many requests helps to mitigate internal latency, also known as `server-side jitter`

- ## Tools
	- simple testing using `Burp Repeater group`
	- more powerful using `turbo intruder`

- ## Ideas 
	- bypassing the rate-limit on sensitive function.
		- like doing brute-force on administrator password.
	- 