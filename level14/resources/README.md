
## level14
This is a mystery, there are no files in the home directory and nothing created by flag14 user in the system that I can see 

```
level14@SnowCrash:~$ find / -user flag14 2>/dev/null
level14@SnowCrash:~$ ls
level14@SnowCrash:~$
```

the output of `top -U flag14` did not show any processes either 

![](https://hackmd.io/_uploads/HJa_kYAMT.png)

I think this means we gotta do everything ourselves now..

Since we used gdb in the last round to change the uid to 4242, maybe we can do the same to `getflag`, we can change the uid to the uid of flag14, which is according to `/etc/passwd`, 3014.

I set the breakpoint at `getuid`, but looks like I got caught red handed

![](https://hackmd.io/_uploads/rJ2RgYRza.png)

So what if i set the breakpoint at `ptrace`? Will I be able to bypass this?

Turns out I can, I adjusted the return value to a sucess value in ptrace and I managed to get to `getuid`. The rest is the same as the last level, and I managed to get the flag `7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ`


![](https://hackmd.io/_uploads/rJFfftAMT.png)

