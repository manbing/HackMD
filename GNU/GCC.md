# GCC (GNU Compiler Collection)
###### tags: `GNU`


gcc -Q -O3 --help=optimizers

## Preprocessor
cpp

### option
**-E**
Stop after the preprocessing stage; do not run the compiler proper. The output is in the form of preprocessed source code, which is sent to the standard output.

Input files that don’t require preprocessing are ignored.

**-P**

### C89/C99
#### offsetof

### C11
#### _Generic
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

#### \_\_typeof__(x)


## Compiler
cc1

**gcc -Wp,-v**:
show include path

**gcc -M**:
Instead of outputting the result of preprocessing, output a rule suitable for make describing the dependencies of the main source file.

### GNU extension
\_\_attribute\_\_((naked))
\_\_attribute\_\_((interrupt))

## Assembler
as

## Linker
collect2
ld

**-T *script***:
Use script as the linker script. This option is supported by most systems using the GNU linker. On some targets, such as bare-board targets without an operating system, the -T option may be required when linking to avoid references to undefined symbols.

**-specs=*file***:
Process file after the compiler reads in the standard specs file, in order to override the defaults which the gcc driver program uses when determining what switches to pass to cc1, cc1plus, as, ld, etc. More than one -specs=file can be specified on the command line, and they are processed in order, from left to right. See Spec Files, for information about the format of the file.


## Reference
[How A Compiler Works: GNU Toolchain](https://www.slideshare.net/jserv/how-a-compiler-works-gnu-toolchain)