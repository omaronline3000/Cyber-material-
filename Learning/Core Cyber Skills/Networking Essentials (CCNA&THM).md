
# Try Hack Me

- the **Internet** in the beginning was was a project funded by united states defense department and it called `ARPANET` since 1960s
- the creator of the *world wide web(www)* was **Tim Berners-Lee**


# CCNA crash course and some N+ and ChatGPT Notes

==**Essentials Topics**== -> `OSI Model , TCP/IP , TCP & UDP, ARP, Routing protocols, Routed Protocols , Mac Address , IPv4 , subnet mask , subnetting , private/public IP, Static NAT, Dynamic NAT, PAT(NAT overloaded), Router, Switch, Firewall, IDS/IPS, Home Router, Modem, NIC, Dial-up,DSL, Antenna,ESA,LAN, WAN, unicaste, multicaste, broadcaste, DHCP, Default Gateway, interface & ports, ARP`  

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
6. *Private & Public* IPs -> the total number of IPs which I can get from IPv4 is `2^32 = 4,294,967,296` , and it's too little number according to the number of devices which use the internet thesfe days 
	 - so we created `Private IP ` and `Public IP`
		 - Private IP
			 - not routable on the intent 
			 - used in internal LANs
			 -  ranges :
				 - 10.0.0.0\8 (10.0.0.0 - 10.255.255.255)
				 - 172.16.0.0\12 (172.16.0.0 - 172.31.255.255)
				 - 192.168.0.0\16 (192.168.0.0 - 192.168.255.255)
			- the subnet mask of the ranges also may change 

		- Public IP
			- routable on the internet
			- used to connect on the WANs (like the Internet)
			- allocated and managed by IANA
7. NAT -> has many types the static and dynamic and pat is just the main ones , but every one of these have many types under it 
	1. home router running as PAT
		1. it does not layer 4 device , it just layer 3 and PAT give it some functionality of layer 4
	2. firewalls and load balancer are layer 4 devices and sometimes maybe layer 7 devices
	3. in pat the router give external port instead of the internal one which you send with , due to the possibility of two different devices send the request with the same Port number, so it gives you unique external port
8. Router -> work on layer 3 and edit in layer 2
	1. every interface other than Mac address to forward the packets, it needs IP address to be identifiable in the network which it belongs to, and to give the router the ability to use the routing protocols and build the routing table and to determine the best path to every packet, and then the router can change the Mac address, and forward your request
9. Switch -> work on layer 2
	1. the only layer 2 technology which we use until now is Ethernet
	2. it's connected to all devices in the network even the router
	3. there is MLS (multi-layer switch)
		1. it works as switch and router in the same time, do job of both
		2. operate on layer 2 and 3

10. Firewall -> work in layer  3 and 4
	1. NGFW -> work also on layer 7
	2. firewall protect the network by specify specific IPs can call specific IPs on specific Ports
	3. it contains thing called `Policy Table` that consists of `src IP` and `des IP` and `Port`, written in it what should pass and what shouldn't.
	4. if your request passed, the firewall give it flag called `statefully`, so the response of this request will pass without any analysis from the firewall.
	
11. IDS & IPS -> usually used together
	1. IDS -> make passive monitoring , just detect and alert , does not interfere
	2. IPS -> make active monitoring, if it found any malicious evidence , it block the traffic
	3. it has two types in doing their functions 
		1. signature-based -> just check its data base of the signatures of the famous attacks
		2. behavioral analysis -> it's take a period of time to study the normal connections and traffics, and when it find any abnormal action it interferes
	4. there are two types of devices 
		1. Host-based IDS/IPS -> run layer 7
		2. Network-based IDS/IPS -> run layer 3 and 4
		
12. interface vs physical port -> every port consider interface , but not all interfaces consider ports

	- interface can be physical like (ports) or logical or virtual like (**Wi-Fi radio** *the antenna* , VLANs, VPN tunnels, loopback interfaces)
	- every interface/ports has mac addresses and IP addresses (can be unique or can be bridged like home router the default gateway IP bridged between all ethernet ports and WIFI interface) 
	
13. Home router -> smaller and cheaper than the enterprise one
	1. this is a box contain *(do this functionality)* `Router, Switch, DHCP, NAT, Firewall, Access point (wifi), DNS forwarder, modem`
	
	2. the Modem convert (modulate) the which arrived to the router to format which can sent to the ISP by the medium which connect your router to the ISP (fiber , coax , ...)
		- it receive the converted data from the NIC and convert (modulate) it again to suitable format to send
	
	3. the router has two networks WAN side(which is the interface which connected to the ISP ) and LAN side (which is the LAN Ethernet ports "*each one is an separated interface*" and the WIFI interface `usually bridged IP (default gateway)` )
	
	4. every interface has its own mac address , the WAN (interface connected to ISP) and Ethernet LAN (interface which in physical ports in the router) and WIFI (interface which in the antenna) *one mac per band in WIFI* (usually the IP be bridged between the WIFI and Ethernet LAN *default gateway* )

14. NIC -> network interface card
	1. it's like modem convert the data from format to format
	2. it works on layer 2 and layer 1 *(Data link , physical)* , it takes the packets and wrap it with headers, and make it frames (layer 2) , then it encodes every frame into suitable format to transmit (electric signals , light pulses, ....)
		1. destination address written before the source
	3. the mac address belongs to it
	
15. Dial-up & DSL
	1. Dial-up -> old technology (internet or phone calls ) on line to both
		1. was a cable connected to the computer
		2. work on PPP session established by ISP
		3. there is no router device 
		4. ISP gives you public IP directly
	2. DSL -> our technology today (internet and phone calls) the both
		1. there is a router
		2. and you know the other thigs -> go to Home router section
16. Antenna -> device send and receive Electromagnetic waves like radio waves
	1. convert electrical signals to radio waves and vice versa 
	2. the digital data in router *(yes the router save digital data due to it contains cpu and ram and so on)* -> NIC/wifi chip convert it to electrical signals -> antenna convert this signals to radio waves
	3. the antenna receive radio waves convert it to electrical signals -> NIC/wifi chip convert it to digital data -> the router receive the data
	4. types of it
		- omnidirectional -> send everywhere around it (360) (home router)
		- directional -> specify one direction to send (long-range transmitting)
		- internal -> built inside AP case (shorter-range)
		- external -> stronger than internal (stronger) , can be adjusted or replaced
	5. radio waves in home router is -> WIFI signals 
17. ESA (Extended Service Area) -> you connect many AP in some place to cover greater geographical area
	1. like when you put two routers in your home with the same SSID , so if you get out of range of some one ,and you was in the range of the second one you will connect directly to it
18. TCP & UDP 
	1. TCP
		- reliable -> making (error detection/correction)
		- ordered -> sequencing the segments 
		- slower -> because it's reliability and **control flow-window size**
	2. UDP
		- unreliable -> just send the data, does not ensure the transmission
		- unordered -> just send the data
		- faster -> because it does not do any features like TCP
19. LAN
	- scope : small areas like in one building, office,....
	- speed: high speed, due to the transfer in short distance 
	- uses: connect computers within office, department,......
	- 1G ~ 100G
20. WAN
	- scope: big geographical areas, maybe worldwide
	- speed: slower than LAN, due to it connect big areas, and transfer across many networks
	- uses: use to connect separated places and LANs to each other across big geographical areas, may cover worldwide 
	- in Megabytes
21. unicast
	- one-to-one -> sending from specific source device *specific source IP* to specific destination device *specific destination IP*
22. broadcast
	- one-to-all -> sending from specific source device *specific source IP* to broad cast IP which exist in all devices in the network
	- also mac address has a broadcast domain `FF:FF:FF:FF:FF:FF`  (ARP message)
23. multicast
	- one-to-many -> sending from specific source device *specific source IP* to specific destination devices *specific destination IPs*
- every connected device has (unicast address, broadcast address, multicast address)
	- the multicast being a range of IPs to send to them, taken when you connect with them in specific thing like group or online game, and you should send to only them and all of them
23. to get IP address on your computer, you can:
	1. get it static (write it manually in the settings)
	2. get it dynamic from **DHCP** server
24. DHCP server
	- layer 7 protocol , use UDP
	- when you configure your computer to take the IP by DHCP, it sends a broad cast message to find it called `DHCP Discovery` 
	- the DHCP server in your `Home Router` respond with `DHCP offer` which contain the *IP and Default Gateway IP and DNS IP* , then your computer send `DHCP Accept` , and the server send `DHCP ACK`
25. Default Gateway
	- it's the router interface which connect to your network
	- you send all requests and messages which go out your network to it
	- it takes IP address and has Mac address 
26. ARP protocol -> address resolution protocol
	- when you want to send a message to specific device in your network, or you want send a message out of your network so you want send to the gateway, the packet come in to the layer 2 with the IP destination, but you maybe do not know the mac address for the destination, whether was the gateway or another device in your network
	- every device has a something called ARP table, which contain the IP addresses and its mac addresses which the device manipulate with it before
	- so when you call a new device *device you do not know its mac address*, your device send ARP message *broadcast* with the IP destination and when the message arrive to the desired device it send back its mac address to my device, so the device save it in the ARP table, and send the message to it

27. 