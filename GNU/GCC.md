# GCC (GNU Compiler Collection)
###### tags: `GNU`

## Preprocessor

## Compiler
**gcc -Wp,-v**:
show include path

**gcc -M**:
Instead of outputting the result of preprocessing, output a rule suitable for make describing the dependencies of the main source file.

### attribute
\_\_attribute\_\_((naked))
\_\_attribute\_\_((interrupt))

## Assembler 

## Linker
**-T script**:
Use script as the linker script. This option is supported by most systems using the GNU linker. On some targets, such as bare-board targets without an operating system, the -T option may be required when linking to avoid references to undefined symbols.

**-specs=file**:
Process file after the compiler reads in the standard specs file, in order to override the defaults which the gcc driver program uses when determining what switches to pass to cc1, cc1plus, as, ld, etc. More than one -specs=file can be specified on the command line, and they are processed in order, from left to right. See Spec Files, for information about the format of the file.