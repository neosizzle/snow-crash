## level04 
For level04, I was greeted with a perl script with the following contents

```
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

Here is the file type

```
level04@SnowCrash:~$ file level04.pl
level04.pl: setuid setgid a /usr/bin/perl script, ASCII text executable
```

since the setuid and setgid bits are enabled, means this is another privellege escallation gig, I will need to execute getflag inside this script somehow to get the flag.


Upon doing some research on what the script does, I figured that this script is meant to be run as a back end CGI, where we need to spin up a webserver to accept requests.

The program defines a subroutine 'x' which takes a parameter which needs to be named 'x' and concats it to the echo call. Not much I can elaborate here since I am not good with perl. Need to trial and error this.

Luckily, the ISO already has Apache installed and it is already running on port `4747`

![](https://hackmd.io/_uploads/S1w_wAEWT.png)

I went port-forwarded the traffic and tried to adjust the x query parameter in my browser, and it does show me something as an output

![](https://hackmd.io/_uploads/r1lkd0VWa.png)

Looks like the print command of the perl script will be run by the server since its surrounded in backticks. In bash, backticks are called **command substitution** in bash which spawn a subshell and captures the output of the command inside the backtick and returns it to the main shell

```
level04@SnowCrash:~$ echo ls
ls
level04@SnowCrash:~$ `echo ls`
level04.pl
level04@SnowCrash:~$
```

If this is the case, I should make the x parameter
```
x=`getflag`
```
To run the getflag program as flag04 user.

To do that, the following query is sent 
```
http://localhost:4747/?x=`getflag`
```

And I got the following output, the flag is `ne2searoevaevoem4ov4ar8ap`

![](https://hackmd.io/_uploads/SkGIqAE-T.png)