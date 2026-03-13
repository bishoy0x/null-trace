# Shell Arguments 
> **Author:** [bishoy0x](https://github.com/bishoy0x/null-trace)

---

## The Core Idea

The shell isn't just reading what you type — it's parsing it. First word is the program to run. Everything after is data you're handing to that program.

---

## The Deep Dive

Programs are useless without input. If `echo` couldn't take arguments, it would just print nothing forever. Arguments are how you talk to a program at runtime — without them, every behavior would have to be hardcoded. The shell splits your input on spaces, hands the pieces to the kernel, and the kernel loads the program with that data attached.

---

## The Security Angle

**Attacker:**  
If a program takes your input and passes it directly to the shell — you control the argument. That's command injection. One malicious string and you're executing arbitrary code.

**Defender:**  
Never trust user input. Sanitize everything before it touches a shell call. Better yet — avoid shell calls entirely.

---

## Real World Example

CVE-2014-6271 — Shellshock. Attackers injected malicious data through environment variables that bash treated as arguments. Remote code execution on millions of servers. All because input wasn't sanitized.

---

## Mental Model

```
hello hackers
  ↑       ↑
program  argument
```

Same as calling a function: `hello("hackers")`

---

## Common Mistakes

- Confusing the command with its arguments.
- Forgetting that spaces are the separator — a space in the wrong place breaks everything.
- Assuming arguments are safe because the user typed them.

---

## Further Reading

1. [bash(1) — Linux man page](https://man7.org/linux/man-pages/man1/bash.1.html)
2. [Shellshock (software bug)](https://en.wikipedia.org/wiki/Shellshock_(software_bug))
3. [pwn.college](https://pwn.college)
