Hello,

I built from sources (provided by HighTec to avoid GPL violations here https://www.hightec-rt.com/en/downloads/sources.html) the binutils 2.13 package targetting the tricore architecture with host Windows under x86 (686) platforms. Sources files are provided on this repository if needed (in case where the URL link will be not available).

As the tricore arch is not an official branch of gcc/binutils, there is no package found with free of charge…. So I prefer to build it as Hightec made sources available.

It could help to read the content of elf files, PFLASH dumps in S-Record formats or bin file (to see opcode, jump table…) on projects using Infineon Tricore CPUs taken in charge by changes made by Hightec, at this date.

The Mingw32 gcc toolchain revision 4.8 have been used to build sources. Windows executables files can also be running under linux using wine by example. Binaries files have been linked statically using the gcc runtime file libgcc.a. No need to provide libgcc_s_dw2-1.dll file (GPL).

The details with examples using somes commands lines, using an elf file built targetting a TC1797 CPU:

# This directory was configured as follows:
./configure --build=i686-pc-mingw32 --host=i686-pc-mingw32 --target=tricore --norecursion

$ file tricore-objdump.exe
tricore-objdump.exe: PE32 executable (console) Intel 80386, for MS Windows

$ ./tricore-objdump.exe -i
BFD header file version 2.13
elf32-tricore
 (header little endian, data little endian)
  TriCore:Rider-B
ieee
 (header endianness unknown, data endianness unknown)
  TriCore:Rider-B
elf32-little
 (header little endian, data little endian)
  TriCore:Rider-B
elf32-big
 (header big endian, data big endian)
  TriCore:Rider-B
srec
 (header endianness unknown, data endianness unknown)
  TriCore:Rider-B
symbolsrec
 (header endianness unknown, data endianness unknown)
  TriCore:Rider-B
tekhex
 (header endianness unknown, data endianness unknown)
  TriCore:Rider-B
binary
 (header endianness unknown, data endianness unknown)
  TriCore:Rider-B
ihex
 (header endianness unknown, data endianness unknown)
  TriCore:Rider-B

               elf32-tricore ieee elf32-little elf32-big srec symbolsrec
TriCore:Rider-B elf32-tricore ieee elf32-little elf32-big srec symbolsrec

               tekhex binary ihex
TriCore:Rider-B tekhex binary ihex

$ ./tricore-readelf.exe -e example.elf | head –n20
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Siemens Tricore
  Version:                           0x1
  Entry point address:               0xa0000000
  Start of program headers:          52 (bytes into file)
  Start of section headers:          8124672 (bytes into file)
  Flags:                             0x800000
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         8603
  Size of section headers:           40 (bytes)
  Number of section headers:         11806
  Section header string table index: 1


$ ./tricore-objdump.exe -m tricore -d example.elf | head -n20

example.elf:     file format elf32-tricore

Disassembly of section .text.DEFAULT_CODE_ROM:

8001f0c8 <CanInit>:
8001f0c8:       a0 04           mov.a %a4,0
8001f0ca:       1d 00 9c 01     j 8001f402 <Can_InitController>

8001f0ce <CanInterrupt>:
8001f0ce:       02 49           mov %d9,%d4
8001f0d0:       91 10 00 fd     movh.a %a15,53249

8001f0d4 <L_377>:
8001f0d4:       d9 ff 36 49     lea %a15,[%a15]-28362 <d0009136 <canllSleepNotify>>
8001f0d8:       01 f9 00 f6     addsc.a %a15,%a15,%d9,0
8001f0dc:       14 f0           ld.bu %d0,[%a15]
8001f0de:       df 10 9f 00     jeq %d0,1,8001f21c <L_12>



$ strings -a tricore-objdump.exe | grep -i gcc
libgcc_s_dw2-1.dll
gcc2_compiled
gcc2_compiled.
gcc_compiled.
GCC: (GNU) 4.8.1

$ tree
.
├── COPYING
├── Readme.txt
└── usr
    └── local
        ├── bin
        │   ├── tricore-addr2line.exe
        │   ├── tricore-ar.exe
        │   ├── tricore-c++filt.exe
        │   ├── tricore-nm.exe
        │   ├── tricore-objcopy.exe
        │   ├── tricore-objdump.exe
        │   ├── tricore-ranlib.exe
        │   ├── tricore-readelf.exe
        │   ├── tricore-size.exe
        │   ├── tricore-strings.exe
        │   └── tricore-strip.exe
        ├── src
        │   └── binutils-2.13.tar.bz2
        └── tricore
            └── bin
                ├── ar.exe
                ├── nm.exe
                ├── ranlib.exe
                └── strip.exe

6 directories, 18 files

Packages are built under the GPL licence. Sources are available in the src folder. See COPYING file.

Regards,
Rabia Chebah.

