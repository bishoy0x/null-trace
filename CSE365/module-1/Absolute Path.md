# Absolute Path


> **Author:** [bishoy0x](https://github.com/bishoy0x/null-trace)

---

## The Core Idea

Every file on a Linux system lives at a unique address starting from `/`, and you reach it the same way no matter where you are standing.

---

## The Deep Dive

Most people think a path is "how to get there from here" — that's a *relative* path, and it's wrong for this concept. An absolute path starts at `/`, the root, and never changes based on your current directory. Every directory is just a node in a tree, and `/` is the trunk. When you type `/pwn`, the kernel doesn't ask "where are you?" — it starts traversal at the root inode and resolves each component in order. No ambiguity, no context dependence, no surprises.

---

## The Security Angle

**Attacker:**  
Imagine your app only opens files inside `/uploads/`. User sends the filename `../../etc/shadow`. Linux literally walks: go up, go up, now open `shadow`. Your app just read a file it was never supposed to touch. The primitive is arbitrary file read.

**Defender:**  
`realpath()` takes any path — messy or clean — and gives back the real final location. `/uploads/../../etc/shadow` becomes `/etc/shadow`. Now you check: does this start with `/uploads/`? No → don't open it. The trick dies before the file ever opens.

---

## Real World Example

Apache should only serve files from `/var/www/html/`. A user sent `/.%2e/.%2e/etc/passwd` in the URL — Apache decoded `%2e` wrong and didn't realize it was just a dot. So it walked up the folders and served `/etc/passwd` to a stranger on the internet. **CVE-2021-41773.** Fix was one thing: resolve the full absolute path first, then check if it still lives inside `/var/www/html/`. It didn't. Block it.

---

## Mental Model

Absolute path works like a street address — "13 Nile St, Cairo, Egypt" finds the house whether you're in Cairo or Tokyo. A relative path is like saying "third door on the left" — only works if you're already in the right hallway.

---

## Common Mistakes

- I almost thought `/` was just the "home" folder — it's the root of the *entire* filesystem tree, not your user home.
- I got confused when `./pwn` didn't work but `/pwn` did — `./` means "here," and `pwn` wasn't in my current directory.
- I almost thought the path `/pwn` was unusual — binaries don't have to live in `/bin`; a CTF can drop an executable anywhere.

---

## Further Reading

1. `man hier` — Linux filesystem hierarchy standard
2. [CWE-22](https://cwe.mitre.org/data/definitions/22.html) — Improper Limitation of a Pathname to a Restricted Directory
3. Michael Kerrisk — *The Linux Programming Interface*, Chapter 18 (Directories and Links)

