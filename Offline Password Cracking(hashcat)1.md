Offline password cracking is an attack method where the attacker obtains password hashes and attempts to recover the original plaintext passwords without interacting with the authentication server.

## How Offline Attacks Work

- The attacker acquires hashed passwords from a compromised system or leaked database.
- Since hashes are non-reversible, the attacker guesses a password, hashes it, and compares it against the stolen hash.
- This process is repeated until a match is found or the attacker gives up.

## Common Hashing Algorithms

| Algorithm | Output Length | Notes |
| --- | --- | --- |
| MD5 | 128 bits | Fast, now considered insecure. |
| SHA-1 | 160 bits | More secure than MD5 but weak. |
| SHA-256 | 256 bits | Modern, secure hash. |
| SHA-512 | 512 bits | More secure, slower. |
| NTLM | 128 bits | Used in Windows authentication. |

## Example Hashes (@rmour123)

| Hash Type | Example Hash |
| --- | --- |
| MD5 | `974911862f344299010d0093ada3daea` |
| SHA-1 | `1376240f6e516bc5c5d32f1d2391721b6c03b248` |
| NTLM | `E41000190802C1FCFD3D8DFF20097090` |
| Password + Salt (123456 salt) | `91ff5ecbad84fbf13b356156f186aaac` |
| Salt + Password (salt 123456) | `93626223266b3dc5170180360ac52b0e` |
| MD5(MD5(password)) | `19cfe2620f714a9b82f4d544d1fab9d2` |
| SHA512(MD5(password)) | `4312007ac22ba1bc88bd9733fa9d35537c73c975a9686a28b7360456c9ee5cb400b1db3484ab086646caa7c2b993054e7e67bd04377610064aaaf515784` |

## Slow Hashes and Custom Hashing

- **Slow hashing algorithms** (e.g., bcrypt, scrypt) are computationally expensive to slow down attackers.
- **Custom hashes** may combine multiple algorithms and salts for added security.

## Hash Identification Tools

- **hash-identifier**: Identifies the hash type from its value.
    
    ```
    hash-identifier 974911862f344299010d0093ada3daea
    ```
    
- **hashid**: Supports many hash types for identification.
    
    ```
    hashid 6885858486f31043b839c735d99457f045affd0
    
    ```
    

## Defense Against Offline Attacks

- Use strong, slow hashing algorithms with unique salts.
- Enforce complex passwords to increase guessing difficulty.
- Regularly update and monitor password storage schemes.

# Password Cracking Rack

A password cracking rack is a distributed system infrastructure designed for large-scale password cracking using multiple GPU/CPU machines.

- Consists of high-performance machines (e.g., Nvidia 3090, 1080 Ti) working in parallel.
- Tasks are distributed to maximize cracking speed.
- Used by security teams for password recovery and auditing, or by attackers for offline cracking.

## Example: GoCrack Managed Cracking Tool

- Developed by FireEye's ICE team.
- Provides a web-based UI for managing cracking tasks in real-time.
- Automatically distributes tasks across machines in the rack.
- Features include user access controls, task auditing, and shared engine files while protecting sensitive data.
- Supports various cracking methods and engine files (e.g., dictionaries, mangling rules).

### Benefits

- Increases password cracking throughput dramatically.
- Simplifies management and automation of cracking campaigns.
- Enables team-based collaboration with fine-grained access controls.
- Centralizes logging and monitoring for compliance and auditing.

### Use Cases

- Auditing password strength in corporate environments.
- Recovering lost passwords from hash dumps.
- Penetration testing to identify weak credentials.
- Research and development of new cracking methods.

# Hashcat

## Installation

```
apt install hashcat

```

## Quick Help & Device Info

- Show full help:
    
    ```
    hashcat -h
    
    ```
    
- Filter help output for specific systems:
    
    ```
    hashcat -h | grep -i "mysql"
    
    ```
    
    ```
    hashcat -h | grep -i "linux"
    
    ```
    
    ```
    hashcat -h | grep -i "unix"
    
    ```
    
- List detected OpenCL/CUDA devices:
    
    ```
    hashcat -I
    
    ```
    
- GPU status:
    
    ```
    nvidia-smi
    
    ```
    
- Watch GPU usage (refresh every 0.5 seconds):
    
    ```
    watch -n 0.5 nvidia-smi
    or
    hashcat -m 0 -a 3 md5-hash.txt ?d?s?l?l?l
    
    ```
    

## Benchmarks

- Quick benchmark:
    
    ```
    hashcat -b
    
    ```
    
- Benchmark all kernels:
    
    ```
    hashcat -b --benchmark-all
    
    ```
    
- Benchmark all kernels on backend device 1:
    
    ```
    hashcat -b --benchmark-all --backend-devices 1
    
    ```
    

## Useful Utilities

- Show example hashes shipped with hashcat:
    
    ```
    hashcat --example-hashes
    
    ```
    
- Show potfile:
    
    ```
    cat ~/.hashcat/hashcat.potfile
    
    ```
    
- Remove potfile (start fresh):
    
    ```
    rm -f ~/.hashcat/hashcat.potfile
    
    ```
    

## Sessions & Restore

- Start with session name:
    
    ```
    hashcat --session myrun -m 0 -a 0 hashes.txt wordlist.txt
    
    ```
    
- Later restore:
    
    ```
    hashcat --session myrun --restore
    
    ```
    

## Basic Command Parts (Quick Reference)

- **Hash mode (type)**: `m <mode>` (e.g., 0 = MD5)
- **Attack mode**: `a <attack>` (0 = straight, 3 = mask, 6 = wordlist+mask, 7 = mask+wordlist)
- **Output file**: `o <file>`
- **Show cracked from potfile**: `-show`
- **Rules**: `r rules/<file>`
- **Devices**: `-backend-devices <ids>`
- **Remove cracked as found**: `-remove`

## Brute-Force (Mask) Examples

- Create MD5 and verify:
Content of `md5-hash.txt`:
    
    ```
    echo -n "1234" | md5sum
    
    ```
    
    ```
    vim md5-hash.txt
    
    ```
    
    ```
    81dc9bdb52d04dc20036dbd8313ed055
    
    ```
    
- Brute-force 4 digits (MD5):
    
    ```
    hashcat -m 0 -a 3 md5-hash.txt ?d?d?d?d -o md52-hash-output.txt
    
    ```
    
- Create 10-digit MD5 and brute-force:
    
    ```
    echo -n "9977747168" | md5sum > md5-hash-2.txt
    
    ```
    
    ```
    hashcat -m 0 -a 3 md5-hash-2.txt ?d?d?d?d?d?d?d?d?d?d
    
    ```
    
- Mixed mask (digit + special + lower):
    
    ```
    hashcat -m 0 -a 3 md5-hash.txt ?d?s?l?l?l
    
    ```
    

## Common Mask Tokens

- `?l`: Lower alpha (a-z)
- `?u`: Upper alpha (A-Z)
- `?d`: Digit (0-9)
- `?s`: Special (punctuation)
- `?a`: All (?l?u?d?s)
- `?h`: Hex (0-9a-f)

Dictionary (Straight) Attack

### Straight Dictionary (MD5 + rockyou)

**Description**: Generates an MD5 hash and cracks it using the rockyou wordlist.

```bash
echo -n 'password1' | md5sum | awk '{print $1}' > md5-hash.txt

```

- **Purpose**: Generates the MD5 hash of `(password1)` and saves it to `md5-hash.txt`.

```bash
hashcat -m 0 -a 0 md5-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs hashcat in straight mode (`a 0`) with MD5 hash mode (`m 0`) using the rockyou wordlist.

---

### Straight Dictionary (SHA1)

**Description**: Generates a SHA1 hash and cracks it using the rockyou wordlist.

```bash
echo -n '(password1)' | sha1sum | cut -d- -f 1 > sha1-hash.txt

```

- **Purpose**: Generates the SHA1 hash of `(password1)` and saves it to `sha1-hash.txt`.

```bash
hashcat -m 100 -a 0 sha1-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs hashcat in straight mode (`a 0`) with SHA1 hash mode (`m 100`) using the rockyou wordlist.
- **error :Confirm hash length**:

```bash
wc -l sha1-hash.txt
awk '{print length}' sha1-hash.txt

```

- All lines should be **40 characters for SHA1**.

---

### MD5(MD5(password))

**Description**: Cracks a double MD5 hash (MD5 of an MD5 hash).

```bash
echo -n '(password1)' | md5sum | cut -d- -f 1

```

- **Purpose**: Generates the MD5 hash of `(password1)`.

```bash
hashid -m 74549dfee9dc06873d2bce70ec516bcb

```

- **Purpose**: Uses `hashid` to confirm the hash type for `74549dfee9dc06873d2bce70ec516bcb`.

```bash
hashcat -m 2600 -a 0 md5-2.txt /opt/rockyou.txt

```

- **Purpose**: Runs hashcat with double MD5 mode (`m 2600`) using the rockyou wordlist.

---

### SHA256(MD5($password))

**Description**: Cracks a hash where the password is first MD5-hashed, then SHA256-hashed.

**Example Hash**: `b332b7a13ebb43fb6dbb0a480786be2e:598151d3486077377f7ac26371e9857fe563b2fbbf2513ef2c5f3d217100febd`

```bash
hashcat -m 20800 -a 0 md5-3.txt /opt/rockyou.txt

```

- **Purpose**: Runs hashcat with SHA256(MD5($password)) mode (`m 20800`) using the rockyou wordlist.

---

## Apply Rules

**Description**: Enhances dictionary attacks by applying transformation rules to the wordlist.

```bash
hashcat -m 0 -a 0 hashes.txt /opt/rockyou.txt -r rules/best64.rule

```

- **Purpose**: Uses the `best64.rule` rule set to mutate words from `rockyou.txt` for MD5 hashes (`m 0`) in straight mode (`a 0`).

---

## Hybrid Attacks

### Wordlist + Mask (Append Mask to Each Word)

**Description**: Appends a mask (e.g., three digits) to each word in the wordlist.

```bash
hashcat -m 0 -a 6 hashes.txt wordlist.txt ?d?d?d

```

- **Purpose**: Runs a hybrid wordlist + mask attack (`a 6`) for MD5 hashes (`m 0`), appending three digits (`?d?d?d`) to each word.

---

### Mask + Wordlist (Prepend Mask to Each Word)

**Description**: Prepends a mask (e.g., three digits) to each word in the wordlist.

```bash
hashcat -m 0 -a 7 hashes.txt ?d?d?d wordlist.txt

```

- **Purpose**: Runs a hybrid mask + wordlist attack (`a 7`) for MD5 hashes (`m 0`), prepending three digits (`?d?d?d`) to each word.

---

## Salted Hashes and Custom Formats

**Description**: Handles hashes with salts, typically in `hash:salt` or `salt:hash` format depending on the mode (`-m`).

**Example**: MD5($password.$salt)

```bash
echo -n "(password1)armourpasswd" | md5sum | awk '{print $1}' > md5-salt.txt

```

- **Purpose**: Generates the MD5 hash of `(password1)armourpasswd` and saves it to `md5-salt.txt`.

```bash
vim md5-salt.txt

```

- **Purpose**: Opens `md5-salt.txt` to add the hash and salt in the format `hash:salt` (e.g., `1dfe565eb2630a6f5203ec94943322a1:armourpasswd`).

```bash
hashcat -m 10 -a 0 md5-salt.txt /opt/rockyou.txt

```

- **Purpose**: Runs hashcat with MD5($password.$salt) mode (`m 10`) using the rockyou wordlist.
- **Note**: For other combinations (e.g., SHA256(MD5($password))), select the exact `m` mode matching the target service.

---

## WPA/WPA2 Handshake Cracking

### Convert pcap/cap to hccapx

**Description**: Converts a captured handshake file to a hashcat-compatible format.

```bash
cap2hccapx.bin ai3-01.cap ai3-01.hccapx

```

- **Purpose**: Converts `ai3-01.cap` to `ai3-01.hccapx` for hashcat compatibility.

---

### Crack Handshake with Wordlist

**Description**: Cracks a WPA/WPA2 handshake using a wordlist.

```bash
hashcat -m 2500 -a 0 ai3-01.hccapx /usr/share/seclists/Passwords/xato-net-10-million-passwords-1000000.txt -o ai3-crack

```

- **Purpose**: Runs hashcat in WPA-EAPOL mode (`m 2500`) with straight attack (`a 0`), using a large wordlist and saving cracked results to `ai3-crack`.

---

## Common Hash Modes

**Description**: List of commonly used hash modes in hashcat.

| Hash Mode | Algorithm |
| --- | --- |
| `-m 0` | MD5 |
| `-m 100` | SHA1 |
| `-m 1400` | SHA256 |
| `-m 3000` | LM (LAN Manager) |
| `-m 1000` | NTLM |
| `-m 2500` | WPA-EAPOL (WPA/WPA2) |
| `-m 1800` | sha512crypt |
- **Tip**: Verify the exact `m` mode for your target using:

```bash
hashcat --example-hashes

```

- **Purpose**: Inspects example hashes for each mode to confirm the correct format.

---

## Password Transforms/Combinatorics

### Apply Rules for Mutations

**Description**: Applies transformation rules to mutate words in a wordlist.

```bash
hashcat -m 0 -a 0 hashes.txt wordlist.txt -r rules/best64.rule

```

- **Purpose**: Uses `best64.rule` to transform words for MD5 hashes (`m 0`) in straight mode (`a 0`).

---

### Preview Transformations

**Description**: Outputs transformed words to stdout for inspection.

```bash
hashcat --stdout wordlist.txt -r rules/best64.rule | head

```

- **Purpose**: Previews the first few transformed words using `best64.rule`.

---

### Combine Dictionaries

**Description**: Combines two dictionaries to create new password candidates.

```bash
hashcat -m 0 -a 1 hashes.txt dict1.txt dict2.txt

```

- **Purpose**: Runs a combinator attack (`a 1`) for MD5 hashes (`m 0`), combining words from `dict1.txt` and `dict2.txt`.

---

## Potfile & Session Tips

### View Potfile

**Description**: Displays the contents of hashcat’s potfile, which stores cracked hashes.

```bash
cat ~/.local/share/hashcat/hashcat.potfile

```

- **Purpose**: Checks previously cracked hashes.

---

### Remove Potfile

**Description**: Deletes the potfile to avoid reusing previously cracked entries.

```bash
rm -f ~/.local/share/hashcat/hashcat.potfile

```

- **Purpose**: Clears the potfile for a fresh run.

---

### Start Named Session

**Description**: Starts a named session for a long-running job.

```bash
hashcat --session myrun -m 0 -a 3 hashes.txt ?d?d

```

- **Purpose**: Starts a session named `myrun` for a mask attack (`a 3`) on MD5 hashes (`m 0`) with a two-digit mask (`?d?d`).

---

### Restore Session

**Description**: Restores a previously started session.

```bash
hashcat --session myrun --restore

```

- **Purpose**: Resumes the `myrun` session.

---

## Performance Tips

### Monitor GPU Temps & Usage (Single Check)

**Description**: Checks GPU temperature and usage.

```bash
nvidia-smi

```

- **Purpose**: Displays current GPU status.

---

### Monitor GPU Temps & Usage (Continuous)

**Description**: Continuously monitors GPU temperature and usage.

```bash
watch -n 0.5 nvidia-smi

```

- **Purpose**: Updates GPU status every 0.5 seconds.

---

### Target Specific GPUs

**Description**: Specifies which GPU to use for hashcat.

```bash
hashcat -m 0 -a 0 --backend-devices 1 hashes.txt wordlist.txt

```

- **Purpose**: Runs hashcat using only GPU device 1 (`-backend-devices 1`) for MD5 hashes (`m 0`).

---

### General Performance Tips

- Use focused masks and quality wordlists before brute-forcing large keyspaces.
- Prefer good rule sets (e.g., `best64`, `d3ad0ne`) over blind masks when applicable.

---

## Final Checklist / Quick Tips

- **Confirm Hash Format**: Verify the exact hash format and `m` mode before running.
- **Check Potfile**: Use `-show` and check the potfile to avoid redundant work.
- **Attack Strategy**: Start with wordlists + rules, then hybrid attacks, then targeted masks, and finally full brute-force if needed.
- **Monitor GPU**: Keep an eye on GPU temperature and power usage.
- **Use Sessions**: Use `-session` to resume long-running jobs.

---

These notes separate each command into its own code block, with clear explanations and all details preserved. Let me know if you need further adjustments or additional clarifications!
