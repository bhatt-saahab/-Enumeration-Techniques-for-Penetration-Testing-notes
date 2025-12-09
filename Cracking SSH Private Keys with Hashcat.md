## Generating an SSH Private Key

SSH private keys in the OpenSSH format can be generated and protected with a passphrase for testing purposes.

- Use the `ssh-keygen` command to create an RSA key with a custom passphrase.

```
ssh-keygen -t rsa -f id_rsa -P "@rmour123"

```

**Note:** The `-P` flag sets the passphrase (corrected from `--` in the provided data). This generates `id_rsa` (private key) and `id_rsa.pub` (public key). The passphrase "@rmour123" is used for encryption.

## Extracting the Hash

To crack the passphrase, first extract the hash from the SSH private key file using `ssh2john` (from John the Ripper suite). This converts the key into a format compatible with Hashcat.

- Run the extraction command:

```
ssh2john id_rsa > ssh_id_rsa_hash.txt

```

**Note:** `ssh2john` parses the OpenSSH private key and outputs a hash in the format `$sshng$<type>$<salt>$<checksum>$<encrypted_data>`. The output file `ssh_id_rsa_hash.txt` will contain the hash for cracking.

### Example Extracted Hash (Type 1)

```
$sshng$1$165A9AD270253055736945459E6E7DA084D$120050997a5555118509F13Fd94fccef@761df1d6eabf2ca2c04745704Fb82271ef66d6fb030e4b1daff&baa74132be1fe18196ebf61d7de7e0b36f8b84bd9ae572e91b7f78b05066676ebc2@161fbc75d3f2065b3e@ca46ba4a70c1eb1e0dfb0e03a72d3ca7930afae5cbd7ba37Lede3ee593f4f041528898ae2dc51fc99b4d85cc9b5987fb6a066461190255459bc26cdd95a13d98eb12df6b972551c9e8Sfaf9e1cdb4052356c573cdf69296d5f1b84dFF447ec0c5dad1e02f2ecfa2850688959c7F00ad55d6bc95684452fdc3b49@cba5f10bocb2f29629212bc89b3f51e54d081179588d6e168bc765ec98ee73e003e382f47d5f6fef6c6d8bbb2376dea8b56c02bb64f7b0acf880884566+4b8@21361cc5a2c6050ab7b12f6a9a4f5ec3bb70fac66d382ce6649a7f8ed162c369f077f9cde5fb20daff6b84dB42b4c30adcB0af7ab22fe5c99d57a9e4de7a8e64e@1bb070e1a20c26791bd11ff8d3f76673360917726674426089805845741eb8071836357e22cb318ecbb7230279df00a4d7c96f7ad225c462/5f75106024577892061484b@b3b6d1a81763@15c1@33116eb5c0f5128324129f00f7c358105dc06b61a8d8cd4b51404959153c1693fc9f02a8d3005905f8b9ef0f865b9d5491933f673c@b149bfa36f7d56a2f2229df4a25c6fc8aac83545486238ff42698e44b4aa4624e3f70айссвеобав836a9fd99e878596457704762f1fd37dce223e56060e15cef5bb3431c69dx2d88e979ca352x35180c9de7edc60484787eba5a379c6760b5b@@2a7eb43b5d5a76674884c85465286206ba4f273873d5ac7dbfcf7388d5ddd27e299de3a206ed70b26784ed7fb67be21067947abdhdba32a6e2f0fefae493186acb7141804bFb7768082a7c271bc1b40bb4blaebbe972d6bff7b26e88e863cb6fdabb64cbd5e3ba9d6f03319064b58a4d9c59db2b68bbe8b8dde6e85c402d0fb7908x2254e0f5454@ladc16e8ab0233601F14dbca78fcb74831f9ee928@d34d8c83ee5288544dbbb6f6dc5ef72#546222371440b5b6984585528@152006660519985443278fcd6d2cf51017bc42645057je696d22e9e61d9cd7d558c9d64d9224ccb169a3194588@c86450358c4bea66c5ba9cb403bfde33f732328ab31ed3e4748d@cb70fbd3ffa880c4be210381f8aed6fbc52dd0376684cf80072@1d0dd893d9edcf3c0d4c836c4c6fd@fc84c39bc156a9ead7af1d5b2e6075dbb6de2832329bda87934c65496871cf53f8fb198c4cfc24e@caf25ddb8c7ee3c233327c573be2687a2b416f9c90a55064ecdb2o094na7676919b13e3ee7011636751caf9488379f8dffbdcd4597bd864e64b93ef30b783865239258163642a6ba71234557c369941685be413b7c7d36a26152f09c5421ecbba3b4edfa1a34cef880b63421bc9720ff38960679aea654475ffbe103b4ed98395aa84830b4b632072405@9db34f5a55c0e38fa3d963ab56075d515ef8939f23fe076001888762138cfa5e4290bf90d-15ae6cb2babc3ad1f19e84ban7e12f04a26143338016709030c5f12#c2067864ff6867f3ae46b15cbfcee591lc3a9b99b421c80923ef3fa6163ea246ceac5ca2@1d8968217c2b2631d6a896de764a019d7cc65a672e63f7bb3cebb1065c33cc07ff7539e66b884765F7145646766155796ce88f

```

**Note:** This is a full, unedited hash example from the data (type value 1, indicated by `$sshng$1$`). It includes salt, checksum, and encrypted data. Hashes like this are sensitive; handle with care.

## Hashcat SSH Key Modes

Hashcat supports specific modes for different SSH key types and encryption ciphers. Select the mode based on the "type value" in the hash (e.g., after `$sshng$<type>$`).

| Type Value | Hashcat Mode | Key Type / Cipher |
| --- | --- | --- |
| 0 | 22900 | RSA/DSA + 3DES |
| 1 | 22931 | RSA, DSA (AES-128) |
| 2 | 22933 | RSA/DSA/EC + Bcrypt PBKDF + AES-256-CBC |
| 3 | 22934 | EC (AES-128) |
| 4 | 22932 | RSA, DSA (AES-192) |
| 5 | 22935 | RSA, DSA (AES-256) |
| 6 | 22936 | RSA/DSA/EC + Bcrypt PBKDF + AES-256-CTR |

**Note:** The table has been reconstructed and corrected for alignment from the jumbled data. "A" in the original appears to be a typo for type 6's description.

### Cracking a Type 1 Hash (Mode 22931)

For hashes starting with `$sshng$1$` (type value 1), use attack mode 0 (straight/dictionary) with a wordlist.

- Basic dictionary attack:

```
hashcat -m 22931 -a 0 ssh_id_rsa_hash.txt /opt/password.txt

```

- Using a larger wordlist (e.g., darkweb2017-top1000.txt):

```
hashcat -m 22931 -a 0 id_rsa_armour1.hash /opt/darkweb2017-top1000.txt

```

**Note:** `-m` specifies the mode, `-a 0` is dictionary attack. Replace paths as needed. Output will show cracked passphrase if successful.

### Cracking a Type 6 Hash (Mode 22936)

For hashes starting with `$sshng$6$` (type value 6), use mode 22936.

- Dictionary attack example:

```
hashcat -m 22936 -a 0 id_rsa_armour2.hash /opt/darkweb2017-top1000.txt

```

### Example Extracted Hash (Type 6)

```
$sshng$6$515a8e3e0b180882d36bb698f3c3cf33e3c$192656f70656e7373682d6b65792d7631000000000a61657332353620637472000000066253727978740000001800000010a8e3e0b180802d36bb698f3c3cf33e3c00000000001000001970000000773736820727361000000030100010000018100a637212402870267707ca63e5e4d79785ebfe@136fd277164c847c49aa151cefc55636de99d38e106cb597356b8e6dc35a3e2dc4a69bf9c26155662778d651deb7ee697f43bfc61c7ebfaa934e9d8850d5815bdf75a46fefb54d2b34c3179bb9bd51c158e88@b6712b811d9e4ae8e29c7763e6a7f1c4fec286585a9a466edd26d25fc151211da8a5ea84b0a4cb3c76d13fe7b63d6df9079fa82f540944906729d821d148cab989b66c419c89338879c1e55d5d1629e9d31127b6afbac026ae6e655830155d98bd2ae05cc@df9419efc7ad7e42fc2d32049857d54283d17563f2773dd9f4bb6df5497b5e40af67ba5e4c8029bc98232a059081104255f8eee83c63d@d6146468db4b6bfdd@d9a4d9cb91e37985d19f6fffbce80e340025647cc2d9fd8c6b2b6f9f6351eafe4c0183db5335dfe3f5f47863fb63f57da249f299f983fbc36674ecdladc63ab84cdecc2bf4d2f8618144d134f09dea4742362f2cb308c30e9937a7c20ff4f1ef04edeJe518243d03ee59be18a47744f957d02c0d5000005a0022ba485c8afaBe8ca8fa206d35f64145e428b6f1540017a33dd9fc531e17ee3e54d3a0ef7d033d379374916543c7a1fb3fcc5601b17266294969793175a28a02cc583dd596911b4d3deaf4384c606a569b28a6841556/689@ec@abe6a7d261cd2@f96f578cadbbce2a2985ed36c264418al46a39ca5d48624f30bd706abacf43b17ac4796dab1907d6db997dc0e67fed7094cc229397b522f64ff8e9cf92968769366a4eca5c0ae9075590e8d3b5377614309728fdc338effda976f475054104248973e4f2d5166b3f212911e66aad44f650354acbb576@ce149b1f5225aa3b37423fd51b910430d9f9bf046980ae997c4ace0a5c0e53517a955785182dacac9e3cbaa23978c785abbbb9b24bb1baf4f7c677b25f4f5@1651edbf8eaf508b6337666df11b286de4f6cff7543c92b9fbbc09ead244596ae16fc24cff7c407941834108621e8e18308e4c7c6a7c4199d986b179b53e6Zeeblee84b15d5be0f8e4aa593863fbbc6d22922a28f6e92f7f95dc8302fdf69e719d354997019c1db3cf48dccbe451bfd13406bfdd598bcaa3ccc897fdb21f2e95f1a2de4d1dee8f61a4066476906212d9eedf789253b27e1ce71bde4701896ecd62e99c143fcd5af61824a24b56b5d94183994cac4258fcace4410baea8153e6fa77d3f373abced@c8c936d568fellec5d1f5847bdd5d50183989a137d6a70@nbc97f4531956217669343ebe2ba7abe7d7e6e573f82f2b2def9f5a74547a7a88d9acof132959b09a4c318306c170d8843b196a0cb@c968a538358c5d5e444f12c@ebacc9f0f7778a701b6ec151574b75b0ff9c18c48533281641972746d8e7e28899fd21b6a1768969Bac3847daf6ce10f3ffe3e7959461aa9bcd7c4c5d56255c405635213b12f79c7e292a7771202508349d2a7a33c1e8e02204121810624401303598f0bf8cc580c785f9a21fa209]1b2dbd9d21475d5a44eed0416cde1c668e5822a8662b7b43e229c57355@25b12ed9261f938eb48c0a7c62bleclede735ecaa19be123e94f163da3921062160340138016dc754c458865338cb9ede55c8fd1f6f7f84fbc492400bf5f55f93645a62e33e311b8a49b925a9c6f3519581098599cfd853081ad7ebc0a7bc6fd47286a2a8c54f22571ff8e161@c2d57c04c00e40a5d348a7cb5d227ebf200bf485d948ebf832ea7686005ebcd70d544b7dd2f8bcf2e9dcb7e575f8935a650d78f5722385928dc63d12cee8571b258cd9a94f1b564778837bd4d3c39446151860027c47aeccc7aa8832d4978068c8caf9e8d7712@1452c9ba315d19c7679cf4f0750bdbdfd992@ef928344d33f25918479e8d08a4974c3dd2e8b51ac28c7733ef6a1b74e8f@aaa66937bc0b71f20f80d0bf4b246d7ce572514bb9192@168436d4e8bc528d50966@bdf151be6baa2617af1e3ecbff3aBee#31cb137650102cdae8c07317f99599056c7b360000772fd4a2b77062f1007e98c624c2459a21e4a1556fe8f6879aa5d894756551c1021973e28191f2e9acabele6413053637171edc5391f3492ea96692cd6643e87ea8e482bcfcc2264ec737135559c5e85bec95fa6d0e68a0637065012425896199c721fe44ff623885bbd0bb78e238cc42495cbf106a274dee1f@2268104fabcaa01e4f6a82428bf5201166aa7c89459e0ba6eaefb9f72691ac585b177d35ffaf617744d6b83b6db40ef8cb0749f22993c58aa7ab5a0b58a5831004f7dac2#84e5ab14f5f146db4e4e4a47e8ddd70bb0694

```

**Note:** This is the full, unedited type 6 hash example from the data (indicated by `$sshng$6$`). It includes OpenSSH header details, salt, and extensive encrypted key material. Use this directly in Hashcat for testing.

## General Cracking Notes

- **Requirements:** Install Hashcat and John the Ripper (for `ssh2john`). Wordlists like `/opt/darkweb2017-top1000.txt` or `/opt/password.txt` are common for dictionary attacks.
• **Attack Modes:** `a 0` for dictionary (straight), `a 3` for brute-force/mask if needed.
• **Performance:** Use GPU acceleration with Hashcat for faster cracking. Monitor with `-status` flag.
• **Security Warning:** Cracking passphrases is for authorized pentesting only. Weak passphrases (e.g., "@rmour123") crack quickly; use strong ones in production.
• **Restoration:** If cracking succeeds, restore the key with the passphrase using `ssh-keygen -y -f id_rsa -P <passphrase> > id_rsa.pub` to verify.
