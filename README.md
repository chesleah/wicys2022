# wicys2022
Files and information for WiCyS Breaking In: Smart Home


## 1 Introduction

The hackable smart home network is a vulnerable-by-design network used for penetration testing and testing skillsets related to offensive security. The exercises include basic penetration testing technical skills such as enumeration, exploitation, escalation, and pivoting within a home network. 

## 2 Background
A `user.txt` and `root.txt` are placed strategically in (`/home/[USERID]/`) directory, with the privileged user's root flag in (`/home/[PRIV-USER]/`). The third vulnerable machine has a `user.txt` and `root.txt` strategically placed in `C:\Users\[USERID]\Desktop` for two users. The fourth and final vulnerable machine has a `user.txt` placed in (`/storage/emulated/[USERID]/DCIM/`) and `root.txt` hidden in the [PRIV-USER] (`/data/`).

Gather all flags only if you can. 

## 3 Getting Started 
You are given a single Kali Linux machine. 

|**Login Credentials:**| 
|-------|
|**username: kali** |
|**password: kali** | 

The IP address of Kali Linux is assigned via DHCP, you can retrieve this by using the command `ip a`. You are looking for the eth0 interface, for ethernet. You will need this IP to retain callbacks from the exploits you run. 

## Earning Points 
*Note: You do not need to obtain the `user.txt` or `root.txt` to be successful*

| Machines: | | |
|----|-----|-----|
| Pi, My Guy | Exploitation and Escalation | 20 points |
| Work From Home | Exploitation and Escalation | 30 points |
| For the Win | Exploitation and Escalation | 25 points| 
|The First Andy| Exploitation and Escalation | 25 points|

## 5 Authorized Scope and Excluded Systems and Activities
| Scope|  |
|-----------|-------|
|In-Scope| 10.20.30.42|

You will begin by scanning the 10.20.30.42 IP address, identifying vulnerabilities, and completing exploitation. Once the root flag has been found and submitted, the *`root.txt` document will identify the new scope, with new exclusions*.

**Scanning, Probing, or otherwise testing the following systems is not authorized:**
- 10.20.30.40
- 10.11.12.1
- 10.11.12.2

The Out-Of-Scope IP should not be scanned under any circumstances, it could break the network!

To run an nmap scan with an exclusionary rule `--exclude <host1>[,<host2>[,...]]` to exclude networks or hosts. 

Our command will look like this: 
`nmap [flags] 10.20.30.42 --exclude 10.20.30.40`

## 6 Failed Exploits

It's all good! Raise your hand and we will look into it! 

**One thing that isn't taught in hacking, but rather learned, is learning to fail and be okay with it.** 

## 7 Tools

Primary Tools - Use other tools as necessary 


| Service | Tool|
|--------------------|-----------------------|
|Port Scanning | `nmap`, `autorecon`, `zenmap`|
|Web Application Vulnerabilities | `nikto`, `gobuster`, `dirb`, `dirbuster`, `wp-scan`, `burpsuite`|
|Brute Forcing Cracking Passwords| `cewl`, `john`, `hashcat`, `hydra`, `medusa`|
| Enumeration | `linpeas`, `winpeas`, `linuxprivchecker.py`, `metasploit`
| Finding Exploits | Google Search, `searchsploit`
| Remote Code Execution | python, `netcat`, `adb`

*Any tool is open to use, we just ask that you know how to use it and can use it responsibly!*
