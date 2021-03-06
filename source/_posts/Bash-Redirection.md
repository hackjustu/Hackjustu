# Bash Redirection and Piping

title: Bash Redirection and Piping
categories: Command Lines
date: 2015-11-16 16:08:09
tags:
- CommandLine
- Bash

---

Every program we run on the command line automatically has three data streams connected to it: `stdin`, `stdout` and `stderr`.  Piping and redirection is the means by which we may connect these streams between programs and files to direct data in interesting and useful ways.

<!--more-->

>**Note:** `0` stands for `stdin`, `1` stands for  `stdout` and `2` for `stderr`.

## Redirection
For bash redirection, basically we can:

- redirect `stdout` to a file
- redirect `stderr` to a file
- redirect `stdout` to a `stderr`
- redirect `stderr` to a `stdout`
- redirect `stderr` and `stdout` to a file

### stdout 2 file
This will cause the ouput of a program to be written to a file.
``` bash
ls -l > test.txt
```

### stderr 2 file
This will cause the `stderr` ouput of a program to be written to a file.
``` bash
grep da * 2> grep-errors.txt
```

### stdout 2 stderr
This will cause the `stdout` ouput of a program to be written to the same filedescriptor of `stderr`.
``` bash
grep da * 1>&2
```

### stderr 2 stdout
This will cause the `stderr` ouput of a program to be written to the same filedescriptor of `stdout`.
``` bash
grep * 2>&1
```

### stderr and stdout 2 file
This will place every output of a program to a file. This is suitable sometimes for cron entries, if you want a command to pass in absolute silence.
``` bash
grep * &> test.txt
```

## Piping
Piping is the mechanism for sending data from one program to another. The `|` operator feeds the output from the program on the left to the program on the right.
``` bash
ls | head -3
```
We can pipe multiple programs.
``` bash
ls | head -3 | tail -1
```
Let's combine the redirection with piping.
``` bash
ls | head -3 | tail -1 > myoutput.txt
```

### Keep the exit status while piping
Usually piping will lose the exit status of the commands before `|` operator. To preserve the previous exit status, we need to turn on the `pipefail` option.

From the [Bash Reference Manual](http://www.gnu.org/software/bash/manual/bashref.html):
> The exit status of a pipeline is the exit status of the last command in the pipeline, unless the `pipefail` option is enabled (see The [Set Builtin](http://www.gnu.org/software/bash/manual/bashref.html#The-Set-Builtin)). If `pipefail` is enabled, the pipeline's return status is the value of the last (rightmost) command to exit with a non-zero status, or zero if all commands exit successfully.

**Example:**

```bash
$ false | tee /dev/null ; echo $?
0
$ set -o pipefail
$ false | tee /dev/null ; echo $?
1
```

*Original link: [StackOverflow](http://stackoverflow.com/a/6872163/3697757)*
