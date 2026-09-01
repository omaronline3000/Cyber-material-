

# URL Encoding


# HPP (HTTP Parameter Pollution)

![[Screenshot from 2025-11-24 03-12-51.png]]


- do not rely on theses , still the reaction is unpredictable , try inject the payload in each one at a time and see which one is reflected in the page , so the reflected one that what the `WAF` scan and you can divide the payload between the both and test 


# Web Parameter Tampering

- Interceptor 


# Origin IP Discovery 

- used when the website use the IP of the `WAF` by default `put it in the A record in Authoritative DNS server`
- ##### Techniques
	- DNS History Research
		- Tools:  [`SecurityTrails`](https://securitytrails.com/), [`ViewDNS`](https://viewdns.info/), or [`DNSDumpster`](https://dnsdumpster.com/).
		- What to do: Look for the oldest IP address associated with the domain. There is a high chance the server is still using that same IP.
	- Subdomain "Leaks"
		-  The Logic: Services like mail (`SMTP`) or FTP often need a direct connection to work properly and cannot sit behind a standard web proxy.
		- How to apply: Use a subdomain scanner (like `subfinder`) and ping the results. If a subdomain returns an IP that isn't owned by Cloudflare, you’ve likely found the origin
	- Outbound Connections
		- The Logic: If the website has a feature that makes the **server** send a request to **you**, the server will reveal its true identity.
		- Email Headers: Sign up for a newsletter or trigger a "Forgot Password" email. View the email's "Raw Source." Look at the `Received:` headers. The IP address of the server that originally sent the email is often the origin IP.
	- [`Censys`](https://Censys.io)/[`Shodan`](https://shodan.io) (Certificate Searching)
		- Internet scanners crawl every IP on the planet and record which SSL certificates they are using.
		- How to apply: Go to **Censys.io** and search for the domain name in the "Certificates" category.
	     - The Goal: Find an IP address that is currently serving a valid SSL certificate for `example.com` but is NOT a Cloudflare IP.



