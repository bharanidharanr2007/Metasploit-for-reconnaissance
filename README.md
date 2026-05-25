# Metasploit-for-reconnaissance
# Metasploit
Metasploit for reconnaissance in pentesting

# AIM:

To get introduced to Metasploit Framework and to  perform reconnaissance  in pentesting .

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode

### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

Find out the ip address of the attackers system
## OUTPUT:
It is a command used to display and configure network interfaces, such as showing the IP address, MAC address, and status (UP/DOWN) of your system
<img width="752" height="355" alt="Screenshot 2026-05-17 131444" src="https://github.com/user-attachments/assets/7e5c23db-c06f-44b4-abfd-93d4a2cf227c" />



Invoke msfconsole:
## OUTPUT:
It is used to start the Metasploit Framework console, where you can run commands for scanning, exploitation, and penetration testing.
<img width="899" height="719" alt="Screenshot 2026-05-17 124517" src="https://github.com/user-attachments/assets/5aca3b61-78d2-4c04-8193-92bf642b28a4" />


Type help or a question mark "?" to see the list of all available commands you can use inside msfconsole.

It is used to display the list of available commands in msfconsole along with their descriptions.
<img width="942" height="914" alt="Screenshot 2026-05-17 124647" src="https://github.com/user-attachments/assets/cc2895ef-92bb-4cba-a7bb-5c13fcd9d40e" />


Port Scanning:
Following command is executed for scanning the systems on our local area network with a TCP scan (-sT) looking for open ports between 1 and 1000 (-p1-1000).
msf >  nmap -sT 192.168.1810/24 -p1-1000  (Replace with appropriate IP Address)
## OUTPUT:

It is the process of checking a system or network for open ports to identify active services and potential vulnerabilities.
<img width="656" height="431" alt="Screenshot 2026-05-17 125632" src="https://github.com/user-attachments/assets/499d51c7-862d-4d39-9bdd-d0eb3b8094c0" />

step4:
use the db-nmap command to scan and save the results into Metasploit's postgresql attached database. In that way, you can use those results in the exploitation stage later.


scan the targets with the command db_nmap as follows.
msf > db_nmap 192.168.181.0/24
## OUTPUT:
It is used to run an Nmap scan and store the results in the Metasploit database for later use.
<img width="691" height="264" alt="Screenshot 2026-05-17 125919" src="https://github.com/user-attachments/assets/fd37da8c-e0e2-4ab7-8ae3-91d6df7f371a" />

 a multitude of scanning modules built in. If we open another terminal, we can navigate to Metasploit's auxiliary modules and list all the scanner modules.
cd /usr/share /metasploit-framework/modules/auxiliary
kali > ls -l
## OUTPUT:
It is used to list files and directories in long format, showing details like permissions, owner, size, and modification date.
<img width="534" height="258" alt="Screenshot 2026-05-17 130009" src="https://github.com/user-attachments/assets/7c4f6b7b-1854-4af9-938c-c2063352cece" />


Search is a powerful command in Metasploit that you can use to find what you want to locate. 
msf >search name:Microsoft type:exploit
## OUTPUT:
It is used to search for exploit modules related to Microsoft in Metasploit.
<img width="957" height="929" alt="Screenshot 2026-05-17 130817" src="https://github.com/user-attachments/assets/2d1366c2-bd12-4a48-a8a6-bb0c4f64ade3" />



The info command provides information regarding a module or platform,

Before beginning, set up the Metasploit database by starting the PostgreSQL server and initialize msfconsole database as follows:
systemctl start postgresql
msfdb init
## OUTPUT:
It is used to initialize and set up the Metasploit database for storing scan results and data
<img width="1016" height="713" alt="image" src="https://github.com/user-attachments/assets/4dbf1701-423e-4147-8330-5ff626b9ac8a" />



## MYSQL ENUMERATION
Find the IP address of the Metasploitable machine first. Then, use the db_nmap command in msfconsole with Nmap flags to scan the MySQL database at 3306 port.
db_nmap -sV -sC -p 3306 <metasploitable_ip_address>

## OUTPUT:
It is used to scan port 3306 (MySQL) on the target, detect service version and run default scripts, while saving the results in the Metasploit database.
<img width="870" height="145" alt="Screenshot 2026-05-17 131204" src="https://github.com/user-attachments/assets/24272593-aa6f-4476-af58-8f5f3a4ddb16" />


Use the search option to look for an auxiliary module to scan and enumerate the MySQL database.
search type:auxiliary mysql
## OUTPUT:

It is used to find all auxiliary modules related to MySQL in Metasploit.
<img width="954" height="931" alt="Screenshot 2026-05-17 130133" src="https://github.com/user-attachments/assets/303c0510-9f78-400e-98b9-5e986f59f3fc" />



use the auxiliary/scanner/mysql/mysql_version module by typing the module name or associated number to scan MySQL version details.
use 11
Or:
use auxiliary/scanner/mysql/mysql_version
## OUTPUT:
It is used to select the MySQL version scanning module in Metasploit to identify the MySQL service version on a target.
<img width="869" height="169" alt="image" src="https://github.com/user-attachments/assets/bb4b22c9-e720-4423-9aee-f368f82e9d50" />

After scanning, you can also brute force MySQL root account via Metasploit's auxiliary(scanner/mysql/mysql_login) module.
## OUTPUT:
It is used to attempt login to the MySQL root account by trying multiple username-password combinations using Metasploit’s auxiliary/scanner/mysql/mysql_login module.
<img width="960" height="337" alt="image" src="https://github.com/user-attachments/assets/0d895d4d-0445-40f3-9f1f-307a54739520" />



set the PASS_FILE parameter to the wordlist path available inside /usr/share/wordlists:
set PASS_FILE /usr/share/wordlistss/rockyou.txt
Then, specify the IP address of the target machine with the RHOSTS command.
set RHOSTS <metasploitable-ip-address>
Set BLANK_PASSWORDS to true in case there is no password set for the root account.
set BLANK_PASSWORDS true
## OUTPUT:
Commands explanation (short answer):

set PASS_FILE /usr/share/wordlists/rockyou.txt
Sets the password wordlist to try during brute force
set RHOSTS <target_ip>
Specifies the target system IP address
set BLANK_PASSWORDS true
Allows testing login with empty passwords
<img width="960" height="337" alt="image" src="https://github.com/user-attachments/assets/b05049af-5f55-45d2-9e7d-8c5c2ed7cc04" />

## RESULT:
The Metasploit framework for reconnaissance is  examined successfully
