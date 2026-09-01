
## Passive Recon
- gathering information about specific target but without rubbing it, like:
	- gathering info about employers of the company on social media
	- or gathering info about the company or the domain or anything about it but without rubbing it
	- even the data which normally you should rub the target to find it, if you found it on third -party, that mean you did not rub the target so it consider `Passive Recon`

## Active Recon

- Unlike Passive one, the Active require rubbing the target to gather the info, like gathering the URLs, Directories, and more like this, that require sending requests for the website

## FootPrinting vs FingerPrinting

- we can call it (macro vs micro) 
	- `Footprinting` is about gathering a huge information about the target with wide scope and the objective here is collecting information as possible
	- `Fingerprinting` is about identify the traits of specific system like `OSes` or `Languages` and more .....

# Web



## definition:

-  gathering Info about a specific target , like:
	- URLs
	- Subdomains
	- dorks
	- JS files
	- PDFs
	- CIDR -> ranges of IP's associated to the company

- I am not collecting a data about every thing `it's not your target` , you search about valuable data to found a bug so, recon for:
	- URLs\ Endpoints, Subdomains, Requests(in burp), Secrets, Functions   
### there are 3 types of Scopes in Recon :

#### #1 Small Scope

- when you have a specific subdomain or specific routes or paths to test on
- sometimes the companies make a special subdomain to let you test on, so as not to cause stopping the service or the website, it maybe like:
	- if they have `api.domain.com` so the testing one may be -> `dev-api.domain.com` or `test-api.domain.com` and so on .....
- 

#### #2 Medium Scope

- here you have a full domain and its subdomains to test on, called `wild card` 
- there are somethings called `acquisitions` and `subsidiary` 
	- acquisitions -> the companies which the main company took and got benefits from it and added it in its collection
	-  subsidiary  -> are the companies which follow main company, like `Meta` and  `facebook`  

#### #1 Large Scope

- here you search about every thing related to this company `every thing`

### Diagrams for Clarity:
1)
![[Screenshot from 2025-10-08 19-53-14.png]]

2)
![[Screenshot from 2025-10-08 19-55-12.png]]


---
## Types and Tools:

### Tools: 
-  Google Dorks
	- `site`
	- `inurl`
	- `intitle`
	- `link`
	- `filetype`
	- `wildcard (*)`
	- `Quotes (" ")`
	- `Or (|)`
	- `Minus (-)`
	- `ext` (.php, .cfm, .asp, .jsp, .pl) => all of these are scripting files
	- [Google dashboard which many hackers share their useful queries ](https://www.exploit-db.com/google-hacking-database/)
		- `Dork Search` -> website
		- `google dorks cheatsheet` -> `github repo
### Types: 

- #### Scope Discovery 
	- ##### `whois` and `Reverse whois`
		- `whois:`that's  a protocol used to query an information from the databases about specific domain like the domain owner or the organization's domain and so on...
		- `Reverse whois:` that's reverse the previous process after gather the organization of the domain, it search for the domains that also belongs to that organization 
	- ##### IP Addresses
		- `ASN` & `CIDR`
	- ##### Certificate Parsing
		- every `SSL/TSL` certificate contains field called `SAN(subject alternatives name)`
			- which give the user the ability to use the same certificate to multiple `hostnames`
			- Tools: [`crt.sh`](https://crt.sh) [`Censys`](https://Censys.io)[`Shodan`](https://shodan.io)
	- ##### Subdomain Enumeration 
		- there is online tools which use a services, to enumerate
			- Tools  :
				- `subfinder`
					- `subfinder -d <domain>`
				- `amass`
					- `amass enum -d <domain>`
				- `httpx` -> find live subdomain 
					- `httpx -sc -title -follow-redirects -port 80,443,8080,8443` 
	- that's consider passive recon,because the websites use services to find the subdomain so no rubbing with the target here, and the tools use API's for services and websites so also you do not rub with the website

	- ##### Service Enumeration
		- `nmap -sV`
	- ##### Directory Fuzzing
		- Using any `Fuzzer` 
			- `ffuf`
			- `dirsearch`
	- ##### `Spidering or Endpoints/URLs crawling`
		- Endpoints /URLs Crawling
		- Tools:
			- `findsomething` -> browser extension
			- `katana` -> web crawler `active URLs`
			- `waymore` -> to find all Endpoints/URLs `from web archive` 
				- `waymore -i <target> -mode <mode>`
	- ##### Third-Party Hosting
		- at many times the companies use a `Third-party company` to host website or host data or save some backups and more ......
		- I can find in it - if it's accessible -  `hidden endpoints, logs, credentials, user infor-mation, source code, and other information that might be useful`
		- one of the examples is `s3(simple storage service) buckets`, and many times the format of the `URL` of the bucket be like this `Bucket.s3.amazon.com` or `s3.amazon.com/Bucket` 
			- Techniques and Tools to find
				- Google Dorks
					- `site:s3.amazonaws.com COMPANY_NAME`
					- `site:amazonaws.com COMPANY_NAME`
					- even if the company use custom URL to its buckets, use more flexible queries, and it most time keep using some keywords like `aws , s3` 
						- `amazonaws s3 COMPANY_NAME`
						- `amazonaws bucket COMPANY_NAME`
						- `amazonaws COMPANY_NAME`
						- `s3 COMPANY_NAME`
				- GitHub Dorks
					-  try to search for term `s3` in GitHub repos
				- GrayhatWarfare (https://buckets.grayhatwarfare.com/)
					- search engine for exposed `s3` buckets 
				- Lazys3 (https://github.com/nahamsec/lazys3/)
					- brute-forcing tool using word list to guess the buckets `URLS`
				- Bucket Stream (https://github.com/eth0izzle/bucket-stream/)
					- Tool that parse the certificate of the organization and try to guess the buckets based on the domains which associated with this certificate 
					- and it's accessible or not
		- after reaching the buckets `URLs`, try to access and if you can access it and it's contain a sensitive data or you can upload or delete any thing from it `it's consider a vulnerability`
			- use `awscli` tool 

	- ##### GitHub Recon
		-  **Target Identification:** Locate relevant GitHub usernames by searching for the organization name, product names, or known employees.
		* **Contributor Mapping:** Record project-related repositories and audit **top contributors** to find additional relevant `codebases`.
		* **Audit Issues & Commits:** Scan these sections for information leaks, unresolved bugs, and recent security patches that may contain new flaws.
		* **Code Investigation:** Search the Code section for vulnerable snippets and use **History** and **Blame** to track how sensitive files were developed.
		* **Secret Hunting:** Search for $`hardcoded`$ API keys, encryption keys, and database passwords using keywords like `key`, `secret`, and `password`.
		* **Credential Validation:** Use **`KeyHacks`** to verify if discovered credentials are valid and to determine how to access the target's services.
		* **Sensitive Functionality:** Focus on source code handling authentication, password resets, state-changing actions, and private data retrieval.
		* **Entry Points Mapping:** Identify code that processes user input, specifically:
	    * HTTP parameters, headers, and request paths.
	    * Database entries, file reads, and file uploads.
		* **Infrastructure Recon:** Analyze configuration files for internal infrastructure details, old endpoints, and **`S3` bucket URLs**.
		* **Dependency Auditing:** Document imports and version lists to identify outdated dependencies with publicly disclosed vulnerabilities.
		* Tools:
			- Gitrob (https://github.com/michenriksen/gitrob/) locates potentially sensitive files pushed to public repositories on GitHub
			- TruffleHog (https://github.com/trufflesecurity/truffleHog/) specializes in finding secrets in repositories by con-ducting regex searches and scanning for high-entropy strings
			- KeyHacks (https://github.com/streaak/keyhacks/) to check if the credentials are valid and learn how to use them to access the target’s services.
	- ##### `JS` Files Analysis
		-  I can gather from it :
			- API tokens
			- Database credentials
			- Developer comments
			- hidden endpoints
			- hidden subdomains
		- in modern JS files, it contained all features and endpoints even it's not visual in the UI of your account but it mentioned in the js files

 - #### Sneaky OSINT Techniques
	 - **Job Post Analysis:** Review engineering job listings to identify the company's tech stack (e.g., Python, Flask, AWS services) and operating systems (e.g., Linux).
	* **Employee Profiles & Forums:** Audit LinkedIn profiles, personal blogs, and technical questions on Stack Overflow or Quora; employee expertise typically mirrors the company's development environment.
	* **Public Google Calendars:** Search for accidentally shared work calendars which may leak meeting notes, internal slides, or login credentials.
	* **Social Media Scrutiny:** Monitor corporate and employee social media for info leaks, such as credentials on Post-it notes visible in office photos.
	* **Engineering Mailing Lists:** Join company mailing lists to gain insight into the internal development process and architectural challenges.
	* **SlideShare Review:** Search for uploaded conference or internal meeting presentations to understand the technology stack and known security hurdles.
	* **Pastebin Monitoring:** * Search for the organization's name to find shared source code, server logs, or development comments.
	* Tools:
		 - PasteHunter (https://github.com/kevthehermit/PasteHunter/) to scan for publicly pasted data.
		 - Wayback Machine (https://archive.org/web/)
- #### Tech Stack Fingerprinting
	- **Version Identification:** Identify software brands and versions to research known misconfigurations and publicly disclosed vulnerabilities (CVEs) on the [CVE database](https://cve.mitre.org/cve/search_cve_list.html).
	* **Nmap Fingerprinting:** Run `nmap -sV` on the target to enable version detection, which reveals specific software versions running on open ports and identifies the OS.
		* **HTTP Header Analysis:** Inspect server responses in Burp Suite for identifying headers:
	    * **Server:** Often reveals the web server software and version (e.g., Apache/2.4.7).
	    * **X-Powered-By:** Indicates the backend language or framework (e.g., PHP/5.0.1).
	    * **Technology-Specific Headers:** Look for unique headers like `X-Generator` (Drupal) or cookies like `PHPSESSID` (PHP).
	* **HTML Source Inspection:** Search page source code (CTRL-F) for signatures like "powered by," "built with," or "running" to find framework names and versions.
	* **FileSystem Signatures:** Check for technology-specific files and directories (e.g., `/phpmyadmin` for PHP or `jinja2` folders for Django).
	* **Automated Fingerprinting Tools:**
	    * **Wappalyzer:** A browser extension for identifying CMS, frameworks, and languages.
	    * **BuiltWith:** A web-based tool to view a site's entire technology stack.
	    * **StackShare:** A platform where developers voluntarily list the tech stacks used by their organizations.
	    * **Retire.js:** A tool specifically designed to detect outdated JavaScript libraries and Node.js packages.
#### additional and techniques
- you can find URLs/Endpoints or even sensitive data
	- look up for`robots.txt` 
- search for hidden URLs/Endpoints, it exists in many places like `Dorks , robots.txt , JS files, ........`

- ==after Recon, create two accounts on the website and try every thing on it, and take notes on what you found with the potential bug==
- 
---


![[Screenshot from 2025-11-30 02-03-48.png]]

![[Screenshot from 2025-11-30 02-04-12.png]]


# Mobile

# API

# Network
