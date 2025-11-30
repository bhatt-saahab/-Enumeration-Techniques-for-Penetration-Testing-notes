RSMangler is a powerful tool that takes a base wordlist and applies multiple automatic mangling rules to produce extensive password candidate lists. These lists are useful for targeted dictionary and brute-force password testing. It is a small, single-file Ruby script included in Kali Linux.

**GitHub:** https://github.co/digininja/RSMangler?

## Key Features

- Applies case changes, leet substitutions, prefixes/suffixes like years or numbers, permutations of words, reversals, suffixes (ed, ing), punctuation, and more
- Generates large sets of common password variants from simple base words
- No external Ruby gems required
- Useful for penetration testing and ethical hacking

## Useful Options

- `file, -f <file>`: Input wordlist file or for stdin
- `perms, -p`: Permutate or combine words (very explosive; use only with tiny lists)
- `double, -d`: Repeat each word twice
- `reverse, -r`: Reverse the word
- `leet, -t`: Apply common leetspeak substitutions
- `full-leet, -T`: Apply full leetspeak variants
- `capital, -c`: Capitalize words
- `upper, -u`: Convert to uppercase
- `lower, -l`: Convert to lowercase
- `swap, -s`: Swap case (toggle upper/lower)
- `ed, -e`: Add "ed" suffix
- `ing, -i`: Add "ing" suffix
- `punctuation`: Add common punctuation suffixes
- `pna/-pnb`: Add 01-09 suffix/prefix
- `na/-nb`: Add 1-123 suffix/prefix
- `years`: Add years from 1990 to current year
- `acronym`: Build acronyms from input words
- `common`: Add common words like admin, sys, pwd
- `allow-duplicates`: Disable duplicate filtering (faster, less memory)
- `output <file>`: Write output to a file

## Practical Examples

Basic pipe with length limits to avoid massive output: (Keeps generated passwords between 6 and 12 characters.)

```
vim base.txt
```

```
armour
hacking
infosec
info
hacking
ethical 

```

```
cat base.txt | rsmangler -m 6 -x 12 --file - > mangled.txt

```

Generate leet + years + numeric variants:

```
rsmangler -file base.txt -leet -years -na -pna -punctuation -output mangled.txt

```

Permutations on a very small list (explosive output):

```
rsmangler.rb --file base.txt -perms -acronym -years > permuted_mangled.txt

```

Permutations on a very small list (explosive output):

```
rsmangler -file base.txt -perms -acronym -years > permuted_mangled.txt

```

Pipe directly into Hydra (authorized testing only):

```
rsmangler -file base.txt -leet -na | hydra -l username -P - ssh://target.example.com

```

**Base.txt Example Content:**

```
armour
infosec
hacking
ethical

```

# CeWL (Custom Word List Generator / Web Crawler) Cheat-Sheet

CeWL is a Ruby web-crawler that spiders a target URL (default depth 2), extracts unique words found on the site (optionally emails and metadata), and outputs a custom wordlist. This is useful for targeted password cracking and pentesting. It can follow external links if enabled and also extract author/creator metadata from documents.

**GitHub:** https://github.com/digininja/CeWL

## Key Options (Most Useful)

- `d, --depth <x>`: Spider depth (default 2).
- `m, --min_word_length`: Minimum word length (default 3).
- `o, --offsite`: Allow crawling other domains (caution: can explode crawl scope).
- `w, --write <file>`: Write output to file instead of stdout
- `e, --email`: Include email addresses found (mailto links). Use `-email_file` to save emails.
- `a, --meta`: Include metadata extraction from files (PDFs, Office). Use `-meta_file` to save metadata
- `u, --ua <agent>`: Set custom User-Agent header.
- `H, --header`: Add arbitrary HTTP headers. Can be used multiple times.
- `-proxy_host/--proxy_port`: Proxy support, including credentials.
- `c, --count`: Show count for each word (frequency).
- `n, --no-words`: Do not output words (useful if only emails or metadata are needed).
- `v, --verbose/--debug`: Enable verbose/debug output.

(Full help: `./cewl.rb --help`)

## Examples

Basic crawl, depth 2 (default), output to stdout:

```
cewl https://www.armourinfosec.com

```

Write words to file, minimum word length 4, depth 1:

```
cewl -d 1 -m 4 -w target_words.txt https://www.armourinfosec.com

```

Extract emails and save separately:

```
cewl --email --email_file emails.txt -w words.txt <https://www.armourinfosec.com>

```

Include metadata extraction and save meta:

```
cewl.rb --meta --meta_file meta.txt -w words.txt <https://www.armourinfosec.com>

```

Crawl offsite domains, use custom User-Agent and proxy:

```
cewl -o -u "Mozilla/5.0 (X11; Linux x86_64)" --proxy_host 127.0.0.1 --proxy_port 8080 -w words.txt <https://www.armourinfosec.com>

```

Docker usage with official image:

```
docker run --rm -v "${PWD}:/host" ghcr.io/digininja/cewl -w /host/words.txt <https://www.armourinfosec.com>

```

Note: Always run only with permission on authorized targets.

# CUPP (Common User Passwords Profiler) Cheat-Sheet

CUPP is a Python-based user-profile-driven wordlist generator. It interactively collects personal or OSINT data about a target (names, birthdays, pets, keywords, company info, etc.) and generates candidate password lists by combining, permuting, and applying common mangles like numbers, years, leet, and case variants. This creates smaller, more targeted wordlists than generic dictionaries.

**GitHub:** https://github.com/Mebus/cupp

## Main Modes / Flags (Common)

- `i`: Interactive mode, prompts user to input target details.
- `o <file>/--output <file>`: Write generated wordlist to file
- `w <file>`: Write wordlist to file (check your CUPP version as flags vary).
- `k <keywords>`: Use comma-separated keywords to seed wordlist (non-interactive).
- `-leet`: Enable leetspeak variations (version dependent).
- `h/--help`: Display help/usage for installed version.

Note: CUPP has several forks with slightly different flags and features. Check your local version with `cupp.py -h` or `cupp --help`.

## Quick Examples

Install:

```
apt install cupp

```

Interactive session: Prompts for names, pets, birthdays, company info, etc. Outputs combined wordlist.

```
cupp -i

```

Non-interactive with seed keywords:

```
cupp -k "rahul, armour, rahul1990, anand" -o cupp_wordlist.txt

```

Typical Kali/Installed flow:

```
cupp -i
# export wordlist when prompted or use -o for file output

```

# Username-to-Password-Wordlist Generator (Python Script)

This Python script reads usernames from a file and generates a comprehensive password wordlist using:

- Original, lowercase, capitalized, uppercase variants
- Reversed versions
- Common patterns like 123, 1234, 2024, etc.
- Case-sensitive output

## Input: users.txt Example

```
Harry
Elly
John
Kathy
Fred
Abby
Tim

```

## Script Code: generate_passwords.py

```
#!/usr/bin/env python3

# generate_passwords.py

def generate_passwords(input_file, output_file):
    with open(input_file, 'r') as f:
        names = [line.strip() for line in f if line.strip()]

    with open(output_file, 'w') as f:
        for name in names:
            original = name
            lower = name.lower()
            capital = name.capitalize()
            upper = name.upper()
            reverse = name[::-1]
            reverse_lower = lower[::-1]
            reverse_capital = capital[::-1]

            patterns = []
            for base in [original, lower, capital, upper, reverse, reverse_lower, reverse_capital]:
                patterns += [
                    base,
                    base + "123",
                    base + "1234",
                    base + "2024",
                    base + "@123",
                    base + "!",
                    base + "01",
                    base + "321",
                    base + "@",
                    base + "#",
                    base + "@" + base,
                    base + base[::-1]
                ]

            seen = set()
            unique_patterns = []
            for pwd in patterns:
                if pwd not in seen:
                    seen.add(pwd)
                    unique_patterns.append(pwd)

            for pwd in unique_patterns:
                f.write(pwd + '\\n')

    print(f"[+] Generated wordlist with case-sensitive and reversed variants into: {output_file}")

if __name__ == "__main__":
    import sys
    if len(sys.argv) != 3:
        print("Usage: ./generate_passwords.py users.txt passwords.txt")
        sys.exit(1)
    input_file = sys.argv[1]
    output_file = sys.argv[2]
    generate_passwords(input_file, output_file)

```

## How to Make It Executable

```
chmod +x generate_passwords.py

```

Then run it like this:

```
./generate_passwords.py users.txt passwords.txt

```

## Usage

```
python3 generate_passwords.py users.txt passwords.txt

```

## Example Output for Harry

```
Harry
Harry123
Harry1234
Harry2024
Harry@123
Harry!
Harry01
Harry321
Harry@
Harry#
Harry@Harry
Harryyrrah
harry
harry123
harry@123
Yrrah
YrraH123

```
