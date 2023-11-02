## level09
Here are the enumeration results
```
level09@SnowCrash:~$ ls
level09  token
level09@SnowCrash:~$ file *
level09: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x0e1c5a0dfb537112250e1c78d5afec3104abb143, not stripped
token:   data
level09@SnowCrash:~$ cat token
f4kmm6p|=�p�n��DB�Du{��
level09@SnowCrash:~$
```

Upon running `ltrace` on the executable, it looks like it detects the attempt and warns us to not reverse this using `ptrace(PTRACE_TRACEME, 0, 0, 0)`.

```
level09@SnowCrash:~$ ltrace ./level09
__libc_start_main(0x80487ce, 1, 0xbffff7f4, 0x8048aa0, 0x8048b10 <unfinished ...>
ptrace(0, 0, 1, 0, 0xb7e2fe38)                                            = -1
puts("You should not reverse this"You should not reverse this
)                                       = 28
+++ exited (status 1) +++
level09@SnowCrash:~$
```

So I just executed the executable arguments to test

```
level09@SnowCrash:~$ ./level09 a
a
level09@SnowCrash:~$ ./level09 b
b
level09@SnowCrash:~$ ./level09 c
c
level09@SnowCrash:~$ ./level09 1
1
level09@SnowCrash:~$ ./level09 ab
ac
level09@SnowCrash:~$ ./level09 abc
ace
level09@SnowCrash:~$ ./level09 abcd
aceg
level09@SnowCrash:~$ ./level09 abcdef
acegik
```

On the surface, looks like the program iterates through the characters and does an ASCII rotation based on the current index of the iteration

```
input: abcde

a - index 0, ascii + 0, output a
b - index 1, ascii + 1, output c
c - index 2, ascii + 2, output e
d - index 3, ascii + 3, output g
e - index 4, ascii + 4, output i

output: acegi
```

I also saw a token file earlier, maybe if that file was encrypted with this mechanism, and I need to decrpypt the file in the same manner. I used `od -t u1` to get the files bytes in decimal so I can construct an array and iterate through it applying the decryption in python. The script can be made like so

```python=
# obtained from od -t u1
# last byte removed because its out of range
bytes = [102,52,107,109,109,54,112,124,61,130,127,112,130,110,131,130,68,66,131,68,117,123,127,140,137]

for x, byte in enumerate(bytes) :
    bytes[x] -= x
print(''.join(chr(i) for i in bytes))

```

With the script created and ran, we are able to login to flag09 and get the flag `s5cAJpM8ev6XHw998pRWG728z`

```
level09@SnowCrash:~$ nano /dev/shm/script.py
level09@SnowCrash:~$ python /dev/shm/script.py
f3iji1ju5yuevaus41q1afiuq
level09@SnowCrash:~$ su flag09
Password:
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
flag09@SnowCrash:~$
```
