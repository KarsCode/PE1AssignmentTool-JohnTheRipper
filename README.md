# John the Ripper: Password Cracking Demo

## 🛠️ Project Overview
This repository demonstrates the usage of **John the Ripper (JtR)**, a powerful open-source password cracking tool widely used for security auditing and ethical hacking. This project provides:

- Sample hash files
- Instructions on installation and configuration
- A working demonstration using common hash formats (MD5, SHA1, etc.)
- Screenshots and scripts for reproducibility

---

## 📦 Installation Instructions

### Linux (Ubuntu/Debian-based)
```bash
sudo apt update
sudo apt install john -y
```

Alternatively, build the Jumbo version from source for extended capabilities:
```bash
git clone https://github.com/openwall/john.git
cd john/src
./configure && make -s clean && make -sj4
```

### Windows (via WSL or Cygwin)
- Enable WSL: https://docs.microsoft.com/en-us/windows/wsl/install
- Install Ubuntu from Microsoft Store
- Follow Linux installation steps inside the WSL terminal

---

## 🔍 Sample Use Cases

This demo includes three categories of hashes:
- MD5
- SHA1
- Unix-style `/etc/shadow` hash

### 1. MD5 Cracking

**Sample Hash File**: `hashes.txt`
```
user1:5f4dcc3b5aa765d61d8327deb882cf99
user2:e10adc3949ba59abbe56e057f20f883e
user3:0d107d09f5bbe40cade3de5c71e9e9b7
```

**Command:**
```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

**Expected Output:**
```bash
Using default input encoding: UTF-8
Loaded 3 password hashes with no different salts (Raw-MD5 [MD5 256/256 AVX2 8x3])
...
guess: password     (user1)
guess: 123456       (user2)
guess: letmein      (user3)
```

### 2. SHA1 Cracking

**Sample Hash File**: `sha1hashes.txt`
```
user1:8621ffdbc5698829397d97767ac13db3
user2:d0763edaa9d9bd2a9516280e9044d885
```

**Command:**
```bash
john --format=raw-sha1 --wordlist=/usr/share/wordlists/rockyou.txt sha1hashes.txt
```

### 3. Unix Shadow Format Cracking

**Sample File**: `shadowfile.txt`
```
test:$1$abc123$3LV3vGuJ4N7JbPzXyX3dX0
```

**Command:**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt shadowfile.txt
```

---

## 📄 Configuration and Troubleshooting

### Common Flags:
- `--format=<hash-type>`: Required if hash type is ambiguous
- `--wordlist=<file>`: Specify dictionary file
- `--show`: Show cracked results

### Tips:
- Make sure hashes have no whitespace or extra formatting
- Always double-check the hash format (e.g., MD5 vs raw-MD5)
- If John skips hashes, try explicitly specifying the format

### Troubleshooting:
- **Issue**: "No password hashes loaded"
  - ✅ Fix: Use the correct `--format`
- **Issue**: "Wordlist file not found"
  - ✅ Fix: Install rockyou.txt (`sudo apt install wordlists && gunzip /usr/share/wordlists/rockyou.txt.gz`)


## 🔗 References
- [John the Ripper Official Site](https://www.openwall.com/john/)
- [rockyou.txt Wordlist](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)

