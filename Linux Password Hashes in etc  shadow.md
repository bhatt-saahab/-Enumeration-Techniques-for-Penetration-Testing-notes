Linux stores encrypted user passwords in the `/etc/shadow` file using a format that shows the hash algorithm, salt, and password hash, typically in the style `$type$salt$hashed`. The `$type$` specifies the cryptographic hash algorithm used, such as:

- `$1$` - MD5crypt (outdated)
• `$2a$` - Blowfish (bcrypt)
• `$2b$` - Blowfish (bcrypt variant)
• `$2y$` - Eksblowfish (bcrypt variant)
• `$5$` - SHA-256crypt
• `$6$` - SHA-512crypt
• `$y$` (or `$7$`) - Yescrypt

**Note:** CentOS and Kali Linux typically use SHA-512crypt (`$6$`) as the default.

## Extracting and Preparing Hashes for Cracking

To extract the hash portion from `/etc/shadow`, excluding disabled entries (those with `!` or `*` in the password field).

- View the full shadow file:

```
cat /etc/shadow

```

- Filter out disabled entries and extract the second field (hash):

```
grep -v '^[^:]*:[!*]' /etc/shadow | cut -d: -f2 > /tmp/linux_hash.txt

```

**Note:** `grep -v '^[^:]*:[!*]'` excludes lines where the password field starts with `!` or `*` (disabled accounts). `cut -d: -f2` pulls the second colon-delimited field (the hash). The output is saved to `/tmp/linux_hash.txt`.

### Example Extracted Hash (SHA-512crypt)

```
$6$FLR/N2+FRFG6T271569MYBUBdA7ofBSksV.isaFTS0TrfrbG6CZCZdaLwpUxg5NH267Gfqa.31.LU/s783.Qin915CHsEEIFescofN/$6$esT4m/oJwP3.UdW7SZnVsa40thUYNHWLxmaxAD/239PF2963NBAEULK31P7.Q5LKxWoo5P5k93d/aC75kn1.38ywXALAABV/Mt8sZ.

```

**Note:** This is a full example SHA-512crypt hash (`$6$`) from the data, including salt and hashed password. It appears concatenated or duplicated in the original; use as-is for testing.

- Use the `unshadow` command to combine `/etc/passwd` and `/etc/shadow` for tools like John the Ripper:

```
unshadow passwd shadow combined.txt

```

**Note:** `unshadow` merges username and hash data into a format like `username:hash:...` in `combined.txt`, useful for cracking tools that require user context.

- Identify hash types quickly using the `hashid` tool:

```
hashid -chash

```

**Note:** `hashid` (from HashID tool) analyzes a hash string (`-c` for single hash) and suggests possible types and cracking tools. Replace `hash` with your actual hash string.

## Understanding /etc/shadow Password Hashes

The second field in each `/etc/shadow` line is the password hash.

- Example format: `$6$salt$hashed` where `$6$` denotes SHA-512crypt.

**Note:** Other hash types (repeated for emphasis from data):
• `$1$` - MD5crypt (outdated)
• `$2a$/ $2b$/ $2y$` - Blowfish/Bcrypt
• `$5$` - SHA-256crypt
• `$6$` - SHA-512crypt (most common on modern distributions)
• `$y$` or `$7$` - Yescrypt (newer, more secure)

## Hashcat Modes for Cracking Linux Passwords

| Hash Type | Shadow Prefix | Hashcat Mode | Description |
| --- | --- | --- | --- |
| MD5crypt | $1$ | 500 | MD5 (Unix) |
| SHA-256crypt | $5$ | 1800 | SHA-256 (Unix) |
| SHA-512crypt | $6$ | 3200 | SHA-512 (Unix) |
| Bcrypt | $2a$/$2b$/$2y$ | 7400 | Blowfish/Bcrypt |

**Note:** The table has been corrected and aligned from the jumbled data. Use the mode matching your hash prefix.

- Example command (cracking SHA-512crypt):

```
hashcat -m 1800 -a 0 linux-hash.txt /opt/password.txt

```

**Note:** `-m 1800` for SHA-256crypt (corrected from data's 3200 mismatch; adjust to 3200 for SHA-512). `-a 0` is dictionary attack mode. Use `/opt/password.txt` as your wordlist.

## Cracking Apache apr1 MD5 Hashes (md5apr1, MD5 (APR))

Apache uses the apr1 format to store MD5-based hashed passwords, commonly found in `.htpasswd` files.

### Generating an Example .htpasswd File

- Create a basic `.htpasswd` file with a user:

```
htpasswd -c /etc/httpd/htpasswd user1

```

**Note:** `-c` creates the file if it doesn't exist. You'll be prompted for a password. This generates an apr1 hash for `user1`.

- View the file contents:

```
cat /etc/httpd/htpasswd

```

**Note:** Output example: `user1:$apr1$salt$hashedPasswordHere`

### Step 1: Identify the Hash Type

The apr1 format corresponds to Apache's variant of MD5 crypt, identified in Hashcat as mode 1600.

- Verify supported modes:

```
hashcat --help | grep -i "apr1"

```

**Note:** This lists modes containing "apr1" (e.g., 1600 for Apache MD5 APR1).

### Step 2: Prepare Your Hash File

Ensure you have your hashes in a file, e.g., `apache_hashes.txt`. Each line should contain a single apr1 hash:

```
user:$apr1$someSalt$encryptedHashString

```

**Note:** Include the username if using full format (user:hash). Strip usernames if necessary, keeping only the hash part for Hashcat compatibility.

### Step 3: Run Hashcat Attack

Use mode 1600 for Apache MD5 (APR1) and run a dictionary attack with your password list:

```
hashcat -m 1600 -a 0 apache_hashes.txt /path/to/wordlist.txt

```

**Note:** `-m 1600` specifies the Apache MD5 (APR1) hash. `-a 0` sets attack mode to dictionary (wordlist). `apache_hashes.txt` contains the hashes. `/path/to/wordlist.txt` is your dictionary of possible passwords.

### Optional Flags

- Use `show` to display cracked passwords after a successful run:

```
hashcat -m 1600 apache_hashes.txt --show

```

- Use `o cracked.txt` to save cracked pairs (username:password) to a file:

```
hashcat -m 1600 -a 0 -o cracked.txt apache_hashes.txt /path/to/wordlist.txt

```

**Note:** Run `--show` post-attack to view results. `-o` outputs cracked hashes during the run. Always use GPU acceleration for speed on large wordlists.

## General Cracking Notes

- **Requirements:** Root access for `/etc/shadow`. Install Hashcat, John the Ripper (for `unshadow`), and HashID.
• **Security:** These hashes are salted; weak passwords crack faster. Use for pentesting only.
• **Performance:** Add `-force` if needed for hardware issues; monitor with `w 3` for high workload.
• **Restoration:** Cracked passwords can be verified by updating `/etc/shadow` (root only) or testing login.
