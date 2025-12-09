# 7z Archives

## Installation and Setup

- **Install Perl LZMA library** (required for 7z2john tool):
    
    ```
    apt install libcompress-raw-lzma-perl
    
    ```
    

## Creating Encrypted Archive

- **Create encrypted 7z archive with a password**:
    
    ```
    7z a -p@rmour123 myfile.7z Sample.pdf
    
    ```
    

## Extracting Hashes for Cracking

- **Convert 7z archive to John the Ripper format with 7z2john** (outputs to bash.txt, but typically redirect to a file):
    
    ```
    7z2john myfile.7z > bash.txt
    
    ```
    
    - Example hash output (do not miss; this is the full important hash data):
        
        ```
        $7z$15$7250519505510546650ee5d54faf05269e96d1b3c8a8a252733364538512851145255217bc85ae1a107566523fe5e0319704ebc56262b463d45a2a92cf825116ba6dbf1423f61d25b9200dbaed6a1eb96aec88828f9920eb65ab1f4$ael3db8da3d70242ed253ba53033fa7a575061d4aa7bf19062201756675bbbe4caaa9b68b552219a895f866048966@c9ee587f7a6da8edc06e6800984c3bd06f45911#2dfba
        
        ```
        
- **Download 7z2hashcat script** (alternative tool for direct hashcat format):
    
    ```
    wget <https://raw.githubusercontent.com/philsmd/7z2hashcat/master/7z2hashcat.pl>
    
    ```
    
- **Make the script executable**:
    
    ```
    chmod +x 7z2hashcat.pl
    
    ```
    
- **Convert 7z archive to hashcat format with 7z2hashcat**:
    
    ```
    ./7z2hashcat.pl myfile.7z > myfile_7z_hash.txt
    
    ```
    
    - Example hash output (do not miss; this is the full important hash data):
        
        ```
        $7z$15$725051950558558923268f4b4205700000000000000005428715458151285114558f6426ffdb91b409$@1c318aae5b4b768dac4b03ff2f371031ff35bc5$@1a806f840e$@afe8083802f2fbf765d9dc273ef25975fe92a75aea3a80306$aec69f47c4bab68ac30c76d176c068fd420176641445510e9d1bc8319c599645b33f4af8e7$@5b5768eb2845ec3a6589573cbca2765ba3704307731f22a3fc8a1a0995cac01
        
        ```
        

## Hashcat Mode Discovery

- **List hashcat modes and grep for 7z-related** (to confirm modes like 11600):
    
    ```
    hashcat --help | grep -i "7z"
    
    ```
    

## Cracking the Hash

- **Crack with hashcat (mode 11600 for 7z)**:
    
    ```
    hashcat -m 11600 myfile_7z_hash.txt /opt/passwords.txt
    
    ```
    

# Zip Archives

## Creating Encrypted Archive

- **Create encrypted zip archive with a password**:
    
    ```
    zip -P rmour123 Sample.zip Sample.pdf
    
    ```
    

## Extracting Hashes for Cracking

- **Extract zip hash with zip2john**:
    
    ```
    zip2john Sample.zip > zip_hash.txt
    
    ```
    
    - Example hash output (do not miss; this is the full important hash data; note: includes PKZIP and file details):
        
        ```
        $pkzip2$1*2*0*3a7-b04-2c29b08-044-8-3a7-8853+b4890dd98c84dc2fa5d8fe42ba862682daf7seBaf3711064fdc0c186f36172659061f13b8d481954106@c97bd523e48d12a6d7cd3cb401h42ca8b63bba5e631756083c66F395278ch5585a8f5183375870b69c6be7473081801818a50a386360fbdbff82fccfc997dkf748800ef41a298f0ef23f559b50ce7f7f4971fe195c52360e98a53d20abc58f5d64c9701098847708800250984912f38002@fcacfdbcf17ab7bc91c91478223e71c9a9a421106@1d56c958945be52781882c4cf8426ede23354f1ccf228a6dd5821a4b720f683dbda64516f9984eb01747b8d3647ccfc72ba3befed040266bb20f1a8b6c4479a0bf618467fc450e941bf2be161a783fa8802a75d7c8207104362b0f491975c007000665ae29dcc553160uf4b53bbf8bcdee7d19231fb67375f5ce0dd2052f02e4c69e205275bb345750cc00c6633a90ab3831ef53a709d819f9104cbb5cf56486fe8f84fb43cc120d8aa4af99b1e@354132361cabbdfLbf8d4296cf09fc0df00d334e2b46c16fc9d861d056796cfc6693ed77f2b4f65345852ce2e3d6f2a3a5369@@f1bb@dcc16b8e968293ffcfide07@f91c035000087740cbc1276050924a3c2encabcheed3fa6d6d8ee8710040080edacac1f91d428574e1bfd4a9e4d76b90b90439dd58618eb0bf809cf6a920f8e12725cccaae5250295bcab@123fce440714b15f9bc4e7d46a8e0627bfb29b06d9c8489a78e25cae21e998a6bb6@lbelec21bf32312e9@w11c9ec8edc@f9c2dd73cfecf5d8109e7e704780b287df38f5ce29ea4fe128684bcc622cdb79bf9fcadlfc64ffeb75076c3a5e05072271b2552ff229197dd2421fc8292418e0cdd7621fbc2c84d62d4eb173be514d639ac0030081267813451d7ack27e3fdc949f66a0f6e26ea6f34afd5c31b4eex5/pkzipS:Sample.pdf:Sample.zip:Sample.zip
        
        ```
        

## Cracking the Hash

- **Crack with hashcat (mode 17200 for traditional ZIP encryption)**:
    
    ```
    hashcat -m 17200 zip_hash.txt /opt/passwords.txt
    
    ```
    
- **Crack with hashcat (mode 17225 for WinZip AES-256 encryption)**:
    
    ```
    hashcat -m 17225 zip_hash.txt /opt/passwords.txt
    
    ```
    

# RAR Archives

## Installation and Setup

- **Install RAR tools**:
    
    ```
    apt install rar
    
    ```
    

## Creating Encrypted Archive

- **Create encrypted RAR archive with a password**:
    
    ```
    rar a -p@rmour123 Sample.rar Sample.pdf
    
    ```
    

## Extracting Hashes for Cracking

- **Extract RAR hash with rar2john**:
    
    ```
    rar2john Sample.rar > rar_hash.txt
    
    ```
    
    - Example hash output (do not miss; this is the full important hash data for RAR5):
        
        ```
        $rar5$16$35a5676875415dd2b134e842304031c7$15$a9803cd98961e2e48000499232b9f639$8$a10c21f95e5035e3
        
        ```
        

## Cracking the Hash

- **Crack with hashcat (mode 12500 for RAR3 encryption)**:
    
    ```
    hashcat -m 12500 rar_hash.txt /opt/passwords.txt
    
    ```
    
- **Crack with hashcat (mode 13000 for RAR5 encryption)**:
    
    ```
    hashcat -m 13000 rar_hash.txt /opt/passwords.txt
    
    ```
