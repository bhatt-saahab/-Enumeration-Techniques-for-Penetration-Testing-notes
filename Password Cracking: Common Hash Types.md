## MD5 (Message-Digest Algorithm 5)

MD5 is a widely used cryptographic hash function that produces a fixed 128-bit (16-byte) hash value from an input message of any length. It was designed by Ronald Rivest in 1991 to replace MD4 and is specified in RFC 1321. MD5 works by processing the input data in 512-bit blocks through a series of operations including bitwise functions (AND, OR, XOR, NOT), modular addition, and left rotations, resulting in a unique 128-bit message digest (hash).

MD5 hashes are typically represented as 32 hexadecimal characters. Even a small change in the input results in a significantly different hash due to the avalanche effect. While MD5 was widely used for purposes like verifying data integrity and digital signatures, it has known security vulnerabilities (collisions) that make it unsuitable for secure cryptographic applications today. However, it remains useful for non-cryptographic purposes like checksums and data partitioning because of its low computational requirements.

- MD5 outputs a 128-bit fixed length hash
- Input message is padded and divided into 512-bit blocks.
- Uses four rounds of 16 operations involving four logical functions
- Commonly represented as 32 hex digits.
- Vulnerable to collision attacks, hence not recommended for cryptographic security
- Still used for checksums and data integrity verification

**MD5-example**: 9749118625344299язавязалазанна fоr Breur123 (note: MOS is net securel echormur123 md5s,,

## SHA-1 (Secure Hash Algorithm 1)

SHA-1 is a cryptographic hash function that produces a 160-bit (20-byte) hash value known as a message digest. It was designed by the U.S. National Security Agency (NSA) and standardized by NIST in 1995 as part of the Secure Hash Standard. SHA-1 processes input data to produce a fixed-size 40-character hexadecimal string, regardless of input length.

SHA-1 operates by dividing the message into 512-bit blocks, padding the data, and processing it through 80 rounds of hashing functions involving bitwise operations and modular additions. It includes logical functions that vary in rounds and constants for processing.

While SHA-1 was widely used in applications like digital signatures, SSL/TLS certificates, and password storage, it has been found vulnerable to collision attacks since 2005. Major browsers and organizations now discourage its use, with formal deprecation by NIST recommending migration to more secure algorithms like SHA-2 or SHA-3 before 2030.

- Produces a 160-bit (20-byte) hash, shown as 40 hex characters.
- Designed by NSA and standardized by NIST in 1995
- Used for data integrity, digital signatures, authentication.
- Vulnerable to collisions and deprecated for secure uses.
- Replaced by newer SHA-2 and SHA-3 algorithms.

**SHA-1-example**: 37:240f8e516bc5c5d32f102391221b6c03b7ell for Armour123. echon armour123 shaisum,,

## SHA-2

Family of hash functions with variable output sizes: SHA-224, SHA-256, SHA-384, and SHA-512.

- Uses Merkle-Damgård structure with Davies-Meyer compression.
- Launched in 2001 as a successor to SHA-1.
- Produces larger hash values (224 to 512 bits), making it more resistant to collision and brute force attacks.
- Widely used today in security protocols such as TLS/SSL, cryptocurrencies, and digital certificates.

## SHA-3

Launched in 2008, standardized in 2015, based on Keccak (Sponge construction).

- Supports similar output sizes as SHA-2 (224, 256, 384, 512 bits), plus extendable output functions (SHAKE-128, SHAKE-256).
- Uses a different construction called Sponge, which absorbs and squeezes the input data.
- Offers strong security benefits, including resistance to length-extension attacks.
- Not a replacement mandated by NSA for SHA-2 yet but considered a robust alternative.
- Faster and more efficient with hardware implementations.,,

## Hash Identification

Useful to identify what type of hash you have before attempting cracking.

### Commands:

```
hashid 5f4dcc3b5aa765d61d8327deb882cf99

```

```
hashid -m 5f4dcc3b5aa765d61d8327deb882cf99

```

```
hash-identifier 5f4dcc3b5aa765d61d8327deb882cf99

```

## Offline Password Cracking

Offline attacks require access to password hashes. Once you have the hash, you can test guesses locally without interacting with the target system.

### Classes of Attacks

- **Brute-force**: try every possible combination in a character set. Guaranteed but slow. Use BF calculators to estimate time/space.
Example BF calculators: [tmedweb.tulane.edu](http://tmedweb.tulane.edu/) bfcalc
- **Dictionary**: try words from wordlists (language-specific, themed, harvested from site content). Fast when users choose common words.
- **Hybrid**: dictionary + appended/prepended numbers/symbols-common user behavior.
- **Syllable attacks**: combine dictionary pieces into many permutations (e.g., putercom, putermoc).
- **Rainbow tables**: precomputed hashes for quick lookup-ineffective if salts are used.
- **Rule-based**: apply transformation rules to dictionary words (e.g., require two digits if length ≥ 8).,,

# Password Cracking Techniques

Offline and online password cracking are two distinct approaches hackers use to gain unauthorized access by discovering passwords.

## Offline Password Cracking

- Involves obtaining encrypted password hashes from a compromised system or database.
- Attackers work on these password hashes locally, using powerful hardware like GPUs and specialized tools (e.g., Hashcat, John the Ripper).
- Without interacting with the target system, attackers can try billions of password guesses rapidly without triggering security alerts or account lockouts.
- Requires prior access to password hash files, often extracted via credential dumping or data breaches.
- Effective against weak hashing algorithms or unsalted hashes.
- Used by both attackers and forensic investigators to crack passwords covertly.

## Online Password Cracking

- Happens in real-time by attempting to authenticate directly on the live system (website login, application, remote access).
- Attackers try guessing passwords by sending login requests, either manually or via automated bots.
- Limited by network speed, system rate-limiting, account lockouts, and monitoring which can detect and block frequent failed attempts.
- Common techniques include brute force, credential stuffing, and password spraying.
- Easier to detect due to authentication attempts logged by servers.
- Often used when password hashes are not available to the attacker.,,

## Comparison: Offline vs. Online Password Cracking Techniques

| Aspect | Offline Password Cracking | Online Password Cracking |
| --- | --- | --- |
| **Attack Environment** | Password hashes obtained and cracked locally on attacker's machine | Attacks target live authentication systems over a network |
| **Speed** | Very fast-uses high-performance hardware (GPUs, ASICs) | Slower due to network latency, server response times, and limits |
| **Detection** | Hard to detect, no direct interaction with the target system | Easier to detect due to multiple failed login attempts |
| **Rate Limiting** | Not limited by server, can try billions of guesses | Throttled by server controls like account lockouts and CAPTCHAS |
| **Access Needed** | Hashes from databases, memory dumps, or breach data | Valid access point for login interface |
| **Tools Used** | Hashcat, John the Ripper, Rainbow tables | Automated scripts, bots, credential stuffing tools |
| **Attack Vector** | Focus on crackable hashes, weak or reused passwords | Direct password guessing, credential reuse, social engineering |
| **Success Likelihood** | Depends on hash strength, salting, and offline resources | Limited by effective monitoring and account lockout policies |
| **Use Scenario** | After compromise or breach to recover passwords | Reconnaissance or direct brute force attacks in real-time |

In summary, offline cracking offers speed and stealth but requires hash access, while online cracking is slower and noisy but requires live system access. Both exploit weak passwords but mitigation strategies differ, focusing on hashing strength for offline and monitoring/rate limiting for online attacks.
