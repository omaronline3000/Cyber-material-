- there is a feature in the `alg` field in the header of the token
	- if you assigned it to `none`, even the tokens with empty signature will be considered valid
	- this feature created for debugging process and should turned off, when the project get production
- there are many `Risks`:
	- Manipulating `alg` field
	- Brute-Forcing the key


- it saved in `LocalStorage` or`sessionStorage`
	- if it saved in `LocalStorage` -> so it will be saved for specific time (persistent)
	- and if it saved in `sessionStorage` -> it will be deleted when you close the tab
