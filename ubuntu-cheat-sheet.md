# Cheat Sheet of Ubuntu Attack Path
### A suggested attack path is provided, but there will be other routes of attack on each machine. Use these suggestions if you get stuck. 
1. Run an nmap scan on 10.20.30.42. 
    - `nmap [-p/-sC/-sV/] 10.20.30.42 -Pn`
2. Enumerate on the 80 and 443 with Nikto or any other web enumeration tool. Browse to the webpage on the browser to seek additional information. 
    - `nikto -h [HOST]` 
    - `gobuster dir -u [http(s)://]10.20.30.42:[PORT]`
    - `dirb` or `dirbuster` can be used also 
    - **Very important to grab the users we see. The meet the team page is brimming with users. We also see a user on the wordpress with the username `adebois` making posts. Keep this in your notes!**
3. Run wp-scan, a wordpress scanner, to discover what is currently running on the webpage and any attacks it can gather from /wp-config.php
    - `wpscan --url [http(s)://]10.20.30.42`
4. Using CEWL, grab all words on the webpage as a dictionary attack. 
    - `cewl [http(s)://]10.20.30.42`
5. Brute force the password for the /wp-admin panel
    - Using Burpsuite:
        - Navigate to the webpage using Burp as a proxy by enabling FoxyProxy in the Firefox Browser. 
        - Use the webpage under /wp-admin to login with false credentials
        - Find the POST request within the Proxy > HTTP Proxy buttons within Burpsuite.
        - Right-click the request and select CTRL + I or Send to Intruder. 
        - Ensure the Attack Target is properly configured with the information of the request. 
        - Clear the payload positions. And change the payload positions to only consist of the username and the password fields. 
        -  Use the username we found earlier as the username as it is the only username confirmed until now. 
        - Under the payloads tab, import the list of potential passwords scraped from `cewl` as a single payload *simple list*.
        - Run the attack! 
        - Your results should show status code 200 to every potential password but ONE: **easypeasy**
        - From here you can select the correct password, right-click and open in the browser to further view the attack surface (WordPress). 
6. Further enumeration will lead to a vulnerable plugin being present. PlainView Activity Monitor, with an active exploit.
    - use `searchsploit Plainview Activity Monitor` to find a metasploit module or a python code to run to attempt exploitation. 
7. Exploitation can be use with Metasploit Framework
    - In the command line, write `msfconsole` to open metasploit
    - `search Plainview` to find the exact exploit and use the one found from `searchsploit` by typing `use [#]` 
8. Fill in the details for the exploit module on MSF, locate the options needed with `show options` or `options`
    - This includes LHOST, RHOST, RPORT, LPORT, **SSL**, username, and password
9. A connection is made to Ubuntu as the WWW-DATA user, which is a low privileged user.
    - We can begin by installing linpeas on this machine, by setting up a python host server on our Kali Machine and having the www-data user grab this information. 
    - To set up a python listener for your victim to grab information from, `python3 -m http.server PORT` *note: if you choose a port that is relatively low, you need to use sudo*
    - Manual Enumeration can be done also. I recommend [HackTricks](https://book.hacktricks.xyz/linux-unix/privilege-escalation) 
12. If manual enumeration is sufficient, a user will discover a cron job running every 5 minutes as the user 'adebois' called cleanup.sh located in the /home/adebois/Work-Scripts folder, which the www-data user has access to see. 
    - Cronjobs can be found a multitude of ways but we can use commands like `cat /var/crontab` or `crontab -e`
12. Exploitation of the cron job running every 5 minutes
    - A simple callback to your kali attacker machine via /bin/bash can solve this. Make sure to open a listener waiting for the connection after 5 minutes to get the shell. Use `netcat`, `netcat -nvlp [port]`. [Here](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) are some suggestions for reverse shell one liners. 
13. Search for your flag and continue onwards to the 10.11.12.0/24 network, but do not scan 10.11.12.1-2!
