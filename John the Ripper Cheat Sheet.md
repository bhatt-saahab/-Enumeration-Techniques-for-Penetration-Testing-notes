A compact, copy-paste friendly Markdown cheat sheet for John the Ripper (including Jumbo helpers). Use only on systems/files you own or where you have explicit written authorization.

## Quick Checklist

- John (install package or build bleeding-jumbo from https://github.com/openwall/john)
- Wordlists (e.g., rockyou.txt)
- Helper scripts: zip2john, rar2john, [7z2john.pl](http://7z2john.pl/), [ssh2john.py](http://ssh2john.py/), [pdf2john.py](http://pdf2john.py/), office2john
- ~/.john/john.pot (cracked passwords pot file)

## Install

### Debian/Ubuntu (Packaged)

```bash
apt update

```

```bash
apt install john libcompress-raw-lzma-perl

```

### For Maximum Format/Script Support (Build Jumbo from Source, Recommended)

- https://github.com/openwall/john (branch: bleeding-jumbo)

## Useful Paths & Listing Formats

### John Data Directory (Examples)

```bash
cd /usr/share/john

```

```bash
cat /usr/share/john/password.lst

```

### Pot File

```bash
cat ~/.john/john.pot

```

- If run as root:

```bash
cat /root/.john/john.pot

```

### List Supported Formats and Subformats

```bash
john --list=formats

```

```bash
john --list=subformats

```

```bash
john --list=subformats | grep -i md5

```

## Basic Commands

### Auto-Detect and Crack

```bash
john hashes.txt

```

### Use a Wordlist

```bash
john --wordlist=/path/to/wordlist.txt hashes.txt

```

- Or short form:

```bash
john -w:/path/to/wordlist.txt hashes.txt

```

### Specify a Format (Use --list=formats to Find Name)

```bash
john --format=FORMATNAME --wordlist=/path/to/wordlist.txt hashes.txt

```

### Show Cracked Results

```bash
john --show hashes.txt

```

### Check Status

```bash
john --status

```

## Session Management (Save & Restore)

```bash
john --session=myrun --wordlist=/opt/rockyou.txt hashes.txt

```

```bash
john --restore=myrun

```

## Common Workflows

### Single MD5 Hash

- Save the hash in md5-hash.txt (one per line) and run:

```bash
john md5-hash.txt

```

- Or with explicit format + wordlist:

```bash
john --format=dynamic_0 --wordlist=/opt/rockyou.txt md5-hash.txt

```

### Linux /etc/shadow (Local or Pulled)

- Fetch from remote (requires root access):

```bash
scp -v root@192.168.1.31:/etc/passwd /tmp/passwd

```

```bash
scp -v root@192.168.1.31:/etc/shadow /tmp/shadow

```

- Combine for John:

```bash
unshadow /tmp/passwd /tmp/shadow > hash_passwd.txt

```

- Attack:

```bash
john --wordlist=/opt/rockyou.txt hash_passwd.txt

```

```bash
john --show hash_passwd.txt

```

### Zip Archives

- Make an encrypted zip:

```bash
zip -e -r myfile.zip secret.txt

```

- Extract John hash:

```bash
zip2john myfile.zip > zip_hash.txt

```

- Crack:

```bash
john --wordlist=/opt/rockyou.txt --format=zip zip_hash.txt

```

```bash
john --show zip_hash.txt

```

### RAR Archives

- Create RAR with password:

```bash
rar a -hpMyPass myfile.rar *.txt

```

- Get hash & crack:

```bash
rar2john myfile.rar > rar_hash.txt

```

```bash
john --wordlist=/opt/rockyou.txt rar_hash.txt

```

```bash
john --show rar_hash.txt

```

### 7z Archives

```bash
7z a -pMyPass myfile.7z *.txt

```

```bash
/usr/share/john/7z2john.pl /tmp/myfile.7z > 7z_hash.txt

```

```bash
john --wordlist=/opt/rockyou.txt 7z_hash.txt

```

```bash
john --show 7z_hash.txt

```

### SSH Private Keys (id_rsa)

```bash
ssh-keygen -q -N "root123" -f /tmp/d1/id_rsa

```

```bash
/usr/share/john/ssh2john.py /tmp/d1/id_rsa > ssh_id_rsa.txt

```

```bash
john --wordlist=/opt/rockyou.txt ssh_id_rsa.txt

```

```bash
john --show ssh_id_rsa.txt

```

### PDF and Office

### PDF

```bash
pdf2john.py myfile.pdf > pdf_hash.txt

```

```bash
john --wordlist=/opt/passwords.txt pdf_hash.txt

```

### Office (docx/xlsx/pptx)

```bash
office2john file.docx file.xlsx > office_hash.txt

```

```bash
john --wordlist=/opt/passwords.txt office_hash.txt

```

## Modes, Masks & Rules

- `-wordlist=FILE` (or `w:`): Use a wordlist
- `rules`: Apply John ruleset (mutations) to a wordlist
- `-mask`: Targeted brute-force masks (very efficient when pattern is known)
- `-incremental`: Full incremental brute force (exhaustive)
- `-format=NAME`: Force a format

### Examples

### Use Rules with Rockyou

```bash
john --wordlist=/opt/rockyou.txt -rules hashes.txt

```

### Mask Example (e.g., 1 Uppercase, 4 Lower, 2 Digits)

```bash
john --mask='?u?l?l?l?l?d?d' hashes.txt

```

### Incremental (Slow; Exhaustive)

```bash
john --incremental hashes.txt

```

## Session & Performance Options

### Multi-Process (CPU)

```bash
john --fork=4 --wordlist=/opt/rockyou.txt hashes.txt

```

### Test/Benchmark Formats

```bash
john --test --format=FORMATNAME

```

### Specify Pot File

```bash
john --pot=/path/to/john.pot --wordlist=rockyou.txt hashes.txt

```

**Notes:** Fork works for CPU-only and can speed up multi-core cracking. For GPU cracking, use hashcat.

## Pot File & Viewing Cracked Passwords

- Default pot: ~/.john/john.pot (or /root/.john/john.pot if run as root).
- Use `john --show hashes.txt` to view cracked entries mapped to usernames.

## Alternatives & Complements

- **hashcat**: GPU-accelerated cracking (preferred for speed on GPUs)
- **hashcat-utils**: Helper tools for masks and wordlists
- Use John for niche formats or when hashcat doesn't support a format.
