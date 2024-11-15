When a C++ program is compiled, the compiler generates object code. After generating the object code, the compiler also invokes linker. One of the main tasks for linker is to make code of library functions available to your program.

A linker can accomplish this task in two ways, by copying the code of library function to your object code, or by making some arrangements so that the complete code of library functions is not copied, but made available at run-time.

## Static Library

A Static library or statically-linked library is a set of routines, external functions and variables which are resolved in a caller at compile-time and copied into a target application by a compiler, linker, producing an object file and a stand-alone executable. This executable and the process of compiling it are both known as a static build of the program.

All the code relating to the library is in this file, and it is directly linked into the program at compile time. A program using a static library takes copies of the code that it uses from the static library and makes it part of the program.

## Shared Library

Shared libraries are .so files. These are linked dynamically simply including the address of the library (whereas static linking is a waste of space). Dynamic linking links the libraries at the run-time. Thus, all the functions are in a special place in memory space, and every program can access them, without having multiple copies of them.

All the code relating to the library is in this file, and it is referenced by programs using it at run-time. A program using a shared library only makes reference to the code that it uses in the shared library.


| Properties            | Static Library                                                                                        | Shared Library             
|-----------------------|-------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Linking time          | It happens as the last step of the compilation process. After the program is placed in the memory     | Shared libraries are added during linking process when executable file and libraries are added to the memory.
| Means                 | Performed by linkers                                                                                  | Performed by operating System  
| Size                  | Static libraries are much bigger in size, because external programs are built in the executable file. | Dynamic libraries are much smaller, because there is only one copy of dynamic library that is kept in memory.
| External file changes | Executable file will have to be recompiled if any changes were applied to external files.             | In shared libraries, no need to recompile the executable.
| Time                  | Takes longer to execute, because loading into the memory happens every time while executing.          | It is faster because shared library code is already in the memory.
| Compatibility         | Never has compatibility issue, since all code is in one executable module.                            | Programs are dependent on having a compatible library. Dependent program will not work if library gets removed from the system . 

## Build Shared Library Autotools

### Directory Tree



