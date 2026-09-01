## # Frameworks Notes:

## Angular `JS`

- `JS framework`, lets the user scans the HTML tags content , and extend it's functionality
- `ng-app` the directive which exist in the root tag
- it allow you to make something called `angularJS expression` using pair of doubly braces
- if the developer does not validate and sanitize the user input properly, you can inject the doubly braces pair and inject Angular expression
- but the `AngularJS` expression executed in sandbox so it does not have access on window object so there is limitations in invocation for global function like alert, so we used this `{{$on.constructor('alert(3)')();}}`
	- `$on` is an Angular built-in function, any other function will also work
	- and secondly the constructor property which refer to the function which created this object 
		- I used the constructor trick to bypass the sandbox and expression context that prevent me to use global scope function like alert by declaring a new anonymous function dynamically while runtime and invoked it which will run in global scope.
	 - there are more functions like `$broatcast` `$emit` 
	 - you may found the DOM do the changes like adding values to input fields using properties not attributes
	 - 
