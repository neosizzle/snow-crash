
## level13
We are greeted by a setuid elf executable, and here are the enumeration results
```
level13@SnowCrash:~$ file *
level13: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xde91cfbf70ca6632d7e4122f8210985dea778605, not stripped
level13@SnowCrash:~$ ltrace ./level13
__libc_start_main(0x804858c, 1, 0xbffff7f4, 0x80485f0, 0x8048660 <unfinished ...>
getuid()                                                                  = 2013
getuid()                                                                  = 2013
printf("UID %d started us but we we expe"..., 2013UID 2013 started us but we we expect 4242
)                       = 42
exit(1 <unfinished ...>
+++ exited (status 1) +++
level13@SnowCrash:~$

```

The executable calls getuid to get the current user id which is the uid of `level13`, and it checks the return value and compares it to 4242.

We couldnt really get more out of this by using tracing programs, so we went to try to modify the uid and made a C program that does so 

```
#include <unistd.h>
#include <sys/types.h>

int main() {
   uid_t uid = 1000; // Set the desired UID

   // Change the current UID to the specified UID
   if (setuid(uid) == -1) {
       perror("setuid");
       return 1;
   }

   return 0;
}
```

However, our user is not root so this code will fail with error `EPERM`. We also tried looking into exploits in the `getuid()` function itself and couldnt find any.

Without any other choices, our only option left is to manipulate register values to bypass the check during the programs runtime using gdb.

We launched gdb with the program name `gdb ./level13` and it brings us to the gdb shell. On another terminal, we run the command `tty` to get the tty device, so that gdb can send the stdout to that device during debugging.

![](https://hackmd.io/_uploads/BJsr3OAzp.png)

On the gdb shell, we first assign the output tty to the device we see just now by running `tty /dev/pts{id}`.

Once its done, we will make a breakpoint at the `getuid` function of the code because we want to make changes around that function by using `b getuid`.

We then enable register and assembly view by doing `layout asm` and `layout reg`.

Once we see the layouts, we can type `run` to start the debugging.

![](https://hackmd.io/_uploads/HyUr6u0f6.png)

We will have to step-in until we see the return instruction to change the register values, we use the command `stepi`

![](https://hackmd.io/_uploads/rJrKpuCfT.png)

Notice we are at the `ret` instruction, which is the assembly instrction for return. It will read the value in `eax` and return to the caller. It is at this point we want to change the value of `eax` to 4242.

```
set $eax=0x1092
```

and to continue the program execution till the process ends, use `continue`. In the other terminal, you should see the flag `2A31L79asukciNyi8uppkEuSx`

![](https://hackmd.io/_uploads/BkYWRuCzp.png)
