### [M7M01_Eukaron](https://github.com/EDI-Systems/M7M01_Eukaron/tree/main) X64 User Level Library Manual

---

[点击此处查看中文文档]()

### 1.Introduction

* This project implements the x86-64 user-space components of the RME microkernel.
* It provides system call wrappers and a set of test cases for verifying kernel functionality.
* The code is designed to run with the user-space page table direct mapping option **DISABLED**.

### 2.Build and Run

* Environment: 
  * Ubuntu 22.04

* Tools:
  * GNU GCC 11.40
  * GNU Make 4.3

* To build the user-space code, run:

```shell
make clean && make
```

* Then, we use the `bincopy` script to convert the binary file into a `UVM.c` file, which contains the user-space program as a C array.

```shell
./bincopy.sh
```

* Finally, copy the `UVM.c` file into your kernel project directory. After recompilation, it will be included in the kernel and loaded as a user-space program by your OS.

### 3.Directory Structure

```
|-- Include
|   |-- Init
|   |   |-- init.h
|   |   `-- syssvc.h
|   |-- Platform
|   |   |-- UVM_platform.h
|   |   `-- X64
|   |       |-- Profiles
|   |       |-- uvmlib_x64.h
|   |       |-- uvmlib_x64.ld
|   |       `-- uvmlib_x64_conf.h
|   |-- UVM.h
|   `-- rme.h
|-- Init
|   |-- init.c
|   `-- syssvc.c
|-- Makefile
|-- Platform
|   `-- X64
|       |-- uvmlib_x64.c
|       `-- uvmlib_x64_asm.s

```

* Init.h:*The header of user-level library.*

* Syssvc.h:
  * *The header of user level low-level library*

* Uvmlib_x64_conf.h:
  * *The platform specific types for x86-64 system library*

* Uvmlib_x64.h:

  * *The header of the platform dependent part*

* Uvmlib_x64.ld:

  * *This project's linker script, designing the memory layout.*

* Uvm_platform.h:

  * *The platform specific types for RME.*

* rme.h:

  *  *The header of the RME RTOS. This header defines the error codes,*

      *operation flags and system call numbers in a generic way.*

* UVM.h:

  * *Empty for now*.

* Init.c:

  * *The init process of MMU-based UVM systems.*

  * *Just a benchmark, and will only output the performance figures. The kernel*

  * *It is not responsible for parsing the .elf files, so the header of this file is*

  ​       *pretty much fixed*

* Syssvc.c:

  * *The x86-64 system library platform specific header*

* Uvmlib_x64_asm.s:

  * *The x86-64 user-level assembly scheduling support of the UVM RTOS*

* Uvmlib_x64.c:

  * *The x86-64 system library platform specific header.*

### 4.System Call Interface

|     System Call      |                        Description                        |
| :------------------: | :-------------------------------------------------------: |
|   *UVM_Captbl_Crt*   |               *Create a capability table.*                |
|   *UVM_Captbl_Del*   |           *Delete a layer of capability table.*           |
|   *UVM_Captbl_Frz*   |      *Freeze a capability in the capability table.*       |
|   *UVM_Captbl_Add*   |      *Add one capability into the capability table*       |
|  *UVM_Captbl_Kmem*   |    *Remove one capability from the capability table.*     |
|    *UVM_Kern_Act*    |               *Activate a kernel function.*               |
|   *UVM_Pgtbl_Crt*    |              *Create a layer of page table*.              |
|   *UVM_Pgtbl_Del*    |              *Delete a layer of page table.*              |
|   *UVM_Pgtbl_Add*    |     *Delegate a page from one page table to another*      |
|   *UVM_Pgtbl_Rem*    |           *Remove a page from the page table.*            |
|   *UVM_Pgtbl_Con*    |   *Map a child page table from the parent page table.*    |
|   *UVM_Pgtbl_Des*    |  *Unmap a child page table from the parent page table.*   |
|    *UVM_Proc_Crt*    |                    *Create a process.*                    |
|    *UVM_Proc_Del*    |                    *Delete a process.*                    |
|    *UVM_Proc_Cpt*    |     *Assign or change a process's capability table.*      |
|    *UVM_Proc_Pgt*    |        *Assign or change a process's page table.*         |
|    *UVM_Thd_Crt*     |                    *Create a thread.*                     |
|    *UVM_Thd_Del*     |                    *Delete a thread.*                     |
|  *UVM_Thd_Exec_Set*  |          *Set a thread's entry point and stack.*          |
| *UVM_Thd_Sched_Bind* |              *Set a thread's priority level*              |
| *UVM_Thd_Sched_Rcv*  | *Try to receive a notification from the scheduler queue.* |
| *UVM_Thd_Sched_Prio* |            *Change a thread's priority level.*            |
| *UVM_Thd_Sched_Free* |         *Free a thread from its current bonding.*         |
| *UVM_Thd_Time_Xfer*  |        *Transfer time from one thread to another.*        |
|    *UVM_Thd_Swt*     |                *Switch to another thread.*                |
|    *UVM_Sig_Crt*     |               *Create a signal capability.*               |
|    *UVM_Sig_Del*     |               *Delete a signal capability.*               |
|    *UVM_Sig_Snd*     |          *Try to send a signal from user level.*          |
|    *UVM_Sig_Rcv*     |           *Try to receive a signal capability.*           |
|    *UVM_Inv_Crt*     |             *Create a invocation capability.*             |
|    *UVM_Inv_Del*     |            *Delete an invocation capability.*             |
|    *UVM_Inv_Set*     |     *Set an invocation stub's entry point and stack.*     |

### 5.ChangeLog

#### [v1.0.0] - 2025-06-11

- Initial release

### Author

[![Username](https://github.com/b1narypoet.png?size=40)](https://github.com/username)  [**b1narypoet**](https://github.com/b1narypoet)

