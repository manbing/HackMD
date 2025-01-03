---
title: GCC (GNU Compiler Collection)
tags: [GNU]

---

# GCC (GNU Compiler Collection)
###### tags: `GNU`

Compilation can involve up to four stages: preprocessing, compilation proper, assembly and linking, always in that order. GCC is capable of preprocessing and compiling several files either into several assembler input files, or into one assembler input file; then each assembler input file produces an object file, and linking combines all the object files (those newly compiled, and those specified as input) into an executable file.

gcc -Q -O3 --help=optimizers

## Compilation

### 1. Preprocessor
cpp

#### option
**-E**
Stop after the preprocessing stage; do not run the compiler proper. The output is in the form of preprocessed source code, which is sent to the standard output.

Input files that don’t require preprocessing are ignored.

**-P**

#### C89/C99
##### offsetof

#### C11
##### _Generic
```
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

##### \_\_typeof__(x)


### 2. Compiler
cc1

show include path:
```
$ gcc -Wp,-v
```

Instead of outputting the result of preprocessing, output a rule suitable for make describing the dependencies of the main source file:
```
$ gcc -M
```

#### GNU extension
\_\_attribute\_\_((naked))
\_\_attribute\_\_((interrupt))

### 3. Assembler
as

### 4. Linker
collect2
dynamic linker (ld)
> The **dynamic linker** is responsible for loading dynamically linked programs and their dependencies (in the form of shared objects). The dynamic linker in the GNU C Library also supports loading shared objects (such as plugins) later at run time. Dynamic linkers are sometimes called **dynamic loaders**.


dump link script:
```
$ gcc -Wl,-verbose main.c
```


**-T *script***:
> Use script as the linker script. This option is supported by most systems using the GNU linker. On some targets, such as bare-board targets without an operating system, the -T option may be required when linking to avoid references to undefined symbols.

**-specs=*file***:
> Process file after the compiler reads in the standard specs file, in order to override the defaults which the gcc driver program uses when determining what switches to pass to cc1, cc1plus, as, ld, etc. More than one -specs=file can be specified on the command line, and they are processed in order, from left to right. See Spec Files, for information about the format of the file.


## C Extenstions
void * __builtin_return_address (*unsigned int level*)
> This function returns the return address of the current function, or of one of its callers. The level argument is number of frames to scan up the call stack. A value of 0 yields the return address of the current function, a value of 1 yields the return address of the caller of the current function, and so forth. When inlining the expected behavior is that the function returns the address of the function that is returned to. To work around this behavior use the noinline function attribute.

[6 Extensions to the C Language Family](https://gcc.gnu.org/onlinedocs/gcc/C-Extensions.html)

## Variable Attriables
**\_\_attribute\_\_\((packed))**

unsigned int **\_\_attribute\_\_((aligned(0x1000)))** page_table_root [256][1024] = {0};
>This attribute specifies a minimum alignment (in bytes) for variables of the specified type.


**\_\_attribute\_\_((section(".isr_vector")))** uint32_t *isr_vectors[] = {...};

## Reference
[How A Compiler Works: GNU Toolchain](https://www.slideshare.net/jserv/how-a-compiler-works-gnu-toolchain)C

 gcc -Wp,-v : show include path