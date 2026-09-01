
# Recon

- [ ] Subdomain enumeration using `(amass enum -d target.tld -silent ; subfinder -d target.tld -silent) | sort -u > subs.txt`

- [ ] Filtering the live subdomains using `httpx -l subs.txt -sc -silent -follow-redirects -port 80,443,8080,8443 | tee live_subs.txt | awk '$2=="[200]" {print $1}' > live_accessable_subs.txt`

- [ ] Web URLs crawling and `WebArchive` Discovery, using `waymore -i target.tld -mode U -oU Archived_URLs.txt && cp Archived_URLs.txt All_URLs.txt && katana -u targret.tld | tee Active_URLs.txt | tee -a All_URLs.txt  > /dev/null`

- [ ] Visual Recon `Screenshots` using `httpx -ss -silent -srd /directory`  

- [ ] Gather `JS` files using `grep -i '\.js$' All_URLs.txt > JS_files.txt`

- [ ] Gather endpoints from `JS` files `cat JS_files.txt | xargs -n 1 -P 10 curl -sk | grep -Po "(['\"\!(put bactek here)!])\K/[a-zA-Z0-9_?&=\/\-\#\.]+(?=\1)" | sort -u > fetched_endpoints.txt`
	-  or use `LinkFinder` 
- [ ] Gather the parameterized URLs `grep -E "https?://[^\s?]+\?[^\s]+" All_URLs.txt > Parmaterized_URLs.txt` 
- [ ] Gather the parameters using `arjun -u <URL> -o params.json` , `arjun -u <URL> -o params.json -m POST`

- [ ] Port Scanning for subdomains using `nmap -sT -sV -A <target.tld/IP>`

- [ ] search for any `CVE` for any used technology 

---
# Automated Testing

# Run the Automation of some Vulnerabilities while Discovering the APP

# 1- Automate the subdomain takeover :

- [ ]  using `nuclei -l live.txt -tags takeover -severity low,medium,high,critical -o takeover_results.txt`

# 2 - Automate Parameters Reflection 

- [ ] using `cat urls.txt | Gxss -c 100 -p 0xPyrmd -o Reflecting_parameters_URLs.txt` 
`

# 3 - Automate Information Disclosure

- [ ] using for JS Analysis `cat fetched_endpoints.txt js_urls.txt | mantra`

---

# Manual Testing

- Start Discovering all Functionalities in the `WebApp` 
- Write all the functionalities and features that the app provide
- take notes for every interesting things for you , and write beside it the potential bugs for it
- when you start hunting - after using the app like normal users - walk sequentially on the bugs 
	- do not hunt on two bugs in the same time


- test 403 forbidden
- remeber nahemsec style xss