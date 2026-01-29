# ACRONYM

***ELF***   :   *Executable and Linkable Format*

***LFB***   :   *Label Function Begin*

***LFE***   :   *Label Function End*

***CFI***   :   *Call Frame Information*

***CFA***   :   *Call Frame Address*

***DWARF*** :   *not acronym, named as the opposite of STABS (a joke)*

# Explaining


## ELF

***
***A standardized file format for executables, object files, shared libraries, and core dumps
on Unix-like systems***

*It orginizes a program's code, data, and metadata into sections allowing linkers, loaders
, and debuggers to locate and interpret program contents consistently across platforms*
***

### Key points
    - Contains *program instructions*(.text) and *static data* (.rodata, .data, .bss)
    - Stores *symbol tables, relocation info, and section headers* for linking
    - Include *optional debug info*(.debug_*, DWARF, CFI) for tools
    - Designed so *loader ca map sections into memory* with proper permissions
    - Supports both *static and dynamic linking* (shared libraries)

### content
    - Executable code (.text)
    - Read-Only data (.rodata)
    - Writeable data (.data, .bss)
    - Debug info (.debug_*, .eh_frame)
    - Symbol tables, relocations, headers

### Why it matters

    - Linkers produce ELF
    - Loaders (like ld.so or kernel) read ELF to lead program into memory
    - Debuggers read ELF to find DWARF, symbols, and sections


### File layout

#### On Disk

    - [ELF Heaer]
    - [Program Header Table]    ;   loader info
    - .text                     ;   machine code
    - .rodata                   ;   constants
    - .data                     ;   initialized globals
    - .bss (not stored)         ;   zero-initialized globals (only memory)
    - .debug_*                  ;   DWARF / CFI / debug info
    - .section headers          ;   describes all sections

#### On Memory

***note***

.   : mean part of ELF file

()  : mean part of memory layout


    - .text                     ; executable code, *read+execute*
    - .rodata                   ; constants, *read-only*
    - .data                     ; initialized globals, *read+write*
    - .bss                      ; zero-initialized globals, *read+write*
    - (mmap regin)              ; memory-mapped files, shared libraries
    - (heap)                    ; dynamic allocation
    - (stack)                   ; stack frames



## DWARF

**A standardized debugging information format** *used by:*

    - GCC / Clang
    - GDB
    - LLVM tools
    - Linux, BSD, macOS(variants)
    
*sotres:*

    - function boundaries (LFB, LFE ranges)
    - variable locations
    - line numbers <-> addresses
    - call frame info (CFI / CFA)
    - types, structs, scopes


## LFB/LFE
    - Compiler-generated local labels
    - Do not affect execution
    - Used only by tools (Debuggers, unwinders, linkers, profilers)
    - Define the memory range of function
    - They mean *exactly* what thire names say:
        LFB -> Label Function Begin:
            holds the *address where the function begins*
        LFE -> Label Function End:
            holds the *address where the function ends*
    - They are just *precise boundary markers* so tool can say:
        **from this address to that address this is one function**
    
## CFI

***A set of rules that describe how to recover a caller's stack frame at any instruction address***
Not code
Not labels
*Rule*


### Content
CFI holds *state-transition rules* saying:
    - where the *Call Frame Address (CFA) is*
    - where *saved registers* are located *relative to CFA*
    - how those rules *changes as instruction execute*

***CFI is a precise, instruction-byinstruction map that tells tools how to unwind the stack correctly***




## CFA
***A single, abstract address per instruction***

CFA defined as:
    CFA = register + offset

What CFA represents
    - A *stable anchor* used to describe where things are
    














