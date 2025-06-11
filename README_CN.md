### [M7M01_Eukaron](https://github.com/EDI-Systems/M7M01_Eukaron/tree/main) X64平台用户态库手册

---

[Click here for EN version](https://github.com/b1narypoet/RME_X64_UVM/blob/main/README.md)

### 1.介绍

* 本项目实现了 RME 微内核的 x86-64 用户态组件。
* 提供系统调用接口封装以及用于验证内核功能的测试用例。
* 该代码设计需在用户态页表直管选项**关闭**的情况下运行。

### 2.构建与运行

* 环境: 
  * Ubuntu 22.04

* 工具:
  * GNU GCC 11.40
  * GNU Make 4.3

* 构建用户态代码时，运行：

```shell
make clean && make
```

* 然后，我们使用 `bincopy` 脚本将二进制文件转换成 `UVM.c` 文件，该文件以 C 数组的形式包含了用户态程序。

```shell
./bincopy.sh
```

* 最后，将 `UVM.c` 文件复制到您的内核项目目录中。重新编译后，它将被包含在内核中，并由您的操作系统作为用户态程序加载。

### 3.目录结构

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

------

- **Init.h：** 用户级库的头文件。
- **Syssvc.h：**
  - 用户级底层库的头文件。
- **Uvmlib_x64_conf.h：**
  - x86-64 系统库的平台相关类型定义。
- **Uvmlib_x64.h：**
  - 平台相关部分的头文件。
- **Uvmlib_x64.ld：**
  - 本项目的连接器脚本，设计内存布局。
- **Uvm_platform.h：**
  - RME 平台相关类型定义。
- **rme.h：**
  - RME 实时操作系统的头文件。该头文件以通用方式定义了错误代码、操作标志和系统调用号。
- **UVM.h：**
  - 目前为空。
- **Init.c：**
  - 基于 MMU 的 UVM 系统的初始化进程。
  - 只是一个基准测试程序，仅输出性能数据。内核
  - 不负责解析 .elf 文件，因此该文件的头部基本固定。
- **Syssvc.c：**
  - x86-64 系统库的平台相关实现。
- **Uvmlib_x64_asm.s：**
  - UVM 实时操作系统中 x86-64 用户态汇编的调度支持。
- **Uvmlib_x64.c：**
  - x86-64 系统库的平台相关实现。

### 4.系统调用接口

|       系统调用       |                 描述                 |
| :------------------: | :----------------------------------: |
|   *UVM_Captbl_Crt*   |          *创建一个能力表。*          |
|   *UVM_Captbl_Del*   |         *删除能力表的一层。*         |
|   *UVM_Captbl_Frz*   |      *冻结能力表中的一个能力。*      |
|   *UVM_Captbl_Add*   |      *向能力表中添加一个能力。*      |
|  *UVM_Captbl_Kmem*   |      *从能力表中移除一个能力。*      |
|    *UVM_Kern_Act*    |         *激活一个内核功能。*         |
|   *UVM_Pgtbl_Crt*    |           *创建一层页表。*           |
|   *UVM_Pgtbl_Del*    |           *删除一层页表。*           |
|   *UVM_Pgtbl_Add*    | *将一页从一个页表委托给另一个页表。* |
|   *UVM_Pgtbl_Rem*    |         *从页表中移除一页。*         |
|   *UVM_Pgtbl_Con*    |      *从父页表映射一个子页表。*      |
|   *UVM_Pgtbl_Des*    |    *从父页表取消映射一个子页表。*    |
|    *UVM_Proc_Crt*    |           *创建一个进程。*           |
|    *UVM_Proc_Del*    |           *删除一个进程。*           |
|    *UVM_Proc_Cpt*    |      *分配或更改进程的能力表。*      |
|    *UVM_Proc_Pgt*    |       *分配或更改进程的页表。*       |
|    *UVM_Thd_Crt*     |           *创建一个线程。*           |
|    *UVM_Thd_Del*     |           *删除一个线程。*           |
|  *UVM_Thd_Exec_Set*  |       *设置线程的入口点和栈。*       |
| *UVM_Thd_Sched_Bind* |         *设置线程的优先级。*         |
| *UVM_Thd_Sched_Rcv*  |      *尝试从调度队列接收通知。*      |
| *UVM_Thd_Sched_Prio* |         *更改线程的优先级。*         |
| *UVM_Thd_Sched_Free* |      *解除线程当前的绑定关系。*      |
| *UVM_Thd_Time_Xfer*  | *将时间从一个线程转移到另一个线程。* |
|    *UVM_Thd_Swt*     |         *切换到另一个线程。*         |
|    *UVM_Sig_Crt*     |         *创建一个信号能力。*         |
|    *UVM_Sig_Del*     |         *删除一个信号能力。*         |
|    *UVM_Sig_Snd*     |       *尝试从用户态发送信号。*       |
|    *UVM_Sig_Rcv*     |       *尝试接收一个信号能力。*       |
|    *UVM_Inv_Crt*     |         *创建一个调用能力。*         |
|    *UVM_Inv_Del*     |         *删除一个调用能力。*         |
|    *UVM_Inv_Set*     |     *设置调用存根的入口点和栈。*     |

### 5.更新日志

#### [v1.0.0] - 2025-06-11

- 初版

### 作者

[![Username](https://github.com/b1narypoet.png?size=40)](https://github.com/username)  [**b1narypoet**](https://github.com/b1narypoet)

