# John the Ripper: Password Cracking Demo

## 🛠️ Project Overview
This repository provides a comprehensive demonstration of **John the Ripper (JtR)** — a powerful and versatile password cracking tool used by cybersecurity professionals for password audits, penetration testing, and ethical hacking assessments. The main objective of this demo is to walk users through:

- Installing John the Ripper across major operating systems
- Creating and loading sample password hash files
- Executing dictionary-based brute force attacks using `rockyou.txt`
- Understanding common password formats (MD5, SHA1, Unix crypt)
- Analyzing and interpreting cracking results

---

## 📦 Installation Instructions

### Linux (Ubuntu/Debian-based)
Install the default package using:
```bash
sudo apt update
sudo apt install john -y
```

For extended features like support for rar/zip hash cracking, GPU acceleration, and additional formats, install the **Jumbo version**:
```bash
git clone https://github.com/openwall/john.git
cd john/src
./configure && make -s clean && make -sj4
```
This build enables dozens of additional formats including bcrypt, NT, and more.

### Windows (via WSL or Cygwin)
For Windows users, the recommended method is using **Windows Subsystem for Linux (WSL)**:
1. Follow the [WSL installation guide](https://docs.microsoft.com/en-us/windows/wsl/install)
2. Install Ubuntu from the Microsoft Store
3. Launch Ubuntu and follow the Linux installation steps above

You can also use **Cygwin** or precompiled Windows binaries, but WSL is better suited for this project.

---

## 🔍 Sample Use Cases

This repository includes hash samples from three popular formats:
- **MD5** — commonly found in web applications
- **SHA1** — slightly more secure but still vulnerable
- **Unix `/etc/shadow` hashes** — traditional Linux system hashes

Each case uses `rockyou.txt` (a famous leaked password list) as the wordlist.

---

### 1. 🔓 MD5 Cracking Example

**Hash File:** `hashes.txt`
```
user1:5f4dcc3b5aa765d61d8327deb882cf99
user2:e10adc3949ba59abbe56e057f20f883e
user3:0d107d09f5bbe40cade3de5c71e9e9b7
```
These hashes correspond to: `password`, `123456`, `letmein`

**Command:**
```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

**Explanation:**
- `--format=raw-md5`: Tells John we're cracking raw MD5 hashes (no salt)
- `--wordlist`: Specifies the dictionary file for the attack

**Expected Output:**
```bash
Using default input encoding: UTF-8
Loaded 3 password hashes with no different salts (Raw-MD5 [MD5 256/256 AVX2 8x3])
guess: password     (user1)
guess: 123456       (user2)
guess: letmein      (user3)
```

---

### 2. 🔓 SHA1 Cracking Example

**Hash File:** `sha1hashes.txt`
```
user1:8621ffdbc5698829397d97767ac13db3
user2:d0763edaa9d9bd2a9516280e9044d885
```
These correspond to: `1234567`, `letmein`

**Command:**
```bash
john --format=raw-sha1 --wordlist=/usr/share/wordlists/rockyou.txt sha1hashes.txt
```

**Output and analysis** will follow the same pattern as MD5 above.

---

### 3. 🔓 Unix Shadow Format Cracking

**Sample File:** `shadowfile.txt`
```
test:$1$abc123$3LV3vGuJ4N7JbPzXyX3dX0
```
This is a salted MD5-based crypt hash used in Unix systems (indicated by `$1$`).

**Command:**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt shadowfile.txt
```

No need to specify format in this case—John will autodetect based on the hash signature.

---

## 📄 Configuration and Troubleshooting

### 🔧 Common Flags
- `--format=<hash-type>`: Enforces format if autodetect fails
- `--wordlist=<path>`: Dictionary file location
- `--show`: Displays cracked results
- `--pot=<file>`: Custom potfile to store cracked passwords
- `--rules`: Enables rule-based mutations on the wordlist

### Tips
- Always sanitize your hash files—remove trailing spaces and invisible characters
- Use `file` and `hexdump` to inspect hash files for hidden formatting issues
- Try running `john --list=formats` to see all supported formats
- Use `--incremental` mode to brute-force every possibility (slow but thorough)

### 🛠️ Troubleshooting Scenarios
- **Error:** `No password hashes loaded`
  -  Fix: Use correct `--format`, check hash structure
- **Error:** `Wordlist file not found`
  -  Fix: Install and decompress rockyou.txt
    ```bash
    sudo apt install wordlists
    sudo gunzip /usr/share/wordlists/rockyou.txt.gz
    ```

---


## 🔗 References
- [John the Ripper Official Site](https://www.openwall.com/john/)
- [rockyou.txt Wordlist (via GitHub)](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)
- [John Wiki](https://github.com/openwall/john/blob/bleeding-jumbo/doc/README)
- [Password Hashing Algorithms](https://en.wikipedia.org/wiki/Cryptographic_hash_function)

