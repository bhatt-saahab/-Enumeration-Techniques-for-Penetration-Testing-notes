This guide provides a comprehensive overview of password cracking, including storage methods, hashing vs. encryption, common cracking techniques, and essential commands/tools.

## 📚 Terminology

- **Plain Text**: The password as entered by the user (e.g., `armour123`).
- **Hash (Non-Reversible)**: A one-way transformation of input for secure storage (e.g., MD5, SHA1, SHA256).
- **Encryption (Reversible)**: Encoding that can be reversed with a key.
- **Salt**: Random data combined with a password to prevent lookup/rainbow-table attacks.

## 🔐 Hashing vs. Encryption

- **Hashing**:
    - One-way process, non-reversible.
    - Used to validate passwords without storing plaintext.
    - **Warning**: Weak algorithms (e.g., MD5, SHA-1) are easily cracked; avoid for new systems.
- **Encryption**:
    - Reversible with the correct key.
    - Used when plaintext recovery is needed (rare for user passwords).

## 🔍 Examples

- **Plain Text**: `@rmour123`
- **Base64 (Reversible)**: `QHJ1b3VVMTIz` (Base64 encoding of `armour123`)
- **ROT13**: Simple character substitution (see: [ROT13 Decoder](https://crystii.com/pipes/ret13-decoder))

## 🔢 Common Hash Types

| Hash Type | Example Hash for `armour123` | Security Note |
| --- | --- | --- |
| **MD5** | `9749118623442990100093ada3daes` | **Not secure** |
| **SHA-1** | `f37b240f8e516bc5c5d32f1d239122106c03b2e0` | Weak |
| **NTLM** | `E41000190882C1FCFD306DFF20097890` | Windows-specific |

## 🧂 Salting Examples

Salting adds random data to passwords to enhance security.

### Password + Salt

- **Input**: `123456 + (*23e9043e-02-38^2sp03e)(*&LOP`
- **Concatenation**: `123456(+23e9043e-02-38^2sp03e)(*&LOP`
- **Hash**: `91ff5ecbad84fb8f13b3561580106aac`

### Salt + Password

- **Input**: `(+23e9043e-02-36^2sp03e)(*&LOP + 123456`
- **Concatenation**: `(+23e9043e-02-36^2sp03e)(*&LOP123456`
- **Hash**: `93626223266b3dc5170a18036dac52ba`

### Salt + Password + Salt

- **Input**: `(+2309043e-02-36^2sp03e)(*&LOP123456(+2309043e-02-36^2sp03e)(*&LOP`
- **Hash**: `c619b9a7a27236defcf2764ff091a139`

## 🔄 Composed Hashes

- **MD5(MD5(password))**: `19cfe2620f714a9b82f4d544d1fab9d2` for `armour123`
- **SHA512(MD5(password))**: Produces a long SHA512 digest of the MD5 result.
- **Note**: Custom or composed schemes may give a false sense of security. Use well-reviewed slow hashes like **bcrypt**, **scrypt**, or **Argon2** for password storage.

## 🕵️‍♂️ Hash Identification

Identify the hash type before cracking.

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

![Screenshot 2025-10-07 085700.png](attachment:a64a813c-f397-4960-af04-b36a9270777b:Screenshot_2025-10-07_085700.png)

![Screenshot 2025-10-07 085744.png](attachment:d3be9008-f643-4e44-8460-e5c50b312941:Screenshot_2025-10-07_085744.png)

![Screenshot 2025-10-07 085757.png](attachment:7f7e01e2-b0fe-4338-a87e-95025b14479f:Screenshot_2025-10-07_085757.png)

![Screenshot 2025-10-07 085802.png](attachment:75c7113d-0e34-4ec6-81ea-330512b7d942:Screenshot_2025-10-07_085802.png)

## 💻 Offline Password Cracking

Offline attacks involve testing guesses against password hashes locally, without interacting with the target system.

### Classes of Attacks

- **Brute-Force**: Tests every possible combination in a character set.
    - **Pros**: Guaranteed to work (eventually).
    - **Cons**: Extremely slow.
    - **Tool**: [Brute-Force Calculator](http://tmedweb.tulane.edu/bfcalc)
- **Dictionary**: Uses wordlists (language-specific, themed, or site-specific).
    - **Pros**: Fast for common passwords.
    - **Cons**: Limited to known words.
- **Hybrid**: Combines dictionary words with numbers/symbols (e.g., `password123`).
- **Syllable Attacks**: Combines dictionary pieces (e.g., `putercom`, `putermoc`).
- **Rainbow Tables**: Precomputed hash lookups.
    - **Limitation**: Ineffective against salted hashes.
- **Rule-Based**: Applies transformation rules to dictionary words (e.g., appending two digits for passwords ≥ 8 characters).
