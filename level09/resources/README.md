## level02
Upon logging in, I was greeted by a `level02.pcap` in my home directory. Something tells be we need to use wireshark for this..

I loaded the file into wireshark and followed the TCP stream which constructs the TCP message based on the frames and got this.

![](https://hackmd.io/_uploads/BkjM-bNba.png)

I tried c/p the string inside the password prompt, but the login failed. Looks like the `...` characters might be something else, I displayed the characters in hex, and got this..


![](https://hackmd.io/_uploads/Byn0fb4W6.png)


This is what the client typed, and turns out the 7F is a delete character in ASCII, which might indicate a backspace. So the password became from  `ft_wandr...NDRel.L0L` to `ft_waNDReL0L` by replacing the `.` with backspaces. I tried against the console and it works.

I got in and obtained the flag `kooda2puivaav1idi4f57q8iq`

```
level02@SnowCrash:~$ su flag02
Password:
Don't forget to launch getflag !
flag02@SnowCrash:~$ getflag
Check flag.Here is your token : kooda2puivaav1idi4f57q8iq
flag02@SnowCrash:~$
```