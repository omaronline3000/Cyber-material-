# Programming

# Languages :

1. JS
    1. DOM (document object model)
        1. One of the components of BOM in JS
        2. that’s an object in JS , let us access and modify the content of HTML files
        3. it has a tree hierarchy
            1. has a root (HTML tag) and has a leaves (Text Tags) or even another tag
        4. the white spaces consider Text objects, but the browser does not appear them if it was in the start or the end of the text (alphanumeric characters) or even another tags , to save the screen display of the website
        5. any spaces before head tag ignored (historical reasons) that’s mean it isn’t part of the DOM Tree
        6. if you write any tags or text after `</body>` it is automatically will return within the scop of the body , due to (according to HTML spec) it can not be any content after body scope , even spaces ignored in DOM tree
        7. if you missed any essentials tags like body or head or even html , the browser will add it by default to be able to use DOM
        8. if you made any errors like missing closing the tags , the browser will correct it , to be able to use  DOM
        9. `<tbody>` is a tag that the browser creates it , and access it with the DOM , to be able to handle tables
        10. there is 12 object type in DOM , only 4 is the common (usually used)
            1. document → this is the entire HTML file
            2. Tags (nodes)
            3. Text
            4. comments
    2. JS file may exist in the same webserver with HTML or in another place like cloud
    3. dialogue boxes
    4. best practices using JS
        1. avoid relying on just client-side validation
        2. do not use external untrusted libraries
        3. avoid hardcoded the secrets like API keys , tokens , ….
        4. always use Minify and Obfuscation
    
# PHP Notes 

1. 

---

# Notes & Topics :

- minification → is a process to delete all unnecessary characters to make the size of the file smaller and load of the file faster (on the web)
- Obfuscation → is the process to make the code harder to read and understand, does not aim to enhance any thing in the code, just make it un readable to prevent reverse engineering by the hackers
    - there is tools to obfuscate and de-obfuscate
- clean code is the opposite of the previous topics
- if the de-obfuscation website failed , try AI

---

## Web behavior

1. the HTML start parsing(DOM build + rendering) first (the HTTP response just carry the HTML code), while parsing HTML if you encounter :
	1. link tag -> if was "stylesheet" type, then it blocks (rendering), and fetch CSS and parsing it to create (CSSOM), but the DOM building continue, and if you encounter `<script>` after this link, the parsing will not start until CSS finish, but if it was another type of link tag the parsing will continue (not-block) just fetch it in parallel
	2. `script`, if  it was :
		1. normal no attributes -> the HTML parse stop (rendering and DOM building)
			- if it was inline, the JS engine (interpreting, JIT compiling and executing ) and parse it and execute, then the parsing of HTML continue
			
			- if it was external, the browser send HTTP request to get JS code, and then send it to the JS engine, and after parsing and execution, the HTML continue

			- if you will not put any attributes, it's best practice to put all script tags in the end of the html file
		
			- html download the files , if it was not cached before
		
		2. with `async` attribute -> the HTML continue, while downloading the file, when starting the parsing and execution the HTML pause until JS finish , but if you have many script with async , order of execution not guaranteed.
		
		3. with `defer` attribute -> the HTML will continue, while downloading the file, and after HTML finish parsing, the script tags will parse in order
	3. images tags -> fetching the image in parallel
# JS notes

1. JavaScript translation series: ``` Source code (text) → Parser → AST (tree) → Interpreter/JIT → machine code.```

2. so for JS external files
        1. if you wrote the script tag in the beginning like in head tag , when the browser reach it , it will go to js file and start parse it before continuing the website (it’s normal) but if i found any error the parsing will stop , and the web site will not continue rendering , so it was best practice to write the script tag under the page
            1. now, you can use “defer” attribute so the script will ignored until the page loaded and then the browser will start execute 

3. let/const for variables
    1. expert in JS say do not use var again
4. typeof “variable” → return the data type
5. Elements selectors in DOM which return HTML section can not apply effects like changing HTML content or the style (can not edit due to you did not select specific one)
    1. ex:
        1. getELementsByClassName()
        2. querySelectorAll()
    2. you can iterate on them and apply on every one
        1. but you should convert “getELementsByClassName()” first due to it’s return collection not iterable ‘Array.from()’

6. the properties which I assign with JS for HTML nodes which I catch or create  , that’s a variables I can manipulate with it as a variable so I can do many things like assign new value or print it or convert it in another place and use it, and so on.

7. the Hoisting phase when the java script read all the variables and function statements and recognize it , but not its value , so if you tried to use it before its declaration , you will not face an error but the content of the variable is undefined 
	1. in new JS , let and const will back to you reference error if you tried to access it before declaration and it's best practice to use function expression and arrow function

8. whether function expression , function declaration or even arrow function , you have to write `()` to invoke it, and if you passing the function expression or arrow function itself without assign it to a variable first, you should write it in this format "( the function here )();" if the function which pass to it expect a value which our function will give it to it, but if you using function expect a reference to the function and it will execute it later like (addEventListener , setTimeOut)
9. more your website source code contain many files and many lines, more time took to load it
10. JS was interpreted but in the modern engines like(like **V8** in Chrome/Node.js, **SpiderMonkey** in Firefox) it mix between interpretation and compilation
	1. first the browser interpret the code normally but if it found `hote code` *repeated part of code* 
	2. so i will use `just-in-time compiler (JIt compiler)` to compile this part to machine code, so it will be fast to use than interpreting



# Python notes:

1. `variable_name.encode(”encoding type”)` → that’s return bytes object
    1. so any encoding conversions with python, you must use it and save it’s value in a variable which you will manipulate
    2. variable.enocde(”ascii”) → important before making any encoding on strings to convert it to ascii codes to manipulate with it.
2. `bytes_object.hex()` → return the hex representation of the sequence
3. `enumerate()` -> to iterate with the index and the value
4.  practice for the code to be safe , reusable
	- files in python can used immediately or as a module
	- `__name__` is defined by the interpreter
	- `if __name__ == "__main__":` 
		- this phrase allow just for the codes inside it to run, if you run it immediately
	- `if __name__ == "__filename__":`
		- this phrase allow just for the codes inside it to run, if you import it as a module
5. 