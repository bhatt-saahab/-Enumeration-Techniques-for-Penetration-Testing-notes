1. Clone / reference the GitHub repo (you provided the link):

```
https://github.com/InfoSecWarrior/Offensive-Pentesting-Resources/tree/main/Upload-Test-Files/Password-Protected-Documents

```

1. Copy the Password-Protected-Documents folder from `/opt` to the current directory (recursive, verbose):

```bash
cp -vr /opt/Offensive-Pentesting-Resources/Upload-Test-Files/Password-Protected-Documents .

```

1. Change directory to the Downloads folder containing the Password-Protected-Documents:

```bash
cd /home/armour/Downloads/Password-Protected-Documents

```

1. Verify you are in the correct directory (print working directory):

```bash
pwd

```

1. List files with human-readable sizes (long listing):

```bash
ls -lh

```

1. (Optional) Remove any existing hash output file before regenerating:

```bash
rm -f office-hash.txt

```

1. Extract Office hashes from Excel files and append to `office-hash.txt`:

```bash
office2john *.xls* >> office-hash.txt

```

1. Extract Office hashes from PowerPoint files and append to `office-hash.txt`:

```bash
office2john *.ppt* >> office-hash.txt

```

1. Extract Office hashes from Word files and append to `office-hash.txt`:

```bash
office2john *.doc* >> office-hash.txt

```

1. View the combined raw office hashes:

```bash
cat office-hash.txt

```

1. Convert the combined hashes to a list (strip the leading `username:...:` fields) and save to `office-hash-list.txt`:

```bash
cat office-hash.txt | cut -d ":" -f 2- > office-hash-list.txt

```

1. Verify the stripped hash list:

```bash
cat office-hash-list.txt

```

1. Crack the Office hashes with hashcat (example: mode 9400, straight attack, rockyou wordlist). Correct syntax (mode `m`, attack `a`):

```bash
hashcat -m 9400 -a 0 office-hash-list.txt /opt/rockyou.txt

```

1. If you want to list hashcat modes and search for MS Office modes:

```bash
hashcat -hh | grep -i "MS Office"

```

(That will show entries like `9400 | MS Office 2007`, `9500 | MS Office 2010`, etc.)

---

## Extracting and cracking hashes from 7z / zip / rar

### 7z Archives

1. Install Perl LZMA library (Debian/Ubuntu):

```bash
sudo apt install libcompress-raw-lzma-perl

```

1. Create an encrypted 7z archive (example uses a password and enables header encryption):

```bash
7z a -pYourPasswordHere -mhe=on myfile.7z sample-pdf.pdf

```

1. Download `7z2hashcat.pl` (script to convert 7z to hashcat format) and make it executable:

```bash
wget https://raw.githubusercontent.com/philsmd/7z2hashcat/master/7z2hashcat.pl
chmod +x 7z2hashcat.pl

```

1. Convert the 7z archive to hashcat format:

```bash
./7z2hashcat.pl myfile.7z > myfile_7z_hash.txt

```

1. Crack 7z hash with hashcat (mode 11600; example uses `/opt/password.txt` as wordlist):

```bash
hashcat -m 11600 -a 0 myfile_7z_hash.txt /opt/password.txt

```

---

### Zip Archives

1. Create an encrypted zip archive (interactive `e` or with `P` to set password on some zip implementations):

```bash
zip -e sample-pdf.zip sample-pdf.pdf

```

(When prompted enter the password; or use `zip -P YourPassword sample-pdf.zip sample-pdf.pdf` if you must supply it non-interactively — note `-P` is insecure because it exposes the password in process lists.)

1. Extract zip hash with `zip2john`:

```bash
zip2john sample-pdf.zip > zip_hash.txt

```

1. Crack zip hash with hashcat — choose mode depending on zip encryption type (examples):

```bash
hashcat -m 17220 -a 0 zip_hash.txt /opt/password.txt
# or
hashcat -m 17225 -a 0 zip_hash.txt /opt/password.txt

```

---

If you want, I can now:

- produce a single script that runs the office2john → cut → hashcat flow automatically, or
- adapt the hashcat mode to match each Office version (e.g., 9500, 9600, 9700, etc.).

Tell me which of those (script or per-mode guidance) you want and I’ll produce it right away.
