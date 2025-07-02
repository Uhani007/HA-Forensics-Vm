# 🕵️ HA: Forensics - VulnHub Walkthrough

This repository contains my complete forensic analysis of the **HA: Forensics** machine from VulnHub. This is a Capture-The-Flag (CTF) style lab with a forensic focus, consisting of **four flags** to be discovered through disk and memory analysis, steganography, privilege escalation, and more.

---

## 📌 Machine Info

- **Name:** HA: Forensics
- **Platform:** VulnHub
- **Download:** [VulnHub Entry](https://www.vulnhub.com/entry/ha-forensics,570/)
- **Difficulty:** Intermediate
- **Category:** Forensics, Privilege Escalation, Enumeration

---

## 🎯 Objectives

- Perform initial network scanning
- Analyze image and memory dump files
- Crack password-protected archives
- Explore hidden services via internal routing
- Gain root access through sudo privilege abuse
- Extract and document **4 hidden flags**

---

## 🧰 Tools Used

- `netdiscover`, `nmap`
- `dirb`, `wget`, `unzip`, `crunch`, `fcrackzip`
- `exiftool`, `steghide`
- `gpg`, `john`, `pypykatz`
- `metasploit-framework`
- `Autopsy` (forensic image analysis)
- `base64`, `sudo`, `su`, `bash`

---

## 🧪 Methodology

### 🔹 1. Network Scanning
- Used `netdiscover` to identify VM IP address.
- Scanned with `nmap -A` to discover open ports (22/SSH, 80/HTTP).

### 🔹 2. Web Enumeration & Flag #1
- Found webpage hosting images.
- Bruteforced directories with `dirb`, discovered `/images/`.
- Downloaded `fingerprint.jpg` and analyzed with `exiftool` to find the **first flag**.

### 🔹 3. Flag #2 — PGP & ZIP File
- Discovered `flag.zip` via `.txt` extension in `dirb`.
- Found `clue.txt` containing PGP encrypted message and private key.
- Decrypted the message to find password pattern.
- Created wordlist with `crunch`, cracked `flag.zip` with `fcrackzip`.
- Unzipped to find `flag.pdf` (second flag) and `lsass.DMP`.

### 🔹 4. Flag #3 — Memory Dump + Metasploit
- Parsed `lsass.DMP` using `pypykatz` to extract NTLM hash.
- Cracked hash using `john` to get credentials.
- Logged in via `metasploit`’s `ssh_login`, upgraded to meterpreter.
- Found internal docker IP using `ifconfig`, routed via `autoroute`.
- Discovered FTP on internal host, connected anonymously, downloaded `saboot.001`.

### 🔹 5. Autopsy Image Analysis
- Used `SimpleHTTPServer` to transfer `saboot.001` to local machine.
- Opened image in `Autopsy` GUI and found `flag3.txt` and `creds.txt`.

### 🔹 6. Flag #4 — Privilege Escalation
- Decoded `creds.txt` with `base64` to get password.
- Switched to user `forensics` using `su`, password obtained from decode.
- Checked `sudo -l` — user had ALL permissions.
- Used `sudo bash` to become root.
- Read `/root/root.txt` to capture the **final flag**.

---

## 🏁 Flags Summary

| Flag # | Method | Description |
|--------|--------|-------------|
| Flag 1 | `exiftool` | Found inside fingerprint.jpg metadata |
| Flag 2 | `PGP + ZIP` | Password-protected ZIP, decrypted with wordlist |
| Flag 3 | `FTP + Autopsy` | Discovered in disk image via Autopsy |
| Flag 4 | `Base64 + Sudo` | Privilege escalation using decoded credentials |

---

## 📁 Repository Structure

```bash
HA-Forensics/
├── HA-Forensics-Report.pdf       # Full detailed PDF report
├── README.md                     # This walkthrough file
├── images/                       # fingerprint.jpg, DNA.jpg etc.
├── extracted/                    # Extracted files from ZIP and FTP
├── dump/                         # lsass.DMP memory dump
├── wordlists/                    # Custom crunch dictionary
├── screenshots/                  # Screenshots of every major step
