# Forks and Knives - Hack The Box Challenge

**Contributors**:  
- Kavya Sree Chandhi  
- Venkata Sri Sai Gaman Yerramneni  
- Tejaswi Reddyvallapu  

---

## Table of Contents

- [Overview](#overview)
- [Tools Used](#tools-used)
- [Setup Instructions](#setup-instructions)
  - [Docker Environment Setup](#docker-environment-setup)
  - [VPN Connection](#vpn-connection)
- [Vulnerability Discovery](#vulnerability-discovery)
- [Attack Strategy](#attack-strategy)
  - [Stack Canary Brute-Forcing](#1-stack-canary-brute-forcing)
  - [Format String Exploit](#2-format-string-exploit)
  - [ROP Chain Construction](#3-rop-chain-construction)
- [Final Exploitation](#final-exploitation)
- [Summary of Exploitation Steps](#summary-of-exploitation-steps)
- [Key Cybersecurity Concepts](#key-cybersecurity-concepts)
- [Important Files](#important-files)
- [Screenshots](#screenshots)
- [Thank You](#thank-you)

---

## Overview

This project documents the complete exploitation process of the **Forks and Knives** Hack The Box (HTB) pwn challenge.  
We successfully performed a full end-to-end attack involving **stack canary brute-forcing**, **format string vulnerability exploitation**, and **ROP chain construction** to gain remote shell access and capture the flag.

---

## Tools Used

- **Pwntools** — for scripting the exploit and automating the attack steps
- **Netcat (nc)** — for manual interaction and testing
- **Docker** — for building a local environment based on Ubuntu 22.04
- **Python 3** — for scripting and exploitation
- **Local libc.so.6** — to resolve function offsets and construct accurate ROP chains

---

## Setup Instructions

### Docker Environment Setup

- Built a Docker image named `pwn_forks_and_knives` using Ubuntu 22.04.
- Created a user `ctf` and copied challenge files to `/home/ctf/`.
- Set permissions for the server binary.
- Verified the build with `docker images`.

### VPN Connection

- Connected to the Hack The Box environment using OpenVPN.
- Ensured access to the remote challenge server.

---

## Vulnerability Discovery

- **Buffer Overflow**:
  - Manual testing revealed that sending 255 'A' characters caused a crash, indicating a stack-based buffer overflow vulnerability.
  
- **Stack Canary**:
  - Observed that overflowing beyond 255 bytes caused immediate crashes, confirming the presence of a stack canary protection.

---

## Attack Strategy

### 1. Stack Canary Brute-Forcing

- Sent payloads byte-by-byte after 255 'A's to determine the correct canary value without crashing the server.
- Reconstructed the full 8-byte stack canary through trial and error.

### 2. Format String Exploit

- Leveraged `%2$p` format specifier to leak a memory address from the stack.
- Subtracted known offset (`lseek64+11`) to calculate the base address of `libc`.

### 3. ROP Chain Construction

- Built a ROP chain to:
  - Redirect socket file descriptors to stdin, stdout, and stderr using `dup2()`.
  - Execute a shell by calling `system("/bin/sh")`.

---

## Final Exploitation

- Successfully bypassed stack canary.
- Leaked the `libc` base address.
- Sent final payload crafted with padding, canary, fake RBP, and ROP chain.
- Gained a remote interactive shell with `io.interactive()`.
- Captured the flag.

---

## Summary of Exploitation Steps

1. Set up the Docker environment for local testing.
2. Connected to the live Hack The Box server over VPN.
3. Performed manual buffer overflow and format string testing.
4. Wrote an automated exploit script using Pwntools.
5. Brute-forced the stack canary byte-by-byte.
6. Leaked a libc address to calculate base address.
7. Constructed a working ROP chain.
8. Achieved remote code execution and captured the flag.

---

## Key Cybersecurity Concepts

- **Buffer Overflow Attack** — Exploited to overwrite stack memory.
- **Stack Canary Bypass** — Required brute-forcing correct canary value.
- **Return Oriented Programming (ROP)** — Chained existing code gadgets for execution.
- **Format String Vulnerability** — Leaked memory addresses for ASLR bypass.
- **Address Space Layout Randomization (ASLR) Bypass** — Overcame randomized memory layouts.

---

## Important Files

- `exploit.py` — Full exploitation script automating the attack.
- `Grp9.pptx` — Presentation.
- `Forks and Knives.zip` - Challenge file.
- `flag.txt` — Flag captured.

---

## Screenshots
![image](https://github.com/user-attachments/assets/08915a8c-8309-4e4c-ba86-85dc02387a70)
![image](https://github.com/user-attachments/assets/30b24217-9540-4903-8ba8-8d4c72be4df2)
![image](https://github.com/user-attachments/assets/d714c998-0c37-43b1-aedd-7a58b6168eab)
![image](https://github.com/user-attachments/assets/ecf332bd-4090-4482-9139-08f699f16f3a)
![image](https://github.com/user-attachments/assets/61ddf042-a6f8-4be6-b73d-b9e6b3be8789)
![image](https://github.com/user-attachments/assets/705e01dc-d016-4512-b097-f25729a47da1)
![image](https://github.com/user-attachments/assets/21c4ef1a-23b7-42ff-b16d-a2cdee95fc70)
![image](https://github.com/user-attachments/assets/b4dab886-aa71-41c3-ad63-ba88f3c7906d)
![image](https://github.com/user-attachments/assets/606dcbbd-8c0a-4984-8212-429d0c8ea37d)
![image](https://github.com/user-attachments/assets/abd60d89-cde7-425d-a825-ef1e93e348d7)
![image](https://github.com/user-attachments/assets/492eaa8f-f541-43ae-b171-87278cf81179)
![image](https://github.com/user-attachments/assets/81e54fe6-8f71-49e1-aba9-37887e939973)













---

# Thank You!

---
