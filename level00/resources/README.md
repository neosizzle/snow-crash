## level00
After some fails, I figured that the ISO given was for Mac OSX 64 bit and I launched to the following interface after booting.

![](https://hackmd.io/_uploads/By02Ctmb6.png)

I created an SSH tunnel and is able to connect through my local machine. I ran the getflag command expecting to get the flag, but looks like the challenge starts for 00 already

![](https://hackmd.io/_uploads/HJAzyq7Zp.png)

I ran a strings command on the getflags binary and here is what I get. I dont think this is anything useful.

```
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
__stack_chk_fail
strdup
stdout
fputc
fputs
getenv
stderr
getuid
ptrace
fwrite
open
__libc_start_main
GLIBC_2.4
GLIBC_2.0
PTRh@
QVhF
UWVS
[^_]
0123456
You should not reverse this
LD_PRELOAD
Injection Linked lib detected exit..
/etc/ld.so.preload
/proc/self/maps
/proc/self/maps is unaccessible, probably a LD_PRELOAD attempt exit..
libc
Check flag.Here is your token :
You are root are you that dumb ?
I`fA>_88eEd:=`85h0D8HE>,D
7`4Ci4=^d=J,?>i;6,7d416,7
<>B16\AD<C6,G_<1>^7ci>l4B
B8b:6,3fj7:,;bh>D@>8i:6@D
?4d@:,C>8C60G>8:h:Gb4?l,A
G8H.6,=4k5J0<cd/D@>>B:>:4
H8B8h_20B4J43><8>\ED<;j@3
78H:J4<4<9i_I4k0J^5>B1j`9
bci`mC{)jxkn<"uD~6%g7FK`7
Dc6m~;}f8Cj#xFkel;#&ycfbK
74H9D^3ed7k05445J0E4e;Da4
70hCi,E44Df[A4B/J@3f<=:`D
8_Dw"4#?+3i]q&;p6 gtw88EC
boe]!ai0FB@.:|L6l@A?>qJ}I
g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|
Nope there is no token here for you sorry. Try again :)
00000000 00:00 0
LD_PRELOAD detected through memory maps exit ..
;*2$"$
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.ctors
.dtors
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
crtstuff.c
__CTOR_LIST__
__DTOR_LIST__
__JCR_LIST__
__do_global_dtors_aux
completed.6159
dtor_idx.6161
frame_dummy
__CTOR_END__
__FRAME_END__
__JCR_END__
__do_global_ctors_aux
getflag.c
end.3187
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
syscall_gets
__i686.get_pc_thunk.bx
data_start
ft_des
stderr@@GLIBC_2.0
strdup@@GLIBC_2.0
_edata
_fini
__stack_chk_fail@@GLIBC_2.4
getuid@@GLIBC_2.0
fwrite@@GLIBC_2.0
__DTOR_END__
getenv@@GLIBC_2.0
__data_start
syscall_open
puts@@GLIBC_2.0
__gmon_start__
__dso_handle
open@@GLIBC_2.0
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
_end
_start
_fp_hw
stdout@@GLIBC_2.0
__bss_start
main
fputc@@GLIBC_2.0
afterSubstr
_Jv_RegisterClasses
isLib
fputs@@GLIBC_2.0
_init
ptrace@@GLIBC_2.0
level00@SnowCrash:~$ getflag
Check flag.Here is your token :
Nope there is no token here for you sorry. Try again :)
level00@SnowCrash:~$ clear
level00@SnowCrash:~$ strings /bin/getflag
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
__stack_chk_fail
strdup
stdout
fputc
fputs
getenv
stderr
getuid
ptrace
fwrite
open
__libc_start_main
GLIBC_2.4
GLIBC_2.0
PTRh@
QVhF
UWVS
[^_]
0123456
You should not reverse this
LD_PRELOAD
Injection Linked lib detected exit..
/etc/ld.so.preload
/proc/self/maps
/proc/self/maps is unaccessible, probably a LD_PRELOAD attempt exit..
libc
Check flag.Here is your token :
You are root are you that dumb ?
I`fA>_88eEd:=`85h0D8HE>,D
7`4Ci4=^d=J,?>i;6,7d416,7
<>B16\AD<C6,G_<1>^7ci>l4B
B8b:6,3fj7:,;bh>D@>8i:6@D
?4d@:,C>8C60G>8:h:Gb4?l,A
G8H.6,=4k5J0<cd/D@>>B:>:4
H8B8h_20B4J43><8>\ED<;j@3
78H:J4<4<9i_I4k0J^5>B1j`9
bci`mC{)jxkn<"uD~6%g7FK`7
Dc6m~;}f8Cj#xFkel;#&ycfbK
74H9D^3ed7k05445J0E4e;Da4
70hCi,E44Df[A4B/J@3f<=:`D
8_Dw"4#?+3i]q&;p6 gtw88EC
boe]!ai0FB@.:|L6l@A?>qJ}I
g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|
Nope there is no token here for you sorry. Try again :)
00000000 00:00 0
LD_PRELOAD detected through memory maps exit ..
;*2$"$
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.ctors
.dtors
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
crtstuff.c
__CTOR_LIST__
__DTOR_LIST__
__JCR_LIST__
__do_global_dtors_aux
completed.6159
dtor_idx.6161
frame_dummy
__CTOR_END__
__FRAME_END__
__JCR_END__
__do_global_ctors_aux
getflag.c
end.3187
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
syscall_gets
__i686.get_pc_thunk.bx
data_start
ft_des
stderr@@GLIBC_2.0
strdup@@GLIBC_2.0
_edata
_fini
__stack_chk_fail@@GLIBC_2.4
getuid@@GLIBC_2.0
fwrite@@GLIBC_2.0
__DTOR_END__
getenv@@GLIBC_2.0
__data_start
syscall_open
puts@@GLIBC_2.0
__gmon_start__
__dso_handle
open@@GLIBC_2.0
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
_end
_start
_fp_hw
stdout@@GLIBC_2.0
__bss_start
main
fputc@@GLIBC_2.0
afterSubstr
_Jv_RegisterClasses
isLib
fputs@@GLIBC_2.0
_init
ptrace@@GLIBC_2.0
```

Ran an strace on the program with no args, still didnt find anything good.

![](https://hackmd.io/_uploads/r1xRkcmW6.png)

Sudoing wont work either
![](https://hackmd.io/_uploads/rkS8-cmW6.png)

Looks like the subject states that I need to run the program as flagXX user, which indicates that this might be a privellege escalation problem.

I checked `/etc/passwd` for any clues and I see this strange thing on the next user, ill keep note. But others than that, all the passwords seem protected
![](https://hackmd.io/_uploads/S1qwxoXb6.png)


I tried running a `sudo -l` to list all the programs I can run, but looks like I got no luck

![](https://hackmd.io/_uploads/r19JpcQZa.png)


In that case, I guess I can find all the files created by me to acheive the same thing using `find / -user level00`
```
/dev/pts/0
/dev/tty1
find: `/etc/chatscripts': Permission denied
.
.
.
/proc/2592/coredump_filter
/proc/2592/io
find: `/root': Permission denied
find: `/sys/kernel/debug': Permission denied
find: `/tmp': Permission denied
find: `/var/cache/ldconfig': Permission denied
find: `/var/lib/php5': Permission denied
find: `/var/lib/sudo': Permission denied
find: `/var/spool/cron/atjobs': Permission denied
find: `/var/spool/cron/atspool': Permission denied
find: `/var/spool/cron/crontabs': Permission denied
find: `/var/tmp': Permission denied
find: `/var/www/level04': Permission denied
find: `/var/www/level12': Permission denied
find: `/rofs/etc/chatscripts': Permission denied
find: `/rofs/etc/ppp/peers': Permission denied
find: `/rofs/etc/ssl/private': Permission denied
find: `/rofs/home': Permission denied
find: `/rofs/root': Permission denied
find: `/rofs/var/cache/ldconfig': Permission denied
find: `/rofs/var/lib/php5': Permission denied
find: `/rofs/var/lib/sudo': Permission denied
find: `/rofs/var/spool/cron/atjobs': Permission denied
find: `/rofs/var/spool/cron/atspool': Permission denied
find: `/rofs/var/spool/cron/crontabs': Permission denied
find: `/rofs/var/tmp': Permission denied
find: `/rofs/var/www/level04': Permission denied
find: `/rofs/var/www/level12': Permission denied
```

Thats alot of files, Ill filter out the error texts by redirecting stderr to /dev/null ` find / -user level00  2>/dev/null`

```
/dev/pts/0
/dev/tty1
/proc/1814
/proc/1814/task
/proc/1814/task/1814
/proc/1814/task/1814/attr
/proc/1814/net
/proc/1814/attr
/proc/1815
/proc/1815/task

[...]

/proc/2672/sessionid
/proc/2672/coredump_filter
/proc/2672/io
```

This is what I got. Doesnt seem to have nothing special here, just process files and system files. I will do the same thing for the `flag00` user..

```
level00@SnowCrash:~$ find / -user flag00  2>/dev/null
/usr/sbin/john
/rofs/usr/sbin/john
level00@SnowCrash:~$

```

... and there are 2 files which can be read by us, intresting.

```
level00@SnowCrash:~$ file /usr/sbin/john
/usr/sbin/john: ASCII text
level00@SnowCrash:~$ file /rofs/usr/sbin/john
/rofs/usr/sbin/john: ASCII text
level00@SnowCrash:~$
```

They are both ASCII files, could they contain the password?

```
level00@SnowCrash:~$ cat /usr/sbin/john
cdiiddwpgswtgt
level00@SnowCrash:~$ cat  /rofs/usr/sbin/john
cdiiddwpgswtgt
level00@SnowCrash:~$
```

Looks like we might have our password, lets try to login to flag00

```
level00@SnowCrash:~$ su flag00
Password:
su: Authentication failure
level00@SnowCrash:~$
```

Nope, its incorrect. Looks like this is an encrypted password. I went on https://www.dcode.fr/caesar-cipher to hopefully find a decryption and luckily, I managed to extract the plaintext from the caesar cipher

![](https://hackmd.io/_uploads/Bymgxsm-T.png)


`nottoohardhere` is the password. I tried it and I'm in, launched getflag and I got my password `x24ti5gi3x0ol2eh4esiuxias`

```
level00@SnowCrash:~$ su flag00
Password:
Don't forget to launch getflag !
flag00@SnowCrash:~$ getflag
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
flag00@SnowCrash:~$
```