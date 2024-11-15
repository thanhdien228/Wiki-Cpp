# GCC's extended compilation pipeline
# Table of contents
1. [Overview](#overview)
2. [Design](#design)
3. [The four stages of the GCC](#stages)
	1. [Preprocessor](#prep)
    2. [Compiler](#comp)
    3. [Assembler](#asm)
    4. [Linker](#lnk)
4. [References](#ref)

## Overview <a name="overview"></a>
- The **GNU Compiler Collection (GCC)** is a set of compilers and development tools available for Linux, Windows, various BSDs, and a wide assortment of other operating systems. 
- It includes support primarily for C and C++ and includes Objective-C, Ada, Go, Fortran, and D.
- The Free Software Foundation (FSF) wrote GCC and released it as completely free (as in libre) software.
- It is a **toolchain** that **compiles** code, **links** it with any library dependencies, **converts** that code to assembly, and then **prepares executable files**.

## Design <a name="design"></a>

Each of the language compilers is a separate program that reads source code and outputs machine code. All have a common internal structure. A **per-language front end** parses the source code in that language and produces an abstract syntax tree. Due to the syntax tree abstraction, source files of any of the different supported languages can be processed by the same **back end**.

GENERIC is an intermediate representation language used as a **middle end** while compiling source code into executable binaries. The middle stage of GCC does all of the code analysis and optimization, working independently of both the compiled language and the target architecture, starting from the GENERIC representation and expanding it to register transfer language (RTL).

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/Compiler_design.svg/1920px-Compiler_design.svg.png" />
</p>


## The four stages of the GCC <a name="stages"></a>

GCC's external interface follows Unix conventions. Users invoke a language-specific driver program (gcc for C, g++ for C++, etc.), which interprets command arguments, calls the actual compiler, runs the assembler on the output, and then optionally runs the linker to produce a complete executable binary.

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/b/b6/GCC_Translation_Diagram.jpg" />
</p>

To start the process, we need to give the compiler a file as input, so we create a source file `main.cpp` and write a message, “Hello world”.

```{bash}
vi main.cpp
```
```
#include <iostream>
using namespace std;

void main(){
    cout << "Hello World" <<endl;
}
```
Now that we have the source file, we can make it an executable by simply calling the gcc command and naming the file:

```{bash}
gcc main.cpp
```

However, since we want to see what happens after each stage, we will have the process with the following flags:

-   `-E` : Stop after the preprocessing stage; do not run the compiler proper.
-   `-S` : Stop after the stage of compilation proper; do not assemble.
-   `-c` : Compile or assemble the source files, but do not link.
-   `-o` `<file>`: Place output in file `<file>`. This applies regardless to whatever sort of output is being produced, whether it be an executable file, an object file, an assembler file or preprocessed C code. If -o is not specified, the default is to put an executable file in `a.out`

```{bash}
gcc -E main.cpp
```
**This first step** generates the extension code that looks like this:
```{bash}
   const size_t __clen = char_traits<char>::length(__s);
   try
     {
       struct __ptr_guard
       {
  _CharT *__p;
  __ptr_guard (_CharT *__ip): __p(__ip) { }
  ~__ptr_guard() { delete[] __p; }
  _CharT* __get() { return __p; }
       } __pg (new _CharT[__clen]);

       _CharT *__ws = __pg.__get();
       for (size_t __i = 0; __i < __clen; ++__i)
  __ws[__i] = __out.widen(__s[__i]);
       __ostream_insert(__out, __ws, __clen);
     }
   catch(__cxxabiv1::__forced_unwind&)
     {
       __out._M_setstate(ios_base::badbit);
       throw;
     }
   catch(...)
     { __out._M_setstate(ios_base::badbit); }
 }
      return __out;
    }




  extern template class basic_ostream<char>;
  extern template ostream& endl(ostream&);
  extern template ostream& ends(ostream&);
  extern template ostream& flush(ostream&);
  extern template ostream& operator<<(ostream&, char);
  extern template ostream& operator<<(ostream&, unsigned char);
  extern template ostream& operator<<(ostream&, signed char);
  extern template ostream& operator<<(ostream&, const char*);
  extern template ostream& operator<<(ostream&, const unsigned char*);
  extern template ostream& operator<<(ostream&, const signed char*);
```

In the second stage, the main task is to let the compiler check your program for any grammatical errors. When your program has no problems, compilation will bring the programming of your program closer to machine language assembly language.

 ```{bash}
gcc -S main.cpp
 ```
**This second** step generates assembly code and if we check the main.s file, the content is an intermediate language that can be read by humans.
```{bash}
        .file   "main.cpp"
        .text
#APP
        .globl _ZSt21ios_base_library_initv
        .section        .rodata
.LC0:
        .string "Hello World"
#NO_APP
        .text
        .globl  main
        .type   main, @function
main:
.LFB1988:
        .cfi_startproc
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        movl    $.LC0, %esi
        movl    $_ZSt4cout, %edi
        call    _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc
        movl    $_ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_, %esi
        movq    %rax, %rdi
        call    _ZNSolsEPFRSoS_E
        movl    $0, %eax
        popq    %rbp
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE1988:
        .size   main, .-main
        .section        .rodata
        .type   _ZNSt8__detail30__integer_to_chars_is_unsignedIjEE, @object
        .size   _ZNSt8__detail30__integer_to_chars_is_unsignedIjEE, 1
_ZNSt8__detail30__integer_to_chars_is_unsignedIjEE:
        .byte   1
        .type   _ZNSt8__detail30__integer_to_chars_is_unsignedImEE, @object
        .size   _ZNSt8__detail30__integer_to_chars_is_unsignedImEE, 1
_ZNSt8__detail30__integer_to_chars_is_unsignedImEE:
        .byte   1
        .type   _ZNSt8__detail30__integer_to_chars_is_unsignedIyEE, @object
        .size   _ZNSt8__detail30__integer_to_chars_is_unsignedIyEE, 1
_ZNSt8__detail30__integer_to_chars_is_unsignedIyEE:
        .byte   1
        .ident  "GCC: (SUSE Linux) 13.3.0"
        .section        .note.GNU-stack,"",@progbits
```

**The third stage** is the assembly stage, this stage is to convert the assembly code generated in the second stage into our executable file, which is to convert our assembly language into a machine language that can be executed by our machine.

```{bash}
g++ -c main.cpp
```

-c means let our program execute the third stage to generate a machine language that the machine can understand. At this point, we can see the .o file generated by ls.
```
$ cat -v main.o
^?ELF^B^A^A^@^@^@^@^@^@^@^@^@^A^@>^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^P^B^@^@^@^@^@^@^@^@^@^@@^@^@^@^@^@@^@^M^@^L^@UHM-^IM-eM-?^@^@^@^@M-8^@^@^@^@M-h^@^@^@^@M-8^@^@^@^@]M-Chello word^@^@GCC: (SUSE Linux) 13.3.0^@^@^T^@^@^@^@^@^@^@^AzR^@^Ax^P^A^[^L^G^HM-^P^A^@^@^\^@^@^@^\^@^@^@^@^@^@^@^Z^@^@^@^@A^N^PM-^F^BC^M^FU^L^G^H^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^D^@M-qM-^?^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^C^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^C^@^E^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^H^@^@^@^R^@^A^@^@^@^@^@^@^@^@^@^Z^@^@^@^@^@^@^@^M^@^@^@^P^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@main.c^@main^@printf^@^@^@^@^@^E^@^@^@^@^@^@^@
^@^@^@^C^@^@^@^@^@^@^@^@^@^@^@^O^@^@^@^@^@^@^@^B^@^@^@^E^@^@^@M-|M-^?M-^?M-^?M-^?M-^?M-^?M-^? ^@^@^@^@^@^@^@^B^@^@^@^B^@^@^@^@^@^@^@^@^@^@^@^@.symtab^@.strtab^@.shstrtab^@.rela.text^@.data^@.bss^@.rodata^@.comment^@.note.GNU-stack^@.rela.eh_frame^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@ ^@^@^@^A^@^@^@^F^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@@^@^@^@^@^@^@^@^Z^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^[^@^@^@^D^@^@^@@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@`^A^@^@^@^@^@^@0^@^@^@^@^@^@^@
^@^@^@^A^@^@^@^H^@^@^@^@^@^@^@^X^@^@^@^@^@^@^@&^@^@^@^A^@^@^@^C^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@Z^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@,^@^@^@^H^@^@^@^C^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@Z^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@1^@^@^@^A^@^@^@^B^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@Z^@^@^@^@^@^@^@^K^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@9^@^@^@^A^@^@^@0^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@e^@^@^@^@^@^@^@^Z^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@B^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^?^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@W^@^@^@^A^@^@^@^B^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@M-^@^@^@^@^@^@^@^@8^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^H^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@R^@^@^@^D^@^@^@@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@M-^P^A^@^@^@^@^@^@^X^@^@^@^@^@^@^@
^@^@^@^H^@^@^@^H^@^@^@^@^@^@^@^X^@^@^@^@^@^@^@^A^@^@^@^B^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@M-8^@^@^@^@^@^@^@M-^P^@^@^@^@^@^@^@^K^@^@^@^D^@^@^@^H^@^@^@^@^@^@^@^X^@^@^@^@^@^@^@       ^@^@^@^C^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@H^A^@^@^@^@^@^@^T^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^Q^@^@^@^C^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@M-(^A^@^@^@^@^@^@a^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^A^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@vagrant@opens
```

**Finally**, functions that have been written, up to this point, if we don't bind all of them independently when we execute them, the program will not be able to execute correctly, so we have to connect in the fourth step.

```{bash} 
g++ main.cpp -o mysystem
```

Now that the build is complete, we can run the executable and get the "Hello World" message with this `. /`
```{bash}
./mysystem 
 hello word 
```

### 3.1 Preprocessor <a name="prep"></a>
In this phase, a program called the **preprocessor** makes various changes to the text of the code file. 
 
 The preprocessor does not actually modify the original code files in any way - it only handles **directives** that modify original code before actual compilation. All changes made by the preprocessor happen either temporarily in-memory or using temporary files.

#### Example of Preprocessor Directives

***File Includings***

```{cpp}
#include <iostream>
#include "myfile.h"
```

When `#include` a file, the preprocessor replaces the `#include` directive with the contents of the included file. The included contents are then preprocessed (which may result in additional includes being preprocessed recursively), then the rest of the file is preprocessed.


***Macro Definitions***

```{cpp}
#define PI 3.14159262
#define SQUARE(x) (x * x)
```

The `#define` directive can be used to create a macro. In `C++`, a **macro** is a rule that defines how input text is converted into replacement output text. There are two basic types of macros: **object-like macros**, and **function-like macros**. 

Preprocessor macros have no scope, as they are not part of the C language. Instead it's a kind of search-replace program that is run before the compiler proper runs. The preprocessor simply goes though any file, and when it finds a macro invocation it simply replaces it with the text in the macro body.

***Conditional Inclusions***
```{cpp}
void doSth() {
    #ifdef PRINT
        std::cout << "Printing!\n";
    #endif
    #ifndef PRINT
        std::cout << "Not printing!\n";
    #endif
}
#define PRINT
int main() {
    doSth();
    return 0;
}
// Output: Not printing!
```

The conditional compilation preprocessor directives allow you to specify under what conditions something will or won't compile. The `#ifdef`/`#ifndef` preprocessor directive allows the preprocessor to check whether an identifier has (been defined/not been defined) yet. 

### 3.2 Compiler<a name="comp"></a>
In order to compile C++ source code files, we use a C++ **compiler**. The C++ compiler sequentially goes through each source code (.cpp) file in your program and does two important tasks:

First, the compiler checks your C++ code to make sure it follows the rules of the C++ language. If it does not, the compiler will give you an error (and the corresponding line number) to help pinpoint what needs fixing. The compilation process will also be aborted until the error is fixed.

Second, the compiler translates your C++ code into machine language instructions. These instructions are stored in an intermediate file called an object file. The object file also contains metadata that is required or useful in subsequent steps.

Object files are typically named **name.o** or **name.obj**, where name is the same name as the .cpp file it was produced from.

For example, if your program had 3 .cpp files, the compiler would generate 3 object files:

<p align="center">
  <img src="https://www.learncpp.com/images/CppTutorial/Chapter0/CompileSource-min.png?ezimgfmt=rs:421x161/rscb2/ng:webp/ngcb2" />
</p>

### 3.3  Assembler <a name="asm"></a>

Assembler is a program for converting instructions written in low-level assembly code into relocatable machine code and generating along information for the loader. It is necessary to convert user-written programs into machinery code. This is called a translation of a high-level language to a low-level that is machinery language. This type of translation is performed with the help of system software. An Assembler can be defined as a program that translates an assembly language program into a machine language program. Self-assembler is a program that runs on a computer and produces the machine codes for the same computer or same machine. It is also known as a resident assembler. A cross-assembler is an assembler that runs on a computer and produces machine codes for other computers.

<p align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/uploads/20190301161722/Screenshot-2019-03-01-at-4.13.08-PM.png" />
</p>

The assembler generates instructions by evaluating the mnemonics (symbols) in the operation field and finding the value of symbols and literals to produce machine code. On the basis of this functionality, assembler has two types:

-	**Single-Pass Assembler**: If an assembler does all this work in one scan then it is called a single-pass assembler.
-	**Multiple-Pass Assembler**: If it does it in multiple scans then called a multiple-pass assembler.

***Working of Assembler***

Assembler divides tasks into two passes:

**Pass-1**
-	Define symbols and literals and remember them in the symbol table and literal table respectively.
-	Keep track of the location counter.
-	Process pseudo-operations.
-	Defines a program that assigns the memory addresses to the variables and translates the source code into machine code.

**Pass-2**
-	Generate object code by converting symbolic op-code into respective numeric op-code.
-	Generate data for literals and look for values of symbols.
-	Defines a program that reads the source code two times.
-	It reads the source code and translates the code into object code.

### 3.4  Linker <a name="lnk"></a>

The linker's job is to combine all of the object files (generated by a compiler or an assembler) and produce the desired output file (such as an executable file).

- **First**, the linker reads in each of the **object files** generated by the compiler and makes sure they are valid.
- **Second**, the linker ensures all **cross-file dependencies** are resolved properly. For example, if you define something in one .cpp file, and then use it in a different .cpp file, the linker connects the two together.
- **Third**, the linker also is capable of linking **library files**. A library file is a collection of precompiled code that has been “packaged up” for reuse in other programs.
- **Finally**, the linker outputs the desired output, typically **an executable file**.

Also, we have 2 mechanism for linking: 
- **Dynamic Linking**: The executable code still contains undefined symbols, plus a list of objects or libraries that will provide definitions for these. **Loading** the program will load these objects/libraries as well, and perform a final linking.
- **Static Linking**: Result of the linker **copying** all library routines used in the program into the executable image.


## References
- [Using the GCC](https://gcc.gnu.org/onlinedocs/gcc-13.3.0/gcc.pdf) 
- [GNU Compiler Collection](https://en.wikipedia.org/wiki/GNU_Compiler_Collection)
- [Assembler](https://www.geeksforgeeks.org/introduction-of-assembler/)
- [The four stages of the GCC](https://medium.com/@gpradinett/the-four-stages-of-the-gcc-compiler-preprocessor-compiler-assembler-linker-3dec8714bb9c)