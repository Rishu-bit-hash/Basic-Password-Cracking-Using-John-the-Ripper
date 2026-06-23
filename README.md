# Basic-Password-Cracking-Using-John-the-Ripper
A security lab using John the Ripper on Kali Linux to extract and crack password hashes from a Metasploitable 2 target. Demonstrates custom wordlist creation and the risks of using default credentials.

## ## Project Overview
This project demonstrates how weak and default configuration credentials pose a major security risk. The objective was to capture an encrypted password hash from a target machine and execute an offline dictionary attack using John the Ripper on the Kali Linux attacker machine to recover the plaintext password.
The activity was performed entirely within an isolated VirtualBox environment to ensure security controls remained within the sandbox lab.

## ## Lab Environment

### ### Attacker Machine
* Kali Linux

### ### Target Machine
* Metasploitable 2
* Oracle VirtualBox

### ### Network Configuration
* Oracle VirtualBox Host-Only / NAT Network
* Subnet: 10.0.2.0/24

### ### IP Addresses
* Kali Linux: 10.0.2.8
* Metasploitable 2: 10.0.2.7

## ## Objectives
The primary objectives of this project were:
* Extract the target user password hash file from the restricted directory.
* Map and save the target credentials on the attacker machine.
* Troubleshoot default wordlist limitations by building a targeted text file.
* Use John the Ripper to successfully crack the hashed credential.

## ## Tools Used
```bash
- Kali Linux
- Metasploitable 2
- John the Ripper (v1.9.0)
- Linux Nano / Terminal Echo

```
## ## Cracking Methodology
### ### Step 1 – Hash Extraction
Logged into the target machine and read the secure user shadow database file to extract the raw cryptographic password hash string for the msfadmin account.
```bash
sudo cat /etc/shadow

```
> <img width="1917" height="795" alt="01_Hash Extraction" src="https://github.com/user-attachments/assets/00f32e51-e1a3-409a-afec-45aceb040a66" />

>
### ### Step 2 – Dictionary Synthesis
The target hash line was saved on Kali Linux as project_hash.txt. Because standard large public password files (like rockyou.txt) failed to find the system-specific default word, a micro-targeted wordlist was created via the command line to test common guesses:
```bash
echo -e "password123\nqwerty\nmsfadmin\nletmein1" > customm_list.txt

```
> <img width="1917" height="760" alt="02_customm_wordlist txt" src="https://github.com/user-attachments/assets/1b6a05fe-3f1b-4515-b69d-67ffc3cb66fa" />

>
### ### Step 3 – Offline Attack Execution
Launched John the Ripper pointing specifically to the custom dictionary file against the target hash record to compute and find a mathematical fingerprint match.
```bash
john --wordlist=customm_list.txt project_hash.txt

```
> <img width="1917" height="932" alt="03_password_extraction" src="https://github.com/user-attachments/assets/b16a9e10-d5b2-4264-a9c8-0fcfd706570f" />

>
### ### Step 4 – Credential Verification
Ran the final confirmation string to read the internal session database memory and display the cracked results cleanly on the terminal screen.
```bash
john --show project_hash.txt

```
> <img width="711" height="107" alt="04_final_verification" src="https://github.com/user-attachments/assets/12657dd3-ca28-4695-a225-fcd93273adc0" />

>
## ## Result

* **Target Account:** msfadmin
* **Hash Type Identified:** md5crypt (MD5-based crypt)
* **Cracked Plaintext Password:** msfadmin
* **Status:** 1 password hash cracked successfully, 0 remaining.

## ## Challenges Faced & Troubleshooting
* **Missing Software Packages:** The core installation image did not have the cracker pre-loaded. This was resolved using sudo apt install john.
* **Compressed Wordlists:** Standard index tables were found in an unreadable zipped file wrapper. Navigated to the folder path and executed sudo gunzip rockyou.txt.gz to expand the references.
* **Syntax Typing Errors:** Deployed quick terminal line redirection commands (echo >) to bypass word extension text mismatches (like .tx) and text editor file path typos.

## ## Conclusion
This project successfully demonstrates that default setup credentials like msfadmin:msfadmin are highly vulnerable to basic dictionary attacks. Even though the Linux environment securely scrambles the credentials into a cryptographic hash, the system can be cracked instantly if the password text itself is simple. The lab highlights the critical need to change all system default passwords to complex strings immediately upon installation.



