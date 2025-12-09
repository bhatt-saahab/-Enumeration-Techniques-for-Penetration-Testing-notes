## Single MD5 Hash

- **Save the hash in md5-hash.txt (one per line) and run.**
    
    ```
    john md5-hash.txt
    
    ```
    
- **Or with explicit format + wordlist.**
    
    ```
    john --format=dynamic --wordlist=/opt/rockyou.txt md5-hash.txt
    
    ```
    
- **Format = dynamic_22 type = dynamic_22: md5(sha1(5p)) ((passWORD)-1c53f7f54f141a70d004981cf7babc6825e56d9b-daad4e5d2622f8c21676b671abald118).**
    
    ```
    john --format=dynamic_22 --wordlist=/opt/rockyou.txt md5-hash.txt
    
    ```
    

## Linux /etc/shadow (Local or Pulled)

- **Fetch from remote (requires root access).**
    
    ```
    scp -v root@192.168.1.31:/etc/passwd /tmp/passwd
    
    ```
    
    ```
    scp -v root@192.168.1.31:/etc/shadow /tmp/shadow
    
    ```
    
- **Combine for john.**
    
    ```
    unshadow /tmp/passwd /tmp/shadow > hash_passwd.txt
    
    ```
    
- **Attack.**
    
    ```
    john --wordlist=/opt/rockyou.txt hash_passwd.txt
    
    ```
    
- **Show results.**
    
    ```
    john --show hash_passwd.txt
    
    ```
    

## Zip Archives

- **Make an encrypted zip.**
    
    ```
    zip -er myfile.zip secret.txt
    
    ```
    
- **Extract john hash.**
    
    ```
    zip2john myfile.zip > zip_hash.txt
    
    ```
    
- **Crack.**
    
    ```
    john --wordlist=/opt/rockyou.txt zip_hash.txt
    
    ```
    
    ```
    john --wordlist=/opt/rockyou.txt --format=zip zip_hash.txt
    
    ```
    
- **Show results.**
    
    ```
    john --show zip_hash.txt
    
    ```
    

## RAR Archives

- **Create rar with password.**
    
    ```
    rar a -hpMyPass myfile.rar *.txt
    
    ```
    
- **Get hash & crack.**
    
    ```
    rar2john myfile.rar > rar_hash.txt
    
    ```
    
    ```
    john --wordlist=/opt/rockyou.txt rar_hash.txt
    
    ```
    
- **Show results.**
    
    ```
    john --show rar_hash.txt
    
    ```
    

## 7z Archives

- **Create 7z with password.**
    
    ```
    7z a myfile.7z *.txt -pmhe-on-p@rmour123
    
    ```
    
- **Get hash.**
    
    ```
    /usr/share/john/7z2john.pl myfile.7z > 7z_hash.txt
    
    ```
    
- **Crack.**
    
    ```
    john --wordlist=/opt/password.txt 7z_hash.txt
    
    ```
    
    ```
    john --wordlist=/opt/rockyou.txt 7z_hash.txt
    
    ```
    
- **Show results.**
    
    ```
    john --show 7z_hash.txt
    
    ```
    

## SSH Private Keys (id_rsa)

- **Create SSH key with passphrase.**
    
    ```
    ssh-keygen -q -N "armour123" -f id_rsa
    
    ```
    
- **Extract hash.**
    
    ```
    /usr/share/john/ssh2john.py id_rsa > ssh_id_rsa.txt
    
    ```
    
- **Crack.**
    
    ```
    john --wordlist=/opt/password.txt ssh_id_rsa.txt
    
    ```
    
    ```
    john --wordlist=/opt/rockyou.txt ssh_id_rsa.txt
    
    ```
    
- **Show results.**
    
    ```
    john --show ssh_id_rsa.txt
    
    ```
    

## PDF and Office

- **PDF: Extract hash.**
    
    ```
    pdf2john sample-pdf.pdf > sample-pdf-hash.txt
    
    ```
    
- **Crack PDF.**
    
    ```
    john --wordlist=/opt/password.txt sample-pdf-hash.txt
    
    ```
    
- **Office (docx/xlsx/pptx): Extract hash.**
    
    ```
    office2john file.docx file.xlsx > office-hash.txt
    
    ```
    
- **Crack Office.**
    
    ```
    john --wordlist=/opt/passwords.txt office-hash.txt
    
    ```
    

## Modes, Masks & Rules

- **-wordlist=FILE (or -w:)** Use a wordlist.
- **-rules:** Apply John ruleset (mutations) to a wordlist.
- **-mask:** Targeted brute-force masks (very efficient when pattern is known).
- **-incremental:** Full incremental brute force (exhaustive).
- **-format=NAME:** Force a format.
