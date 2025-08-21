# Linux

# General Knowledge :

- There is many UNIX-like operating systems and called like this due to it follows the principles and conventions and standard which Unix built on by Bill Labs , and it’s not necessarily by certified by Bill Labs , Key characteristics of UNIX is :
    - multi-tasking
    - multiple users
    - Hierarchical file system
    - shell and command line
    - POSIX complaint
    - modular structure
    
    the OSs may be certified like “macOS” or not like “Linux , android , …..”
    
- GNU stands for “Not Unix” it was project directed by Richard Stallman (1983), to be free open-source operating systems , and it nearly like that (it provide multiple tools like shell , compilers , debuggers , libraries ,.. and so on) but without the Kernel due to the project does not completed(the kernel was supposed to call Hurd) , So when Linus completed Linux kernel , developers combine between the Linux and GNU tools , to build Linux distros (when we say Linux now we mean GNU/Linux)
    - the Kernel is the Middleman between the hardware and user-level programs , it talk with the hardware and manage system resources like (memory, processes, I/O)

---

# Important Notes:-

- “symbolic” or soft link in “ln” tool , make the new file indicate to the original file not the data so if you deleted the original one the copy also will be inaccessible
- which → return to you the path of the tool
- wc → return data about the content of file
- The STDIN just work with programs , but it does not work with shell built-in commands like “cd” , but it can work with the built-in if it accepts characters
- If i passed a file using STDIN with a command expects a file as input , it will take the file itself.
- STDIN can pass the characters but not special ones like (arguments , Meta Data , Paths , variables) , you can use it as input for scripts
- a lot of built- in commands read from environment variables to provide you the info like {pwd , hostname ,…}
- su & sudo
    - su (substitute user) used to change the user to another one , asking for the password of the account that you want access (by default it change to root)
    - it’s best practice to access on another user by adding dash after the command like this “su - $userName” , it is load the environment variables of the new user , and put you in the home path of the new user , the previous command will change the user but will keep the environments variables for the last user and you will still in the home of the last user which will make you unauthorized to do or edit any thing (you can overcome this by using “sudo” with every command) , so you should use “su - $userName”
    - sudo (Superuer do) it gives you permissions of another user (root by default) to do a command  , but your user name should be in “/etc/sudoers” file which contain the users who can use sudo , and because you are authenticated in the sudoers file , it will just ask you for your current password not the password of the user which you try to access its permissions
    - in the new Debian versions like (ubuntu , kali ,….) , root account looked out by default so it does not has a password and you can not access it directly , you can use this command “sudo su” , which let you as “sudo”  have root permissions without be root , but it just give you the shell it will not load (Environment variables) or put you on home directory , so you can use “sudo -i” and it do all the mentioned above and it better than write “sudo su -”
    - “su username -c “command” ”  it let you perform a command as another user but without switching to him
    - `echo “password” | sudo -S <rest of command>` → this command pass the password to sudo without asking you “-S” let you pass the password as input file or string rather than standard input device
- there is difference between the service and the application , application is interactive program needs from user manipulate with them like games , browsers , media players,….etc. but the service is non-interactive program , it’s running in the background and does not need any manipulation from the user.
- install software
    - the company of the package manager connects it with centralized server or multiple servers  called (mirror) that’s mirrors contain the packages which aggregated from another servers and repos , and provide it to the tool
    - when you tell to  tool “update” , it’s go to the mirror and updates the program list which in your os about the new additions on the mirrors , and when you use “upgrade” it’s starting to install these packages from mirrors
    - “apt-get” & “apt-cache” is old versions , and compatible with old versions Debain_Based systems , “apt” is new and better to use
    - “dist-upgrade”== “full-upgrade” after updating , it’s upgrade the all things in distribution for you
    - if you did not find the software which you want  in the mirrors , you can download the .deb one from the original site , and installing it with dpkg
    - any thing installed by apt “just apt remove it” , and anything installed by dpkg “just dpkg remove it”
- GNOME for “GNU Network Object Model Environment” , it’s desktop environment which provide
    - The graphical interface like {windows , icons , menus , panels}
    - some applications like {file manager , text editor , terminal}
    
    it’s supported by many Linux distros and UNIX-like systems , and it’s come preinstalled on Ubuntu
    
    and it’s clean and modern and simple , and it use GTK as toolkit to build the interfaces of the apps , because that it be consistent.
    
- there is many desktop environments (DEs) like  KDE , Xfce (default for kali) , and more

- there is also something called Display Manager , it’s the login screen which you encounter when the graphical interfaces loaded , it’s very important and responsible about {starting X server or Wayland session , and it let you determine which desktop environment to use if you have multiple ones , and it authenticates you}
    - { X server and Wayland } these are responsible about drawing the graphics and so on , X server or X11 is old(1980s) and heavy it was calling with the apps (compatible with old apps) , Wayland is new (2010s) and fast and light and calling with specific components in the desktop environments , you choose from display manager
    - gdm3 → GNOME
    - lightdm → for multiple ones like Xfce , KDE , MATE,….
- “package is held-back” mean it will not be upgrading when you run “sudo apt upgrade” , because upgrade command is very conversative it does not install any dependencies it just upgrade the tool and if any tool which has upgrade but also it’s need another dependencies the system will hold it back , or you can hold any package or tool you want by `sudo apt-mark hold <name>`, and to can upgrade every thing even the held-back packages you need to use stronger command like “sudo apt full-upgrade” it’s similar to `sudo apt install distro-upgrade`

---


- * the options which you pass with the commands called switch or argument
- * autoremove command delete the package and its dependencies
- `locate <filename>` → to search in the whole OS about specific file
- * recursively → meaning doing the act again and again on the sub-items in the item.
- curl → tool to send requests with manipulating with various of protocols , does not support download recursively
    - examples
        - `curl https://Google.com`  // get request , display the indx.html in the terminal
        - `curl -X POST -d “user=admin” https://yahoo.com` //post request
        - `curl -H “Agent-OS : Linux” https://yahoo.com` // adding headers
- wget → also this tool to send requests but it mostly use web protocols (http , https , ftp) , it mainly designed for downloading files and websites
    - examples
        - `wget https://Google.com` //download index file
        - `wget https://yahoo.com/file.zip -O <new name>` //downloading this file with the new name
        - `wget -r https://cyber.com` // download the whole web site recursively
- `tail -f <path>` //show you all things added to that file like a live show
- /var/log/apache2/access.log → here saved all info about who accessed the server and some info about his connection
- *glob patterns & regex
    - when you write “ ls *.txt “ the * here considered glob patterns(used by the shell) not regex and it means match any character zero or more
    - “*.txt” called shell wildcard , the shell expand it and pass its result to the tool like ls
    - but when you use “*” in regex you should specify before it the character which should appear zero or more
    - “+” one or more
    - you can write the ranges in the same brackets like `[A-Za-z0-9]` → these are all alphanumeric characters
- -o in grep print just the pattern which you want , and ignore the other data in the line
- grep also can be has a recursive mode ‘-r’ it search in every files and subdirectory in the path which you pass to it
- -u in sort like usig uniq after sort
- `tee <file name>` rather than STDOUT to specific file , it displays the output in the terminal and save it in the file “both”
- echo -e → enable escape characters
- * cut → extract specific section of data from a file
    - cut -c1-5 file.txt //from first to fifth char in the file
    - cut -d “:” -f2 //delimiter the lines by : and return the second column (field)
- *awk → tool for manipulation with files  especially with records , fields files format
    - the default delimiter is blank
        - `-F <new one>` → to change the delimiter
    - $1 , $2 , …. → is the fields and $0 is the whole line
    - `awk ‘pattern {action}’ <file>`
        - pattern → can be a condition for the action and if it missed the action will run on all lines
        - action → is the output which will appear or the process which happened , print all line if missed
        - END {} → run after {action}
    - examples:
        - awk ‘$2 > 50 {print $0}’ file.log
        - awk -F‘,’ ‘{print $2}’ file.log
        - awk -v variabliename=”$variable preassigned in the terminal” ‘pattern {action}’
        - awk ‘{sum+=$3} END {print sum}’ file.log
            - the variables does not need declaration , zero by default
            - initialize it with another value if you need
- /sbin/init → is the first process run after the booting process and (after the Kernel initialize the hardware and memory) in traditional linux systems
    - it is responsible of running the start ups scripts and set up environment and it monitors and manages the child process, it is the root of all processes
    - if this process killed the system may crash , due to it is essentials for system managements
    - in the modern systems, it replaced by “systemd” (daemon → hidden soul or hidden server)
    - you can control and manage it using systemctl command
- df → like disk management in windows
    - we using -h → to clarify the output, it will demonstrates the size and other info in clear manner, not just some confusing numbers
- top → like task manager, display the process and apps that run and its usage of memory
    - htop → better version
- apt list —upgradeable → packages which need upgrade
- apt list —installed → packages which installed by apt
- look this image ❤️
![[image.png]]
- use (ps aux —forest or pstree or pstree -p (more detailed than pstree)) → to display all sub processes

![[image2.png]]

- `ssh username@ip_or_domainName` → secure shell is a tool to access on servers or machines remotely, then you will be asked for the password
    - `ssh -p <port> username@ip_or_domainName` → specify port
    - to copy files to the server → scp file.txt username@ip_or_domainName:path in the server
- linux has text editor called mousepad
- -f → always used to ignore any asking for making sure it’s mean force
- when you use locate, it does not search in the operating systems, it search in pre-built data base, which contain all paths for your files
    
 - so use updatedb → to update this data base frequently
 
- `dhclient <particular NIC>` → searching about the DHCP around the network (broadcast) and asking it for IP
    - `dhclient <particular NIC> -v` → to see the process
    - valuable if the internet connection stopped suddenly and you want to reconnect , or you want change you IP address , or you want connect internet and there is no automatic configuration to connect you, and more….
- built-in linux firewall is iptables
- $(command) → this notation run the command in a subshell and return the output as input in the position that used in
- uptime → the operating period
- kill -9 PID → to kill
- `chown user:group file` → to change the owner
- `alias aliasName=’original’`
- (shellType) -c “the command” → run in the subshell and run the command in it
- `(terminal name) <optoins>` → open another terminal and you can run specific command in it
    - xfce4-terminal -c “bash -c ‘exec bash’ ” → for opening a window and you can exit it normally
    - xfce4-terminal  —hold -c “whoami” → for opening the window and run the command and hold it
        - withoud hold it will auto close after the command ran
---
