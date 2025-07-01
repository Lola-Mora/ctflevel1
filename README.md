# My First CTF Experience – Masterschool CTF Level One

Recently, I completed my first Capture The Flag (CTF) challenge, which was hosted through a TryHackMe room as part of my training with **Masterschool**. The CTF was divided into two main tasks:

1. Gain SSH access to a target system  
2. Find 21 hidden flags across system, web, and service layers

---

## 🔐 Part 1 – Gaining SSH Access

We were provided with SSH credentials:

- **Username:** `ctf`  
- **Password:** `ctf`

```bash
ssh ctf@<IP_ADDRESS>
```

After logging in, we had to create a new user and a directory:

```bash
sudo adduser noelb
su noelb
mkdir holanlb
```

---

## 🏁 Part 2 – Finding the 21 Flags

### 🖥️ System Flags

#### Flag 1 – `{F1nd_Fl4g_Fun}`

Hint in Notion: _"one of the system flags is find_flag.txt"_

```bash
find / -name "*find_flag.txt*" -type f 2>/dev/null
cd /var/backups
cat find_flag.txt
```

#### Flags 2 & 3 – `{St0ry_Fl4g}` and `(Y0u_G0T_1t)`

```bash
find / -name "*flag*" 2>/dev/null
```

- `Flag/story.txt` → `{St0ry_Fl4g}`  
- `flag/.../f_l_a_g.txt` → `(Y0u_G0T_1t)`

#### Flag 4 – `{H1d3_1n_pl41n_s1gh7}`

```bash
ls -a
cat .f.txt
```

---

### 🌐 Web Page Flags

#### Using Browser (GUI):

- Main page: `{STUDENT_CTF_Web}`
- Developer Tools → Inspector: `{Another_Web_Flag}`
- Hidden paths:
  - `/hide.html` → `{H1d3_Fl4g}`
  - `/index2.html` → `{C0nf1gur4t10n_Fl4g}`

#### Using Terminal:

```bash
cd /var/www/html
ls -al
cat flag.txt             # → {Fl4g_fl4g_fl4g}
cat flag2.txt            # base64 → {Fl4g2_fl4g2_fl4g2}
curl http://<IP>/robots.txt   # → {Robots_Flag}
```

---

### 🔐 Hash Cracking

Copied the directory to root due to permission issues:

```bash
scp -r ctf@<IP>:/home/ctf/hash_to_crack/ /home/noe
```

Then used `hash-identifier` and `john` to crack the hashes.  
Note: `hash1.txt` and `hash5.txt` initially showed as MD2 but had to be cracked as MD5.

---

### 🗜️ ZIP File Flag

Scanned open ports with Nmap:

```bash
nmap -p 21 <IP>
```

Anonymous FTP access was enabled:

```bash
ftp <IP>
Name: anonymous
Password: anonymous
```

Found `files.zip` (password: `Masterschool`).  
Inside: `secret.zip` and `wordlist.txt`.

Used:

```bash
zip2john secret.zip > hash.txt
john hash.txt
```

Password cracked: `CTF_TIME`  
→ Revealed flag file: `{LetMe1n123!@#}`

---

### 🧩 Other Flags

- Upon login: `{h4ck3r5_r_us}`
- From FTP session: `{ftp_server_4_lyfe}`

---

## ✅ Final Thoughts

This CTF was an excellent hands-on learning experience. I practiced essential Linux skills, file enumeration, base64 decoding, hash cracking, and even basic web exploitation. I’m excited to move forward and tackle more advanced CTFs in the near future.

---

_Thanks for reading!_  
Feel free to connect or reach out with tips or feedback.  
🔐💻🚩
