## level01

I noticed the weird thing on `/etc/passwd` for this user during the last level, and Ill try to login with the weird string `42hDRfypTqqnw` for this one. 

```
level01@SnowCrash:~$ su flag01
Password:
su: Authentication failure
level01@SnowCrash:~$
```

Nope, does not work. Ill need to do more research on what this is.

Upon googleing, the string included in /etc/passwd is the encrypted user password, and usually is stored in a more secure form in /etc/shadow following this format:

![](https://hackmd.io/_uploads/H1eAbom-6.png)

So our next step is to attempt to decrypt the password using one of the following formats.

It is not MD5 since the password is not 32 Characters long.
I dont think its blowfish because we need to get a seperate key for that (could be the username). It is not the SHA algorithms either because the length of the password does not meet the encryption results. So we are left with yescrypt

I did some google and there are people trying to crack yescrypt with john the ripper, so I gave it a go and installed it 

```
wget https://www.openwall.com/john/k/john-1.9.0.tar.xz --no-check-certificate

tar -xvf john-1.9.0.tar.xz

cd john-1.9.0/src && make

```

I ran the program and got the plaintext password

![](https://hackmd.io/_uploads/Hy8CDgVb6.png)

The password turns out to be correct, and the flag is obtained `f2av5il02puano7naaf6adaaf`
```
level01@SnowCrash:/dev/shm/john-1.9.0$ su flag01
Password:
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
flag01@SnowCrash:~$
```