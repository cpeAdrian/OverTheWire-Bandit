# OverTheWire Bandit Solutions

A personal writeup and command cheat sheet for completing the OverTheWire Bandit wargame.

---

### Level 0 -> 1
* **Concept:** Connecting via SSH and reading simple text files.
* **Command:**
  ssh bandit0@bandit.labs.overthewire.org -p 2220
  ls
  cat readme

**Level 1 -> 2**
**Concept:** Opening files named with a hyphen (-) using relative pathing (./) to avoid command-line option misinterpretation.
**Command:**
ls
cat ./-

**Level 2 -> 3**
**Concept:** Accessing files containing spaces in their filenames by wrapping the path in double quotes.
**Command:**
ls
cat "spaces in this filename"

**Level 3 -> 4**
**Concept:** Navigating directories and using find to reveal hidden files starting with a dot (.).
**Command:**
cd inhere
find . inhere
cat "... Hiding-From-You"

**Level 4 -> 5**
**Concept:** Identifying human-readable ASCII text files among binary data files using the file inspection tool with wildcards.
**Command:**
cd inhere
ls
file ./*
cat ./-file07

**Level 5 -> 6**
**Concept:** Searching subdirectories using the find utility with specific criteria for file type, exact byte size, and execution permissions.
**Command:**
cd inhere
ls
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
