**Overview**: Use `pdf2john` and `office2john` (from John the Ripper) to extract password hashes from protected document files (PDF, Word, Excel, PowerPoint). Then, use Hashcat to crack the extracted hashes. The example password used is `@rmour123`, and the wordlist is `/opt/rockyou.txt`.

---

## PDF Files

1. download the .pdf file which is alreday protected 

```bash
wget -O pdf-example-password.pdf "https://download.novapdf.com/download/samples/pdf-example-password.pdf"

```

### 2.pdf info

```bash
pdfinfo pdf-example-password.pdf | grep -i Encrypted
 
 
#it show incorrect password
```

3.conveted it into hash file 

```bash
 pdf2john pdf-example-password.pdf > pdf-example-password-hash.txt
```

**Description**: Extracts the password hash from a protected PDF file using `pdf2john`.

1. see the content

```bash
cat pdf-example-password-hash.txt 

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: pdf-example-password.pdf:$pdf$2*3*128*-4*1*16*34b1b6e593787af681a9b63fa8bf563b*32*289ece9b5ce451a5d7064693dab3badf101112131415161718191a1b1c1d1e1f*32*badad1e86442699427116d3e5d5271bc80a27814fc5e80f815efeef839354c5f

5 . NOW REMOVE THE FILE NAME FROM HASH USING VIM 

```bash
vim pdf-example-password-hash.txt

```

1. cheak mode : 

```bash
hashcat -hh | grep -i "pdf"

```

1. cracked the hash :

```bash
hashcat -m 25400 -a 0 pdf-example-password-hash.txt  /opt/rockyou.txt

```

8 .Run this to display the cracked password (no wordlist needed—it pulls from the potfile):

```bash
hashcat -m 25400 pdf-example-password-hash.txt --show

```

done ……………………………

---

---

## Excel Files

### Excel 2019

**Description**: Extracts and cracks the password hash from an Excel 2019 file.

```bash
office2john excel-2019.xlsx > excel-2019-hash.txt

```

- **Purpose**: Extracts the hash from `excel-2019.xlsx` and saves it to `excel-2019-hash.txt`.

```bash
cat excel-2019-hash.txt

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: `$office$*2013*100000*256*16*8266394254593b17e54e717cfb5a86c9*09d211d0bd860dad9464a68690eb0846*2168fee0f606521098ba00d450590d5de145598c71d546f50c9d93a6082c4422`

```bash
hashcat -hh | grep -i "MS Office"

```

- **Purpose**: Lists Hashcat’s supported MS Office hash modes to confirm the correct mode (e.g., `m 9600` for MS Office 2013/2016/2019).

```bash
hashcat -m 9600 -a 0 excel-2019-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2013/2016/2019 mode (`m 9600`) using the rockyou wordlist.

---

### Excel 2021

**Description**: Extracts and cracks the password hash from an Excel 2021 file.

```bash
office2john Excel-2021.xlsx > Excel-2021-hash.txt

```

- **Purpose**: Extracts the hash from `Excel-2021.xlsx` and saves it to `Excel-2021-hash.txt`.

```bash
cat Excel-2021-hash.txt

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: `$office$*2013*100000*256*16*41da9e431c68053743773bad6327a050*501f26df2b3fab4c118d17c6582b730*aed5b298f8150167797534e06*0abc6b2ecdd04097677ea8a75b361ca3ae33f8`

```bash
hashcat -m 9600 -a 0 Excel-2021-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2013/2016/2019/2021 mode (`m 9600`) using the rockyou wordlist.

---

## PowerPoint Files

### PowerPoint 2016

**Description**: Extracts and cracks the password hash from a PowerPoint 2016 file.

```bash
office2john powerpoint-2016.pptx > powerpoint-2016-hash.txt

```

- **Purpose**: Extracts the hash from `powerpoint-2016.pptx` and saves it to `powerpoint-2016-hash.txt`.

```bash
cat powerpoint-2016-hash.txt

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: `$office$*2013*100000*256*16*dc3a58a2709ed61ac43deb051deffe57*f0f29a498514a78ca4fd2a545043442*058c63baa41ec0ccdb66f944041784a92152013ded5474097c41ac69d0`

```bash
hashcat -m 9620 -a 0 powerpoint-2016-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2013/2016 mode (`m 9620`) using the rockyou wordlist.

---

### PowerPoint 2021

**Description**: Extracts and cracks the password hash from a PowerPoint 2021 file.

```bash
office2john PowerPoint-2021.pptx > PowerPoint-2021-hash.txt

```

- **Purpose**: Extracts the hash from `PowerPoint-2021.pptx` and saves it to `PowerPoint-2021-hash.txt`.

```bash
cat PowerPoint-2021-hash.txt

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: `$office$*2013*100000*256*16*d3f6b06b2d149c48fff9b4e4ede1812f*44598b8f88867344b4c88edd4845a771*98272eead155efd92ed136290006490aff1ea4d96e7d94daebb87b836c991605`

```bash
hashcat -m 9600 -a 0 PowerPoint-2021-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2013/2016/2019/2021 mode (`m 9600`) using the rockyou wordlist.

---

## Word Files

### Word 2016

**Description**: Extracts and cracks the password hash from a Word 2016 file.

```bash
office2john word-2016.docx > word-2016-hash.txt

```

- **Purpose**: Extracts the hash from `word-2016.docx` and saves it to `word-2016-hash.txt`.

```bash
cat word-2016-hash.txt

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: `$office$*2013*100000*256*16*2777434851800f71543054b27eb59f5*02046351612480104e9834f831da393e*4689a4ab9dc239464f090b8d35cf8a890b4de50992ebb3afb7124c281fe14d5`

```bash
hashcat -m 9600 -a 0 word-2016-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2013/2016/2019 mode (`m 9600`) using the rockyou wordlist.

---

### Word 2021

**Description**: Extracts and cracks the password hash from a Word 2021 file.

```bash
office2john word-2021.docx > Word-2021-hash.txt

```

- **Purpose**: Extracts the hash from `word-2021.docx` and saves it to `Word-2021-hash.txt`.

```bash
cat Word-2021-hash.txt

```

- **Purpose**: Displays the extracted hash.
- **Example Output**: `$office$*2013*100000*256*16*c283f22431f34edcdc29c367d37ae4d8*ced3e28bbce9aeae394f482e6eb20b68*d99e7f3d27059bf7c6323cbce9e1c346690446553c07e6b218c88e1da66c4303`

```bash
hashcat -m 9600 -a 0 Word-2021-hash.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2013/2016/2019/2021 mode (`m 9600`) using the rockyou wordlist.

---

## Office 2007 Series (Legacy)

**Description**: Extracts and cracks multiple Office 2007 hashes stored in a single file.

```bash
vim office2007.txt

```

- **Purpose**: Creates or edits `office2007.txt` to include multiple Office 2007 hashes.
- **Example Content**:
    
    ```
    $office$*2007*20*128*16*fa9191b6caf0dde203f3258af1be7284*306ec93aec32fc5d75cb78dbe8422c0b*027a26f440802defd799442bdde3692b7cde4e00
    $office$*2007*20*128*16*9fbc303fcb8e5a1fe2d6f9f976e0c737*6eaa86126d9c3213d76d81a6f67ce835*807a828b5b9d06e8bccb7aeb38227739ba58787a
    $office$*2007*20*128*16*5b492ae80774785d34824b3738e0d61f*c736d33d8802e0f212111e975ffc434d*0b1f4042b011faf8bfbf01e3bd692724f281f1fa
    
    ```
    

```bash
hashcat -m 9400 -a 0 office2007.txt /opt/rockyou.txt

```

- **Purpose**: Runs Hashcat in straight mode (`a 0`) with MS Office 2007 mode (`m 9400`) to crack all hashes in `office2007.txt` using the rockyou wordlist.

---

## Common Hashcat Modes

**Description**: List of Hashcat modes for different document types.

| File Type | Hash Mode | Description |
| --- | --- | --- |
| PDF (v1.7+) | `-m 25400` | Adobe PDF 1.7 Level 8 (Acrobat 9) |
| Office 2007 | `-m 9400` | MS Office 2007 |
| Office 2010 | `-m 9500` | MS Office 2010 |
| Office 2013-2019 | `-m 9600` | MS Office 2013/2016/2019 |
| Office 2021 | `-m 9600` | Same as 2013-2019 |

---

## Notes

- **Example Password**: The provided example password is `@rmour123`, tested with the `/opt/rockyou.txt` wordlist.
- **Hash Extraction**: Always use `pdf2john` for PDFs and `office2john` for MS Office files to extract hashes.
- **Hashcat Mode Verification**: Use `hashcat --hh | grep -i "pdf"` or `hashcat --hh | grep -i "MS Office"` to confirm the correct `m` mode.
- **Cracking Strategy**: Start with straight attacks (`a 0`) using a quality wordlist like `rockyou.txt` before trying rules or hybrid attacks.

---

These notes separate each command into its own code block, wrapped in backticks, with clear explanations and all details preserved. Let me know if you need further refinements or additional clarifications!
