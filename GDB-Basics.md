# GNU Project Debugger (GDB) Basics

## 1. Overview of GNU Project Debugger (GDB)  <a name="overview"></a>

- GNU Project Debugger (GDB) is a debugging tool for C and C++. It provides the ability to trace while a program is running, as well as let we know what exactly happens when a crash occurs in our program.
- As mentioned above, GDB has 2 main use case:
	+ Control the program execution and see insides of a running program
    + Analyze the cause of a crash/error
- GDB provides a command line interface (CLI). supports multiple languages, and also allows remote debugging (using [`gdbserver`](https://developers.redhat.com/blog/2015/04/28/remote-debugging-with-gdb)) 

## 2. Using GDB to debug a C/C++ program <a name="usage"></a>

### 2.1 Prepare a program to debug <a name="progprep"></a>

Create the file `hello.c` with the following chunk of source code:

```{c}
#include <stdio.h>

int main() {
    int x;
    int a = x;
    int b = x;
    int c = a + b;
    printf("%d\n", c);
    return 0;
}
```

Next, we need to compile the source file using `gcc` by the following command:

```{bash}
gcc -g test.c -o test
```

It will then generate the `hello` executable file, which will be used for our debugging session. Notice that we compile the source file with `-g` flag, which [provide debugging feature](https://bytes.usc.edu/cs104/wiki/gcc/) for our program

### 2.2 Check GDB availability <a name="availability"></a>

First, check if our machine already have `gdb` or not. To do so, run:

```{bash}
gdb --version
```

If `gdb` is already installed, the output might seem like:

```{output}
GNU gdb (GDB; SUSE Linux Enterprise 15) 13.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

If not, install `gdb` by the following command (this may vary base on the package manager we using, mine using zypper)

```{bash}
sudo zypper install gdb
```

### 2.3 Start GDB <a name="startgdb"></a>
After ensuring that GDB is installed on the machine, we can now use it to investigate further in our compiled program

To start GDB, simply run:

```{bash}
gdb [executable_file_path]
```

Or, you can also run:

```{bash}
gdb
```

And attach the file later, after we started GDB, using the `file` command. For example:

```{bash}
file [executable_file_path]
```

If we managed to start GDB, the output should be as follows:

```{output}
GNU gdb (GDB; SUSE Linux Enterprise 15) 13.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-suse-linux".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://bugs.opensuse.org/>.
Find the GDB manual and other documentation resources online at:
--Type <RET> for more, q to quit, c to continue without paging--
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) 
```

You can also ignore the information above by using the `-q` (quiet) with the command:

```{bash}
gdb -q
```

### 2.4 GDB Basic Commands to get started <a name="basiccmds"></a>

#### `run` or `r` <a name="cmd_run"></a>
The run command to start our progarm under GDB. This command executes the progarm from start. <u>BEFORE</u> using the **run** command we need to [specify the program name](#startgdb) with an argument to GDB:
<code>gdb<code>*[executable_file_path]*</code></code> or using the <code>file<code>*[file_name]*</code></code> or <code>exec-file<code>*[file_name]*</code></code>

```{bash}
(gdb) run
```

If the program run in an execution environment that supports processe,
**run** creates an inferior process and makes that process run our program. In some environments without processes, **run** jumps to the start of our program. Except other targets like **`remote`**, are always running. If you use the run command, it will send an error message like this one:

```{bash}
 The "remote" target does not support "run".
 Try "help target" or "continue".
```
If you run without [specify the program name](#startgdb) you will start the program but can't run.

```{bash}
Starting program:  
No executable file specified.
Use the "file" or "exec-file" command.
```

#### `break` or `b` <a name="cmd_break"></a>
A **breakpoint** makes our program stop whenever a certain point in the program is reached. We can set breakpoint by using the break command and its variants, to specify the place where your program should stop by line number, function name or exact address in the program.

```{bash}
(gdb) break [locspec]
```
 Set a breakpoint at all the code locations in your program that result from resolving the given **`locspec`**. 
 
- **`locspec`** can specify a function name, a line number, an address of an instruction, and more. 

Set a breakpoint with condition cond:
```{bash}
(gdb) break [locspec] if cond
```
For example, gdb reports below that two of the three locations are disabled

```{bash}
(gdb) break func if a == 10
 warning: failed to validate condition at location 0x11ce, disabling:
 No symbol "a" in current context.
 warning: failed to validate condition at location 0x11b6, disabling:
 No symbol "a" in current context.
 Breakpoint 1 at 0x11b6: func. (3 locations)
```
Locations that are disabled because of the condition are denoted by an uppercase N in the output of the info breakpoints command:
```{bash}
 (gdb) info breakpoints
 Num    Type        Disp    Enb     Address         What
 1      breakpoint  keep    y       <MULTIPLE>
        stop only if a == 10
 1.1                        N* 0x00000000000011b6   in...
 1.2                        y 0x00000000000011c2    in...
 1.3                        N* 0x00000000000011ce   in...
 (*): Breakpointconditionisinvalidatthislocation.
```
#### `disable` <a name="cmd_disable"></a>
You can disable one or multiple breakpoints, watchpoints, tracepoints, and catchpoints by using **`disable`** command. If you disable the specified breakpoints/all breakpoints or none are listed. A disabled breakpoint has no effect but is not forgotten. You can use the following command:
```{bash}
(gdb) disable [breakpoints] [list...]
```
***Notice:** All options such as ignore-counts, conditions and commands are remembered in case the breakpoint is enabled again later.

#### `enable` <a name="cmd_enable"></a>
The opposite of **`disable`**, you can **`enable`** the breakpoint stops your program. A breakpoint set with the break command starts out in this state. There are **four** main syntax for **`enable`** and explained in detail:
```{bash}
(gdb)  enable [breakpoints] [list...]
```
**Detail:** Enable the specified breakpoints (or all defined breakpoints). They become
effective once again in stopping your program.
```{bash}
(gdb)   enable [breakpoints] once [list...]
```
**Detail:** Enable the specified breakpoints temporarily. gdb records count with each of the specified breakpoints, and decrements a breakpoint's count when it is hit. When any count reaches 0, gdb disables that breakpoint. If a breakpoint has an ignore count, that will be decremented to 0 before count is affected.
```{bash}
(gdb)  enable [breakpoints] delete [list...]
```
**Detail:** Enable the specified breakpoints to work once, then die. gdb deletes any of these breakpoints as soon as your program stops there. Breakpoints set by the <u>tbreak</u> command start out in this state.

#### `next` or `n` <a name="cmd_next"></a>
You can continue to the next source line in the current (innermost) stack frame. the command is simular to [step](#cmd_step), but function calls that appear within the line of code are executed without stopping. To test you can use the following command:
```{bash}
(gdb) next [count]
```
Execution stops when control reaches a different line of code at the original stack level that was executing when you gave the next command. This command is abbreviated n.

An argument count is a repeat count, as for step.

The next command only stops at the first instruction of a source line. This prevents multiple stops that could otherwise occur in switch statements, for loops, etc.
#### `step` <a name="cmd_step"></a>
The command is simular to [next](#cmd_next). The step command only enters a function if there is line number info for the function. If dont it will act like a [next](#cmd_next) command.
```{bash}
(gdb) step
```
- ***Warning***: If you use the step command while control is within a function that was compiled without debugging information, execution proceeds until control reaches a function that does have debugging information. Likewise, it will not step into a function which is compiled without debugging information. To step through functions without debugging information, use the step**i** command, described below.

Continue running as in step, but do so count times.
```{bash}
(gdb) step [count]
```
If a breakpoint is reached, or a signal not related to stepping occurs before count steps, stepping stops right away.

#### `list` or `l` <a name="cmd_list"></a>
To print lines from a source file, use the `list` command. By default, ten lines are printed.<br>
The forms of the list command most commonly used: <br>
- Print more lines. If the last lines printed were printed with a **list** command, this prints lines following the last lines printed; however, if the last line printed was a solitary line printed as part of displaying a stack frame, this prints lines centered around that line. If no list command has been used and no solitary line was printed, it prints the lines around the function main.
```{bash}
(gdb) list
```
- Print lines centered around line number *linenum* in the current source file.
```{bash}
(gdb) list [linenum]
```
- Print lines centered around the line or lines of all the code locations that result from resolving *locspec* (Location Specification).
```{bash}
(gdb) list [locspec]
```
- Print lines centered around the beginning of function *function*.
```{bash}
(gdb) list [function]
```
- Print lines from *first* to *last*. Both arguments are location specs. When a **list**
command has two location specs, and the source file of the second location spec
is omitted, this refers to the same source file as the first location spec. If either
*first* or *last* resolve to more than one source line in the program, then the list
command shows the list of resolved source lines and does not proceed with the
source code listing.
```{bash}
(gdb) list [first,last]
```
- Print lines starting with *first*.
```{bash}
(gdb) list [first,]
```
- Print lines ending with *last*.
Likewise, if *last* resolves to more than one source line in the program, then the
list command prints the list of resolved source lines and does not proceed with
the source code listing.
```{bash}
(gdb) list [,last]
```
- Print lines just after the lines last printed.
```{bash}
(gdb) list +
```
- Print lines just before the lines last printed.
```{bash}
(gdb) list -
```
#### `print` or `p` <a name="cmd_print"></a>
Prints the value of a given expression.
```{output}
(gdb) print [Expression]
(gdb) print $[Previous value number]
(gdb) print {[Type]}[Address]
(gdb) print [First element]@[Element count]
(gdb) print /[Format] [Expression]
```
**Expression:** Specifies the expression that will be evaluated and printed.<br>
**Previous value number:** When this format is used and i is specified as the previous value number, the **print** command will repeat the output produced by its i-th invocation.<br>
**Type/Address:** This format allows explicitly specifying the address of the evaluated expression and can be used as a shortcut to the C/C++ type conversion. E.g. **\*((int \*)p)** is equivalent to **{int}p**<br>
**First element/Element count:** This form allows interpreting the **First element** expression as an array of **Element count** sequential elements. The most common example of it is **\*argv@argc**<br>
**Format:** If specified, allows overriding the output format used by the command.<br>
The format letters supported are:<br>
x&nbsp;&nbsp;&nbsp;&nbsp;Print the binary representation of the value in hexadecimal.<br>
d&nbsp;&nbsp;&nbsp;&nbsp;Print the binary representation of the value in decimal.<br>
u&nbsp;&nbsp;&nbsp;&nbsp;Print the binary representation of the value as an decimal, as if it were unsigned.<br>
o&nbsp;&nbsp;&nbsp;&nbsp;Print the binary representation of the value in octal.<br>
t&nbsp;&nbsp;&nbsp;&nbsp;Print the binary representation of the value in binary. The letter 't' stands for "two".<br>
a&nbsp;&nbsp;&nbsp;&nbsp;Print as an address, both absolute in hexadecimal and as an offset from the
nearest preceding symbol<br>
c&nbsp;&nbsp;&nbsp;&nbsp;Print as an address, both absolute in hexadecimal and as an offset from the
nearest preceding symbol<br>
f&nbsp;&nbsp;&nbsp;&nbsp;Regard the bits of the value as a floating point number and print using typical
floating point syntax.<br>
s&nbsp;&nbsp;&nbsp;&nbsp;Regard as a string, if possible<br>
z&nbsp;&nbsp;&nbsp;&nbsp;he value is treated as an integer and printed as hexadecimal, but leading zeros are printed to pad the value to the size of the integer type.<br>
r&nbsp;&nbsp;&nbsp;&nbsp;Print using the 'raw' formatting.<br>
#### `quit` or `q` <a name="cmd_quit"></a>
Use the **quit** command to exit GDB. <br>
If you do not supply *expression*, gdb will terminate normally; otherwise it will terminate using the result of
*expression* as the error code.
```{bash}
(gdb) quit [expression]
```
#### `clear` <a name="cmd_clear"></a>
- Delete any breakpoints at the next instruction to be executed in the selected stack frame.
```{bash}
(gdb) clear
```
- Delete any breakpoint with a code location that corresponds to *locspec* (Location Specification).
```{bash}
(gdb) clear [locspec]
```
#### `continue` or `c` <a name="cmd_continue"></a>
- Resume program execution, at the address where your program last stopped; any breakpoints set at that address are bypassed.<br>
- The optional argument *ignore-count* allows you to specify a further number of times to ignore a breakpoint at this location<br>
- The argument *ignore-count* is meaningful only when your program stopped due to a breakpoint. At other times, the argument to *continue* is ignored.
```{bash}
(gdb) continue [ignore-count]
```
- Using the **continue** command before a program is started will result in an error. If you encounter it, use the **run** command to start the program
### 2.5 Let us Debug! <a name="debugsample"></a>

We will start `gdb` with the `hello` executable file that we have prepared.

```{bash}
gdb ./hello
```

After successfully start `gdb`, in the terminal, we will see `(gdb)` preceeds our cursor, which looks like this 

```{bash}
GNU gdb (GDB; SUSE Linux Enterprise 15) 13.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-suse-linux".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://bugs.opensuse.org/>.
--Type <RET> for more, q to quit, c to continue without paging--
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) 
```

Now, let's begin with displaying the source code, using `l` or `list` command:

```{bash}
(gdb) l
```

This is the output we get after running the `l` command:

```{bash}
(gdb) l
1       #include<stdio.h>
2
3       int main() {
4           int x;
5           int a = x;
6           int b = x;
7           int c = a + b;
8           printf("%d\n", c);
9           return 0;
10      }
```

Let's try to set a breakpoint at line 5

```{bash}
(gdb) b 5
```

```{output}
(gdb) b 5
Breakpoint 1 at 0x4005de: file test.c, line 5.
```

`gdb` tells us that a breakpoint is set at line 5, in test.c file

Now, let's run our program. Type in the terminal `run` or `r`:

```{bash}
(gdb) run
```

```{output}
(gdb) run
Starting program: /home/vagrant/gdb-wiki-test-prg/test 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib64/libthread_db.so.1".

Breakpoint 1, main () at test.c:5
5           int a = x;
(gdb)
```

The program starts running and it stop before our breakpoint at line 5. Now let's create another breakpoint and then we view all the breakpoints we have made. 

```{bash}
(gdb) b 8
```

```{output}
(gdb) b 8
Breakpoint 2 at 0x4005f5: file test.c, line 8.
(gdb)
```

To see all the breakpoints we have created, use the following command:

```{bash}
(gdb) info breakpoints
```

```{output}
(gdb) info breakpoints 
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x00000000004005de in main at test.c:5
        breakpoint already hit 1 time
2       breakpoint     keep y   0x00000000004005f5 in main at test.c:8
(gdb)
```

To disable a breakpoint, let's say the number 2, we use `disable` command:

```{bash}
(gdb) disable 2
(gdb) info breakpoints
```

```{output}
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x00000000004005de in main at test.c:5
        breakpoint already hit 1 time
2       breakpoint     keep n   0x00000000004005f5 in main at test.c:8
(gdb)
```
We can see the `Enb` attribute of breakpoint 2 at line 8 is `n`, which means it is now disable.

Of course we can make it available again using `enable` command:

```{bash}
(gdb) enable 2
(gdb) info breakpoints
```

```{output}
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x00000000004005de in main at test.c:5
        breakpoint already hit 1 time
2       breakpoint     keep y   0x00000000004005f5 in main at test.c:8
(gdb)
```

We can also investigate the value of `x`, using `p` or `print`

```{bash}
(gdb) p x
```

```{output}
$1 = 32767
```

By default, the format of `print` or `p` is decimal, we can print `x` in hexadecimal or binary format by using `p/x` and `p/t` (t stands for "two"):

```{bash}
(gdb) p/x x
$2 = 0x7fff
(gdb) p/t x
$3 = 111111111111111
```

More printing format can be access by this [link](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Output-Formats.html)

We can use `where` to get our current position in the source file:

```{bash}
(gdb) where
#0  main () at test.c:5
```

Step to the next line using `step` or `s`. We can also step over number of lines using `step [num_of_lines]`

```{bash}
(gdb) s
6           int b = x;
```

Examine if `a` is assigned with the same value with `x`:

```{bash}
(gdb) p a
$4 = 32767
(gdb) p x
$5 = 32767
```

Step 2 more lines to approach the `printf` function:

```{bash}
(gdb) step 2

Breakpoint 2, main () at test.c:8
8           printf("%d\n", c);
```

To step over a function, we use `next`:
```{bash}
(gdb) next
65534
9           return 0;
```
We see that it printed c to the console and jump to the next line, instead of stepping inside the function.

If there is no breakpoint, and we call `c`, it will run until the end of the program or when error occurs:

```{bash}
(gdb) c
Continuing.
[Inferior 1 (process 12867) exited normally]
(gdb)
```

When we finished using GDB, we can use `q` or `quit` command:
```{bash}
(gdb) q
```

There is a thing we should notice about the debugging process of GDB. We can easily spot that `x` in the `test.c` file is not initialized, but when we run the program using GDB multiple times, we observe the same value. It is explained [here](https://sourceware.org/pipermail/gdb/2022-February/049919.html) that GDB disable "Address space layout randomization" (ASLR), making the space layout too predictable and fail to reproduce a memory-related bug like this one. This is not a bug, but we can tackle the problem using the command below:

```{bash}
(gdb) set disable-randomization off
```

This allows the program to run in a normal, randomized environment. In fact, after re-run the program, we got the following output, which is different from the first run:

```{bash}
(gdb) set disable-randomization off
(gdb) file test
Reading symbols from test...
(gdb) r
Starting program: /home/vagrant/gdb-wiki-test-prg/test 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib64/libthread_db.so.1".
65130
[Inferior 1 (process 15735) exited normally]
(gdb)
```

We believe this above example is enough for everyone to get started with GDB. For further reading and full documentations, we suggest the reference for GDB commands [here](https://visualgdb.com/gdbreference/commands/).

## 3. References
- [GDB Command Reference](https://visualgdb.com/gdbreference/commands/)
- [Output Formats (Debugging with GDB)](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Output-Formats.html)
- [GDB (Step by Step Introduction)](https://www.geeksforgeeks.org/gdb-step-by-step-introduction/)
- [Remote debugging with GDB](https://developers.redhat.com/blog/2015/04/28/remote-debugging-with-gdb)
- [G++ Cheatsheet](https://bytes.usc.edu/cs104/wiki/gcc/)
- [Does GDB initialize uninitialized value?](https://sourceware.org/pipermail/gdb/2022-February/049919.html)
- [Chapter 8. GNU Debugger (GDB) | Red Hat Product Documentation](https://docs.redhat.com/en/documentation/red_hat_developer_toolset/10/html/user_guide/chap-gdb#sect-GDB-Prepare)
- [Sourceware GDB Documentation | GDB User Manual PDF](https://sourceware.org/gdb/current/onlinedocs/gdb.pdf)