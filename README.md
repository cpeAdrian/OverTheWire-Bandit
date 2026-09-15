# OverTheWire Bandit Solutions
A personal writeup and command cheat sheet for completing the OverTheWire Bandit wargame.

---

### Level 0 -> 1
**Concept:** Connecting via Secure Shell (SSH) to a remote server and reading simple text files.

**Command:**
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
ls
cat readme
```

**How it works in practice:**
Instead of using a graphical user interface (like clicking through folders in Windows or macOS), you use the terminal to securely log into a remote machine (`ssh`), view its contents (`ls`), and dump the text of a file straight onto your screen (`cat`).

The basic syntax is:
```bash
ssh username@host -p port
cat filename
```

**Common Examples & Flags:**
* **Connect to a custom port (`-p`):** `ssh user@host -p 2220` (Specifies the port if it isn't the default 22).
* **List all files including hidden ones (`-a`):** `ls -a` (Reveals files starting with a dot).
* **List files with detailed info (`-l`):** `ls -l` (Shows file sizes, owners, and permissions).

---

### Level 1 -> 2
**Concept:** Opening files named with a hyphen (-) using relative pathing (./) to avoid command-line option misinterpretation.

**Command:**
```bash
ls
cat ./-
```

**How it works in practice:**
Many Linux commands use a hyphen (`-`) to specify flags (like `-l` or `-a`). If a file is literally named `-`, typing `cat -` confuses the system because it thinks you are trying to pass an incomplete option. To fix this, you explicitly tell Linux to look at the current folder location (`./`) first.

The basic syntax is:
```bash
cat ./filename
```

**Common Examples & Flags:**
* **Target a file in the current directory (`./`):** `cat ./-file` (Forces the program to treat the hyphen as a filename string).
* **Target a file by absolute path (`/`):** `cat /home/bandit1/-` (Bypasses option parsing by using the full system folder structure).

---

### Level 2 -> 3
**Concept:** Accessing files containing spaces in their filenames by wrapping the path in double quotes or escaping.

**Command:**
```bash
ls
cat "spaces in this filename"
```

**How it works in practice:**
Linux treats spaces as separators between different commands or arguments. If you type `cat spaces in this file`, Linux thinks you want to open 4 separate files named "spaces", "in", "this", and "file". Grouping the words inside quotes treats the entire name as a single entity.

The basic syntax is:
```bash
cat "file name with spaces"
```

**Common Examples & Flags:**
* **Using double quotes:** `cat "my document.txt"` (Safely handles spaces).
* **Using backslash escaping (`\`):** `cat my\ document.txt` (The backslash tells the terminal to ignore the special meaning of the very next space character).

---

### Level 3 -> 4
**Concept:** Navigating directories and using find or list flags to reveal hidden files starting with a dot (.).

**Command:**
```bash
cd inhere
find .
cat "... Hiding-From-You"
```

**How it works in practice:**
In Linux, any file or folder that starts with a full stop (`.`) is automatically hidden from standard view. This keeps configuration files clean and out of sight. You need to use specific tools or flags to tell the operating system to show you everything, hidden or not.

The basic syntax is:
```bash
ls -a
find .
```

**Common Examples & Flags:**
* **List all items (`-a`):** `ls -a` (Stands for "all", revealing hidden directories like `.` and `..`).
* **Find items in the current spot (`.`):** `find .` (Recursively lists every hidden and visible file underneath your current directory).

---

### Level 4 -> 5
**Concept:** Identifying human-readable ASCII text files among binary data files using the file inspection tool with wildcards.

**Command:**
```bash
cd inhere
ls
file ./*
cat ./-file07
```

**How it works in practice:**
Extensions (like `.txt` or `.exe`) don't actually matter to the Linux kernel; a file's true nature depends on its internal data structure. When you face dozens of files filled with garbled, compiled binary code, the `file` command peeks inside the metadata to tell you exactly what kind of file it is before you try opening it.

The basic syntax is:
```bash
file filename
```

**Common Examples & Flags:**
* **Check all files in a folder (`*`):** `file ./*` (The wildcard symbol `*` matches everything, allowing you to scan a whole directory at once).
* **Brief summary mode (`-b`):** `file -b text.txt` (Outputs just the file type description without repeating the filename).

---

### Level 5 -> 6
**Concept:** Searching subdirectories using the find utility with specific criteria for file type, exact byte size, and execution permissions.

**Command:**
```bash
cd inhere
ls
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
```

**How it works in practice:**
When you are looking for a needle in a haystack across hundreds of subfolders, you filter your search by setting specific parameters—like size constraints, file types, or negative filters (like things you aren't allowed to execute)—to drastically narrow down your targets.

The basic syntax is:
```bash
find /path -type f -size [number]c
```

**Common Examples & Flags:**
* **Filter by file type (`-type f` / `-type d`):** `find . -type d` (Limits results exclusively to directories instead of files).
* **Filter by exact byte size (`c`):** `find . -size 500c` (The `c` modifier stands for bytes/characters).
* **Logical NOT (`!`):** `find . ! -executable` (Finds files that do *not* have permission to run as a program).

---

### Level 6 -> 7
**Concept:** Searching the entire root filesystem using specific ownership properties (user and group) and exact byte sizes while redirecting error streams (`2>/dev/null`) to filter out permission errors.

**Command:**
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

**How it works in practice:**
When looking through the entire operating system, you will constantly hit files belonging to other users or the core system administrator (`root`), resulting in a flood of "Permission denied" errors. You can redirect these annoying errors straight into a virtual trash bin (`/dev/null`) so you only see valid hits.

The basic syntax is:
```bash
find /path -user username -group groupname 2>/dev/null
```

**Common Examples & Flags:**
* **Filter by owner (`-user`):** `find / -user bandit7` (Locates items belonging to a particular user target).
* **Filter by group (`-group`):** `find / -group bandit6` (Locates items associated with a particular security group).
* **Silence errors (`2>/dev/null`):** `command 2>/dev/null` (Catches stream `2`, which is standard error, and drops it into a digital black hole).

---

### Level 7 -> 8
**Concept:** Scanning massive data files using the `grep` utility to instantly target and extract lines matching a specific keyword.

**Command:**
```bash
grep "millionth" data.txt
```

**How it works in practice:**
Instead of manually opening a document and using `Ctrl + F`, you type a quick command in your terminal to find information instantly. 

The basic syntax is:
```bash
grep "search_term" filename.txt
```

**Common Examples & Flags:**
* **Search for a word:** `grep "error" server.log` (Pulls up every line containing the word "error").
* **Ignore capitalization (`-i`):** `grep -i "apple" fruit.txt` (Finds "apple", "Apple", or "APPLE").
* **Show line numbers (`-n`):** `grep -n "millionth" data.txt` (Displays the exact line number where the match was found).
* **Search multiple files recursively (`-r`):** `grep -r "TODO" ./project_folder` (Searches through every single file in a project directory).

---

### Level 8 -> 9
**Concept:** Sorting data and counting line occurrences using a pipe (`|`) to visually isolate the only non-repeating password string.

**Command:**
```bash
sort data.txt | uniq -c
```

**How it works in practice:**
The `uniq` command requires identical lines to be adjacent to count them. Running `sort` first groups all duplicate rows together. When piped into `uniq -c`, the system prefixes every line with a number showing its total occurrence count, allowing you to instantly spot the password prefixed with a `1`.

The basic syntax is:
```bash
sort filename.txt | uniq [flag]
```

**Common Examples & Flags:**
* **Count occurrences (`-c`):** `sort files.txt | uniq -c` (Prefixes each line with the number of times it repeated).
* **Show only completely unique lines (`-u`):** `sort files.txt | uniq -u` (Completely hides any line that had a duplicate).
* **Show only duplicated lines (`-d`):** `sort files.txt | uniq -d` (Prints only the lines that repeated).

---

### Level 9 -> 10
**Concept:** Extracting human-readable text strings from a compiled binary file using `strings` and filtering the text output with `grep`.

**Command:**
```bash
strings data.txt | grep "==="
```

**How it works in practice:**
Standard text tools like `grep` fail when targeting raw binary files because the terminal gets overwhelmed by machine code. The `strings` utility strips away all the corrupted binary characters and isolates text sequences longer than 4 characters. Piping that clean stream into `grep "==="` instantly targets the pattern surrounding the password.

The basic syntax is:
```bash
strings filename | grep "search_term"
```

**Common Examples & Flags:**
* **Extract text from a binary:** `strings program_file` (Dumps all ASCII strings hidden inside an executable or data blob).
* **Change minimum string length (`-n`):** `strings -n 8 data.txt` (Only extracts text strings that are 8 characters or longer).
* **Scan the whole file (`-a`):** `strings -a data.txt` (Forces the tool to scan every single byte of the file, not just the data sections).

---

### Level 10 -> 11
**Concept:** Decoding base64-encoded strings using the `base64` utility and piping text inputs with `echo -n` to bypass trailing newlines.

**Command:**
```bash
echo -n "VGhlIHBhc3N3b3JkIGlzIHBZZk9ZNkh3VXNEajVyTDlVdnloVTdNQ212OHZONVJvCg==" | base64 -d
```

**How it works in practice:**
Base64 is a binary-to-text encoding scheme often used to transmit data securely across systems without data corruption. It isn't encryption, so anyone can read it if they decode it. By passing the encoded string through `echo -n` (which prevents adding a hidden new line at the end) and piping it to `base64 -d`, the terminal instantly translates the scrambled characters back into human-readable text.

The basic syntax is:
```bash
echo -n "encoded_string" | base64 -d
```
*(Alternatively, you can decode directly from a file using: `base64 -d filename.txt`)*

**Common Examples & Flags:**
* **Decode data (`-d` / `--decode`):** `base64 -d file.b64` (Converts a Base64 text file back into its original text or binary format).
* **Encode text into Base64:** `echo -n "hello" | base64` (Converts the plain text string "hello" into a Base64 encoded string).
* **Ignore garbage characters (`-i`):** `base64 -d -i file.b64` (Ignores non-alphabet characters like random newlines or spaces that might break the decoder).

---

### Level 11 -> 12

**Concept:** Decoding ROT13-encoded text using the `tr` utility, while using `tr -d`, `fold`, and `paste` to clean and reconstruct the output.

**Command:**

```bash
tr -d '[:space:]' < data.txt | fold -w1 | tr 'A-Za-z' 'N-ZA-Mn-za-m' | paste -sd "" -
```

**Practical Output:**

```bash
$ cat data.txt
Gur cnffjbeq vf TEBbmJCB8DlA0zTewHxVQ0JPLxMvDkeA

$ tr -d '[:space:]' < data.txt | fold -w1 | tr 'A-Za-z' 'N-ZA-Mn-za-m' | paste -sd "" -
The password is TEBozWPO8OyN0mGjrUkID0WCYKkZiQxr
```

**How it works in practice:**
ROT13 ("rotate by 13 places") is a simple substitution cipher that replaces each letter with the letter 13 positions away in the alphabet. Because the English alphabet has 26 letters, applying ROT13 twice returns the original text.

The command first removes whitespace from `data.txt` using `tr -d '[:space:]'`. The `fold -w1` command then separates the text into individual characters. Each character is passed through `tr 'A-Za-z' 'N-ZA-Mn-za-m'`, which performs the ROT13 translation. Finally, `paste -sd "" -` joins the individual characters back into a single line.

The basic syntax is:

```bash
tr -d '[:space:]' < data.txt | fold -w1 | tr 'A-Za-z' 'N-ZA-Mn-za-m' | paste -sd "" -
```

**Common Examples & Flags:**

* **Delete characters (`-d`):** `tr -d '[:space:]' < data.txt` (Removes spaces, tabs, and newlines from the input).
* **Split characters (`fold -w1`):** `fold -w1` (Wraps the input so that each line contains one character).
* **Decode/Encode ROT13:** `tr 'A-Za-z' 'N-ZA-Mn-za-m'` (Rotates uppercase and lowercase letters by 13 positions).
* **Join lines (`paste -sd "" -`):** `paste -sd "" -` (Combines the individual lines back into one continuous string).
