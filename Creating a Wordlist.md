

## Prebuilt Wordlists

Common commands and tips for working with distributed/prepackaged wordlists:

### View Compressed Contents Without Extracting

```
zcat /usr/share/wordlists/rockyou.txt.gz

```

### Copy Compressed File to Another Location and Extract

```
cp -v /usr/share/wordlists/rockyou.txt.gz /opt/rockyou.txt.gz

```

### Unpack rockyou (Debian/Ubuntu path)

```
gunzip /opt/rockyou.txt.gz

```

```
cat /opt/rockyou.txt

```

### Search for a Literal Word in a List

```
grep "^ilove$" /opt/rockyou.txt

```

### Useful Public Lists

- rockyou.txt (commonly available via Kali / wordlists packages)
- CrackStation wordlist ([crackstation.net/files/crackstation.txt.gz](http://crackstation.net/files/crackstation.txt.gz)),,

## seclists *

### Install Additional Collections

```
apt update

```

```
apt install seclists

```

```
cd /usr/share/seclists/Passwords

```

### Useful Public Lists

- CrackStation wordlist
- https://crackstation.net/files/crackstation.txt.gz,,

## Crunch - Generate Custom Wordlist

Crunch is a flexible generator for brute-force / mask-based wordlists.

### Basic Usage and Notes

### View Help/Manual

```
crunch --help

```

```
man crunch

```

### Syntax

```
crunch <min> <max> [charset] -t <pattern> -o <output-file>

```

### All Combinations Length 4 from Default Charset [a-z]

```
crunch 4 4

```

### Numeric Combos Length 4

```
crunch 4 4 0123456789

```

### Length 8 Using Default Charset [a-z]

```
crunch 8 8

```

### Lengths 8 through 12

```
crunch 8 12

```

### Using -t Pattern with Placeholders

@= lowercase, = UPPERCASE, % = numbers, ^ = symbols

```
crunch 10 10 0123456789 - 98260%%%%%

```

```
crunch 13 13 0123456789 - 98260%%%%%^^^

```

### Example Targeted Lists

```
# Example targeted lists
crunch 8 8 -t air%%%%% -o airtel-wordlist.txt

```

```
crunch 8 8 -t pass%%% -o pass-patterns.txt

```

```
#-l allows treating some placeholders as literal characters (must match -t length)
crunch 8 8 -t pass@%%% -1 aaaa@aaa

```

```
crunch 8 8 -t pass@%%% -l aaaa^%aa

```

### Other Useful Options

- p to generate permutations of given words (e.g., crunch pabc)
- o to write to a file, or -b to split output into smaller files (see crunch --help),,
