
# Try Hack Me

- the **Internet** in the beginning was was a project funded by united states defense department and it called `ARPANET` since 1960s
- the creator of the *world wide web(www)* was **Tim Berners-Lee**


# CCNA crash course and some N+ Notes

==**Essentials Topics**== -> `OSI Model , TCP/IP , TCP & UDP , ARP , Routing protocols , Routed Protocols , Mac Address , IPv4 , subnet mask , subnetting , private/public IP`  

1. IPv4 -> 32 bit , 4 octet , every octet carry number `0-255` *8 bits*
2. subnet mask divide the IP address to two parts *Network Part & Host Part*
	- it has two three famous forms to represents
		- `/8 or /16 and so on` , its range from 0-32 bits
		- decimal -> `255.0.0.0` or `255.255.0.0`
		- binary -> `11111111.0.0.0` or `11111111.11111111.0.0`
	- and there is classes and that's back to the history IP
		- Class A 1 -172 ->  `/8` 
		- Class B 128 - 191 -> `/16`
		- Class C 192 - 223 -> `/24`
	-   Network address used in Routing tables to choose the best path to your traffic
	-   Host address used in the hosts like computers and servers and laptops to identify it
	-    Broad cast address is an extra , and used by all machines in the network
3. subnetting is slicing a network into many small networks called subnets
	- transmit bits from the host part to the network part and its combination will be the number of gained network
	- then subnet mask will change
	- and you will get many small networks to work on
4. Router does not forward Broad cast messages
5. Access point devices normally in enterprises connected to DHCP server or router
	- but home `router/access point` is different, it contains DHCP server
6. the total number of IPs which I can get from IPv4 is `2^32 = 4,294,967,296` , and it's too little number according to the number of devices which use the internet these days 
	 - so we created `Private IP ` and `Public IP`
		 - Private IP
			 - not routable on the intent 
			 - used in internal LANs
			 -  ranges :
				 - 10.0.0.0\8 (10.0.0.0 - 10.255.255.255)
				 - 172.16.0.0\12 (172.16.0.0 - 172.31.255.255)
				 - 192.168.0.0\16 (192.168.0.0 - 192.168.255.255)


		- Public IP
			- routable on the internet
			- used to connect on the WANs (like the Internet)
			- allocated and managed by IANA
7. 