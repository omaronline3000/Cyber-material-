# Core

> Back end is the main thing in the website `the brain` , it connect to the data base to save the data , receive the requests that sent to specific endpoints and it do the desired instructions and return the web page `html file` to the browser

> the endpoints predefined on it

> the paths , which written in form tag , consider the endpoints and it signed in the back end when it receive any requests on it , it apply the specified instruction 

---

## Notes
- while using NoSQL DB, the developer determine the format that the data will be saved as in the data base
	- for Instance, JSON
- any element call backend should be in form tag, to determine the endpoint and the method type 
- there is three types of parameters
	- `Path Parameter` : which when the parameters values be part of the `Route/Endpoint`, like:
		- `https://ex.example.com/findSum/5/8` , which be like this in the back-end `/findSum/:firstnum/:secondnum`
		- another example:
			- the path may be like this `view/:id` , when I added a colons that's mean it's variable not one specific endpoint, and the backed will redirect you to the desired endpoint
	- `Body Parameters`: which sent in the body like `JSON`
	- `Query Parameters`: which is the `URL-encoded` that sent in the URL in the `GET`Request
- `WebApps` Development in the past was 
	- `SSR (Server-Side Rendering)` 
		- which men when you entering to the web site and got the initial page, with every Endpoint you ask for, the server has all theses pages inside the back-end Files `MVC`, so it sends too you the whole page
	- but now after the propagation of `JS ferameworks`the most of websites use `CSR (Client-Side Rendering)`
		- which men when you entering to the web site and got the initial page, with every Endpoint you ask for, the browser send to the Components which predefined in the Front-end using `JS Framework` and received with the initial page, and theses Component changes in the page Content, and then the browser send a request to the server to get the data, in this case the back-end just contain the `APIs`, so the Endpoints is `API Endpoints` , and the response is Format of Data - like `JSON or XML`- not HTML file
		- and due to this methodology, the number of required html files is just one file - beside the components files -, so it called `SPA (Single-Page Application )`
			- `PushState()`function related to this topic, it change the content of the page without any refreshing, it just change the `url`, and the `JS` manipulate the exist html, it may just send `fetch()` request to gather some data from the `API`
- Flask’s flexible routing can lead to `misconfigurations`
- Django’s ORM (Object-Relational Mapper) prevents SQL injection
- 