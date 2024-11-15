# **Autotools**

## **Autoconf** <a name="autoconf"></a>

### **1. Introduction** <a name="autoconf_introduction"></a>

#### **1.1 Definition** <a name="autoconf_definition"></a>

- **Autoconf** is a tool for producing shell scripts that automatically configure software source code packages to adapt to many kinds of Posix-like systems.

#### **1.2 Advantage** <a name="autoconf_advantage"></a>

- The configuration scripts produced by **Autoconf** are independent of Autoconf when they are run, so their users do not need to have Autoconf.
- Make **configure** painless, portable, and predictable for the end user of each *autoconfiscated* package.
- **Autoconf** scripts can support cross-compiling, if some care is taken in writing them.

#### **1.3 Disadvantage** <a name="autoconf_disadvantage"></a>

- Lots of difficulties in writing **Autoconf** input.
- **Autoconf** does not solve all problems related to making portable software packages.

### **2. Components** <a name="autoconf_component"></a>

#### **2.1 `configure` scripts** <a name="autoconf_configure_script"></a>

- The configuration scripts that Autoconf produces are by convention called **configure**.
- When run, **configure** creates several files:
    + one or more **Makefile** files.
    + optionally, a C header file.
    + a shell script called **config.status**.
    + an optional shell script normally called **config.cache**.
    + a file called **config.log** containing any messages produced by compilers.

#### **2.2 `configure.ac`** <a name="autoconf_configure_input"></a>

- To produce a **configure** script for a software package, create a file called **configure.ac** that contains invocations of the **Autoconf** macros that test the system features your package needs or can use.
- Standard **configure.ac** Layout:
```{bash}
# Autoconf requirements
AC_INIT(package, version, bug-report-address)
# information on the package
# checks for programs
# checks for libraries
# checks for header files
# checks for types
# checks for structures
# checks for compiler characteristics
# checks for library functions
# checks for system services
AC_CONFIG_FILES([file…])
AC_OUTPUT
```

### **3. Commands** <a name="autoconf_command"></a>

#### **3.1 `autoconf`** <a name="autoconf_command_1"></a>

- To create **configure** from **configure.ac**, run the **autoconf** program with no arguments.
- If you give **autoconf** an argument, it reads that file instead of **configure.ac** and writes the configuration script to the standard output instead of to **configure**.
- If you give **autoconf** the argument -, it reads from the standard input instead of **configure.ac** and writes the configuration script to the standard output.
- autoconf accepts the following options:<br>

**`--help`** | **`-h`** <br>
Print a summary of the command line options and exit.<br>

**`--version`** | **`-V`** <br>
Print the version number of Autoconf and exit.<br>

**`--verbose`** | **`-v`** <br>
Report processing steps.<br>

**`--debug`** | **`-d`** <br>
Don't remove the temporary files.<br>

**`--force`** | **`-f`** <br>
Remake **configure** even if newer than its input files.<br>

**`--include=dir`** | **`-I dir`** <br>
Append *dir* to the include path. Multiple invocations accumulate.<br>

**`--prepend-include=dir`** | **`-B dir`** <br>
Prepend *dir* to the include path. Multiple invocations accumulate.<br>

**`--output=file`** | **`-o file`** <br>
Save output (script or trace) to *file*. The file - stands for the standard output.<br>

**`--warnings=category[,category...]`** | **`-Wcategory[,category...]`**<br>
Enable or disable warnings related to each category.<br>

**`--trace=macro[:format]`** | **`-t macro[:format]`**<br>
Do not create the **configure** script, but list the calls to *macro* according to the *format*.<br>

**`--initialization`** | **`-i`**<br>
By default, **--trace** does not trace the initialization of the Autoconf macros.<br>

#### **3.2 autoreconf** <a name="autoconf_command_2"></a>

- **autoreconf** runs **autoconf**, **autoheader**, **aclocal**, **automake**, **libtoolize**, **intltoolize**, **gtkdocize**, and **autopoint** (when appropriate) repeatedly to update the GNU Build System in the specified directories and their subdirectories.
- If you install a new version of some tool, you can make **autoreconf** remake *all* of the files by giving it the **--force** option.
- **autoreconf** accepts the following options:<br>

**`--help`** | **`-h`** <br>
Print a summary of the command line options and exit.<br>

**`--version`** | **`-V`** <br>
Print the version number of Autoconf and exit.<br>

**`--verbose`** | **`-v`** <br>
Print the name of each directory **autoreconf** examines and the commands it runs. If given two or more times, pass **--verbose** to subordinate tools that support it. <br>

**`--debug`** | **`-d`** <br>
Don't remove the temporary files.<br>

**`--force`** | **`-f`** <br>
Consider all generated and standard auxiliary files to be obsolete.<br>

**`--install`** | **`-i`** <br>
Install any missing standard auxiliary files in the package.<br>

**`--no-recursive`** <br>
Do not rebuild files in subdirectories to configure.<br>

**`--symlink`** | **`-s`** <br>
When used with **--install**, install symbolic links to the missing auxiliary files instead of copying them.
<br>

**`--make`** | **`-m`** <br>
When the directories were configured, update the configuration by running '**./config.status --recheck && ./config.status**', and then run '**make**'.<br>

**`--include=dir`** | **`-I dir`** <br>
Append dir to the include path. Multiple invocations accumulate. Passed on to **aclocal**, **autoconf** and **autoheader** internally.<br>

**`--prepend-include=dir`** | **`-B dir`** <br>
Prepend *dir* to the include path. Multiple invocations accumulate. Passed on to **autoconf** and **autoheader** internally.<br>

**`--warnings=category[,category...]`** | **`-Wcategory[,category...]`** <br>
Enable or disable warnings related to each *category*.<br>

## **Automake** <a name="automake"></a>

### **1. Introduction** <a name="automake_introduction"></a>

#### **1.1 Definition** <a name="automake_definition"></a>

- Automake is a tool for automatically generating **Makefile.ins** from files called **Makefile.am**.
- Each **Makefile.am** is basically a series of **make** variable definitions1, with rules being thrown in occasionally.
- The generated **Makefile.ins** are compliant with the GNU Makefile standards. 

#### **1.2 Advantage** <a name="automake_advantage"></a>

- Remove the burden of **Makefile** maintenance from the back of the individual GNU maintainer (and put it on the back of the **Automake** maintainers). 
- The distributions created by **Automake** are fully GNU standards-compliant, and do not require **perl** in order to be built.

#### **1.3 Disadvantage** <a name="automake_disadvantage"></a>

- **Automake** assumes that the project uses **Autoconf**.
- **Automake** enforces certain restrictions on the **configure.ac** contents.
- **Automake** requires perl in order to generate the **Makefile.ins**.

# **Reference**
- [Autoconf](https://www.gnu.org/savannah-checkouts/gnu/autoconf/manual/autoconf-2.72/html_node/index.html#Top)
- [GNU Automake](https://www.gnu.org/software/automake/manual/html_node/index.html#toc-Introduction-1)