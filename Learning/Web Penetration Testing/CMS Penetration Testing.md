
# Wordpress

- WordPress is combination of themes and plugins which maybe contain `CVEs`
	- which you can exploit it
- I can by adding user by `POST or PUT` but If i found an Authorization header
- 

## Recon and Enumeration

- find the version of the wordpress, not all companies using the up-to-date version of wordpress
	- use tools like `Wappalyzer`, `whatweb`,`enumerate manually like find meta tag which contain the version`

- ### Plugins Enumeration
	- `curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d'"' -f2`

- ### Themes Enumeration
	- `curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d'"' -f2`


## Sensitive Static Paths/Endpoints

- `author-sitemap.xml` => return usernames 

## Notes:
- it mainly relies on `jQuery` so understanding it and know its `CVEs` is crucial 
	- interesting [`jQuery Security`](https://trustedsec.com/blog/everything-you-need-to-know-about-jquery-and-its-vulnerabilities)
- `YesWeHack` and `BugCrowd` consider if the `wp-login.php?action=register` is open, os it's a vulnerability