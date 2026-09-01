

# Bash Script

- when you write bash file use the `shebang -> #!` to specify for the shell what is the type of this file
	- it will make the file executable even if you did not write `.sh` extension

### Variables :

- to set a variable just write its name and its value `var=value`
	- *do not add any whitespace 'error'* 
- and to unset a one use `unset -v 'var'` 
- arguments ->  
	![[Screenshot 2025-09-08 222735.png]]
	- when you write `echo $0` in the terminal you will got the shell type, because it's the script which you work on
### Additions
- command substitution :
	- `$()` explained in Linux part
	- `backtiks`
		- shell=`env | grep "SHELL" `

- `sleep 'time in seconds'`  
- `export` 
	- when you just define variable in the command line `var='hello'` this is local to this session on the shell , if you opened new session on this shell , or even changed the shell , the variable will be undefined
	- when you use `export var='hello'` , it define it as `environment variable` so it's global variable across your current session on the whole operating system, so if you changed any thing the variable still valid while you in the current session on the OS




---
