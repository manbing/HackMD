format of Executable and Linking Format (ELF) files


![image](https://hackmd.io/_uploads/HkK8Ve5LJl.png)

![image](https://hackmd.io/_uploads/HywQ-G9U1e.png)



# Section
* .dynsym
This table contains a subset of the symbols from the `.symtab` table that are needed to support dynamic linking. This symbol table is allocable, and is therefore available in the memory image of the process. This section holds the dynamic linking symbol table.

* .symtab
This symbol table contains every symbol that describes the associated ELF file. This symbol table is typically non-allocable, and is therefore not available in the memory image of the process.
GCC and debug tools, e.g., GDB and Valgrind, need this section to work.
Linker involves this section during compilation, linking phase. if it does not need this section anymore, it can be stripped off by command, `strip`.
`strip` cammnd can strip non-allocable sections from ELF. e.g., `.ARM.attributes`, `.debug_aranges`, `.debug_info` and so on. The program can execute normally without these non-allocable sections.

* .interp
This section holds the pathname of program interpreter (Dynamic linker/loader).

* .data
This section holds initialized data that contrbute to the program's memory image.

* .bss
This section holds uninitialized data that contributes to the program's memory image.


* .dynamic
This section holds dynamic linking information.

* .init
This section holds executable instructions that contribute to the process initialization code.  When a program starts to run the system arranges to execute the code in this section before calling the main program entry point.

* .text
This section holds the "text", or executable instructions, of a program.
              
* .rodata
This section holds read-only data that typically contributes to a nonwritable segment in the process image.

![image](https://hackmd.io/_uploads/SJ4j1H9IJg.png)



# Program header (Phdr)
An executable or shared object file's program header table is an array of structures, each describing a `segment` or other information the system needs to prepare the program for execution.  An object file segment contains one or more sections. Program headers are meaningful only for executable and shared object files.

The linker merges together all sections of ==the same type== included in the input object files into a single section and assigns an initial address to it. For instance, the `.text` sections of all object files are merged together into a single `.text` section, which by default contains all of the code in the program. Some of the segments defined in an ELF binary file are used by the GNU loader to assign memory regions with specific access rights to the process.

Executable files include four canonical sections called, by convention, `.text`, `.data`, `.rodata`, and `.bss`. The `.text` section contains executable code and is packed into a segment ==which has the read and execute access rights==. The `.data` and `.bss` sections contain initialized and uninitialized data respectively, ==and are packed into a segment which has the read and write access rights==. Linker will pack all sections which has same permissions into same segment. When executing program, it is convenient for ld (dynamic linker/loader), to allocate memory; for kernel to manage memory.

Linux loads the `.text` section into memory only once, no matter how many times an application is loaded. This reduces memory usage and launch time and is safe because the code doesn't change. For that reason, the `.rodata` section, which contains read-only initialized data, is packed into the same segment that contains the `.text` section. The `.data` section contains information that could be changed during application execution, so this section must be copied for every instance.

![image](https://hackmd.io/_uploads/BkTA992LJe.png)


# Extensive reading
[GNU Binutils](https://hackmd.io/X79jOXNcQOW0DG_1vXdvKg): we can use it to examine ELF file.


# Reference
[Executable and Linkable Format](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
[format of Executable and Linking Format (ELF) files](https://man7.org/linux/man-pages/man5/elf.5.html)
[Special sections in Linux binaries](https://lwn.net/Articles/531148/)