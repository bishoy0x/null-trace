# Command History in Linux

## The Core Idea
The shell remembers every command you've typed.
You don't have to retype — you navigate.

## The Deep Dive
Why does history exist?
Because humans repeat tasks. The shell was built
around the assumption that if you ran something once,
you'll run something like it again.

The shell stores every command in `~/.bash_history`.
Every session reads from it and writes back to it.
Up arrow = walk backwards through time.

## The Security Angle
**Attacker:** history files are goldmines.
Passwords typed accidentally as commands,
API keys, server addresses, usernames —
all sitting in plaintext in `~/.bash_history`.

**Defender:** never type secrets directly into
the terminal. Clear sensitive history with
`history -c`. Some teams disable history entirely
on production servers.

## Real World Example
Countless real breaches started with an attacker
reading bash_history after gaining initial access.
Common in CTFs and real penetration tests —
first thing attackers check after shell access.

## Mental Model
```
~/.bash_history = your terminal's diary.
Everything you've ever typed is written there.
Anyone who reads it knows exactly what you did.
```

## Common Mistakes
- Typing passwords directly into the terminal
- Forgetting history persists across sessions
- Not knowing that `history` command shows everything

## Further Reading
1. https://man7.org/linux/man-pages/man3/history.3.html
2. https://www.gnu.org/software/bash/manual/bash.html#Bash-History-Facilities
3. https://attack.mitre.org/techniques/T1552/003
