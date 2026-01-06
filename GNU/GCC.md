---
title: GCC (GNU Compiler Collection)
tags: [GNU]

---

# GCC (GNU Compiler Collection)
###### tags: `GNU`

Compilation can involve up to four stages: **preprocessing**, **compilation proper**, **assembly** and **linking**, always in that order. GCC is capable of preprocessing and compiling several files either into several assembler input files, or into one assembler input file; then each assembler input file produces an object file, and linking combines all the object files (those newly compiled, and those specified as input) into an executable file.
``` console
gcc -Q -O3 --help=optimizers
```

## Compilation

### 1. Preprocessor
The output from the C preprocessor looks much like the input, except that all preprocessing directive lines have been replaced with blank lines and all comments with spaces. Long runs of blank lines are discarded.

Program: `cpp`
Filename extension of the output: `.i`

#### option
**-E**
> Stop after the preprocessing stage; do not run the compiler proper. The output is in the form of preprocessed source code, which is sent to the standard output.
> 
> Input files that don’t require preprocessing are ignored.

**-P**

#### C89/C99
``` c
offsetof()
```


#### C11
``` c
/**
 * _Generic()
 */

#include <stdio.h>
#include <math.h>

#define cbrt(X) \
    _Generic((X), \     
             long double: cbrtl, \
             default: cbrt,  \
             const float: cbrtf, \
             float: cbrtf  \
    )(X)

int main(void)
{
    double x = 8.0;
    const float y = 3.375;
    printf("cbrt(8.0) = %f\n", cbrt(x));
    printf("cbrtf(3.375) = %f\n", cbrt(y));
}
```

``` c
 __typeof__(x)
```


### 2. Compiler
converts the **C code** into **assembly code**.

Program: `cc1`
Filename extension of the output: `.s`


show include path:
```
$ gcc -Wp,-v
```

Instead of outputting the result of preprocessing, output a rule suitable for make describing the dependencies of the main source file:
```
$ gcc -M
```

-fomit-frame-pointer
> Omit the frame pointer in functions that don’t need one. This avoids the instructions to save, set up and restore the frame pointer; on many targets it also makes an extra register available.
> 
> This flag is working on AArch32. It can see in core dump, **the stack frame will be not trace able**. That is, the GDB command, backtrace, can not provide useful information.
> 
> On some targets this flag has no effect because the standard calling sequence always uses a frame pointer, so it cannot be omitted.
> 
> Note that -fno-omit-frame-pointer doesn’t guarantee the frame pointer is used in all functions. Several targets always omit the frame pointer in leaf functions.
> 
> Enabled by default at -O1 and higher.

#### GNU extension
``` c
__attribute__((naked))
__attribute__((interrupt))
```

### 3. Assembler
Converts the **assembly code** into **object files**

Program: `as`
Filename extension of the output: `.o`


### 4. Linker
Collects **object files** into **executable file** or a **shared library**.

Program: `collect2` `dynamic linker (ld)`
Filename extension of the output: `.out` `.elf`

> The **dynamic linker** is responsible for loading dynamically linked programs and their dependencies (in the form of shared objects). The dynamic linker in the GNU C Library also supports loading shared objects (such as plugins) later at run time. Dynamic linkers are sometimes called **dynamic loaders**.


dump link script:
``` console
$ gcc -Wl,-verbose main.c
```


**-T *script***:
> Use script as the linker script. This option is supported by most systems using the GNU linker. On some targets, such as bare-board targets without an operating system, the -T option may be required when linking to avoid references to undefined symbols.

**-specs=*file***:
> Process file after the compiler reads in the standard specs file, in order to override the defaults which the gcc driver program uses when determining what switches to pass to cc1, cc1plus, as, ld, etc. More than one -specs=file can be specified on the command line, and they are processed in order, from left to right. See Spec Files, for information about the format of the file.


## C Extenstions
[6 Extensions to the C Language Family](https://gcc.gnu.org/onlinedocs/gcc/C-Extensions.html)

void * __builtin_return_address (*unsigned int level*)
> This function returns the return address of the current function, or of one of its callers. The level argument is number of frames to scan up the call stack. A value of 0 yields the return address of the current function, a value of 1 yields the return address of the caller of the current function, and so forth. When inlining the expected behavior is that the function returns the address of the function that is returned to. To work around this behavior use the noinline function attribute.


### 6.12 How to Use Inline Assembly Language in C Code



## Variable Attriables
``` c
__attribute__((packed))

// This attribute specifies a minimum alignment (in bytes) for variables of the specified type. 
unsigned int __attribute__((aligned(0x1000))) page_table_root [256][1024] = {0};


__attribute__((section(".isr_vector"))) uint32_t *isr_vectors[] = {...};
```

## Memory ordering

* Memory barrier

`asm volatile("": : :"memory")` 

Inline asm. actually does nothing at runtime, there's no command performed there and the CPU doesn't know about it. it only serves at compile time, to tell the compiler not to move loads or stores beyond this point (in any direction) as part of its optimizations. It's called a **SW barrier**.


`__sync_synchronize()`

Builtin sync. would simply translate into a **HW barrier**, probably a fence (mfence/sfence) operations if you're on x86, or its equivalents in other architectures. The CPU may also do various optimizations at runtime, the most important one is actually performing operations out-of-order - this instruction tells it to make sure that loads or stores can't pass this point and must be observed in the correct side of the sync point.

## Pragmas

## Reference
[How A Compiler Works: GNU Toolchain](https://www.slideshare.net/jserv/how-a-compiler-works-gnu-toolchain)

[GCC Wiki](https://gcc.gnu.org/wiki/HomePage)

[GCC online documentation](https://gcc.gnu.org/onlinedocs/)# GCC (GNU Compiler Collection)
###### tags: `GNU`

Compilation can involve up to four stages: **preprocessing**, **compilation proper**, **assembly** and **linking**, always in that order. GCC is capable of preprocessing and compiling several files either into several assembler input files, or into one assembler input file; then each assembler input file produces an object file, and linking combines all the object files (those newly compiled, and those specified as input) into an executable file.
``` console
gcc -Q -O3 --help=optimizers
```

## Compilation

### 1. Preprocessor
The output from the C preprocessor looks much like the input, except that all preprocessing directive lines have been replaced with blank lines and all comments with spaces. Long runs of blank lines are discarded.

Program: `cpp`
Filename extension of the output: `.i`

#### option
**-E**
> Stop after the preprocessing stage; do not run the compiler proper. The output is in the form of preprocessed source code, which is sent to the standard output.
> 
> Input files that don’t require preprocessing are ignored.

**-P**

#### C89/C99
``` c
offsetof()
```


#### C11
``` c
/**
 * _Generic()
 */

#include <stdio.h>
#include <math.h>

#define cbrt(X) \
    _Generic((X), \     
             long double: cbrtl, \
             default: cbrt,  \
             const float: cbrtf, \
             float: cbrtf  \
    )(X)

int main(void)
{
    double x = 8.0;
    const float y = 3.375;
    printf("cbrt(8.0) = %f\n", cbrt(x));
    printf("cbrtf(3.375) = %f\n", cbrt(y));
}
```

``` c
 __typeof__(x)
```


### 2. Compiler
converts the **C code** into **assembly code**.

Program: `cc1`
Filename extension of the output: `.s`


show include path:
```
$ gcc -Wp,-v
```

Instead of outputting the result of preprocessing, output a rule suitable for make describing the dependencies of the main source file:
```
$ gcc -M
```

-fomit-frame-pointer
> Omit the frame pointer in functions that don’t need one. This avoids the instructions to save, set up and restore the frame pointer; on many targets it also makes an extra register available.
> 
> This flag is working on AArch32. It can see in core dump, **the stack frame will be not trace able**. That is, the GDB command, backtrace, can not provide useful information.
> 
> On some targets this flag has no effect because the standard calling sequence always uses a frame pointer, so it cannot be omitted.
> 
> Note that -fno-omit-frame-pointer doesn’t guarantee the frame pointer is used in all functions. Several targets always omit the frame pointer in leaf functions.
> 
> Enabled by default at -O1 and higher.

#### GNU extension
``` c
__attribute__((naked))
__attribute__((interrupt))
```

### 3. Assembler
Converts the **assembly code** into **object files**

Program: `as`
Filename extension of the output: `.o`


### 4. Linker
Collects **object files** into **executable file** or a **shared library**.

Program: `collect2` `dynamic linker (ld)`
Filename extension of the output: `.out` `.elf`

> The **dynamic linker** is responsible for loading dynamically linked programs and their dependencies (in the form of shared objects). The dynamic linker in the GNU C Library also supports loading shared objects (such as plugins) later at run time. Dynamic linkers are sometimes called **dynamic loaders**.


dump link script:
``` console
$ gcc -Wl,-verbose main.c
```


**-T *script***:
> Use script as the linker script. This option is supported by most systems using the GNU linker. On some targets, such as bare-board targets without an operating system, the -T option may be required when linking to avoid references to undefined symbols.

**-specs=*file***:
> Process file after the compiler reads in the standard specs file, in order to override the defaults which the gcc driver program uses when determining what switches to pass to cc1, cc1plus, as, ld, etc. More than one -specs=file can be specified on the command line, and they are processed in order, from left to right. See Spec Files, for information about the format of the file.


## C Extenstions
[6 Extensions to the C Language Family](https://gcc.gnu.org/onlinedocs/gcc/C-Extensions.html)

void * __builtin_return_address (*unsigned int level*)
> This function returns the return address of the current function, or of one of its callers. The level argument is number of frames to scan up the call stack. A value of 0 yields the return address of the current function, a value of 1 yields the return address of the caller of the current function, and so forth. When inlining the expected behavior is that the function returns the address of the function that is returned to. To work around this behavior use the noinline function attribute.


### 6.12 How to Use Inline Assembly Language in C Code



## Variable Attriables
``` c
__attribute__((packed))

// This attribute specifies a minimum alignment (in bytes) for variables of the specified type. 
unsigned int __attribute__((aligned(0x1000))) page_table_root [256][1024] = {0};


__attribute__((section(".isr_vector"))) uint32_t *isr_vectors[] = {...};
```

## Memory ordering

* Memory barrier

`asm volatile("": : :"memory")` 

Inline asm. actually does nothing at runtime, there's no command performed there and the CPU doesn't know about it. it only serves at compile time, to tell the compiler not to move loads or stores beyond this point (in any direction) as part of its optimizations. It's called a **SW barrier**.


`__sync_synchronize()`

Builtin sync. would simply translate into a **HW barrier**, probably a fence (mfence/sfence) operations if you're on x86, or its equivalents in other architectures. The CPU may also do various optimizations at runtime, the most important one is actually performing operations out-of-order - this instruction tells it to make sure that loads or stores can't pass this point and must be observed in the correct side of the sync point.

## Pragmas

## Reference
[How A Compiler Works: GNU Toolchain](https://www.slideshare.net/jserv/how-a-compiler-works-gnu-toolchain)

[GCC Wiki](https://gcc.gnu.org/wiki/HomePage)

[GCC online documentation](https://gcc.gnu.org/onlinedocs/)