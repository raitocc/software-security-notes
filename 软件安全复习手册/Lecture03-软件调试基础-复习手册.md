# Lecture03 软件调试基础 - 复习手册

> GitHub: https://github.com/raitocc/software-security-notes

### 本讲主线

Lecture03 讲五个知识点：

1. PE 文件格式。
2. 虚拟内存。
3. 调试分析工具。
4. PE 文件代码注入示例。
5. 软件破解示例。

它承接 Lecture02 的寄存器、栈和汇编基础。上一讲关心“函数调用时栈和寄存器怎么变化”，这一讲关心“可执行文件怎么变成进程，调试器如何定位和修改它”。

一条主线可以这样记：

```text
PE 文件在磁盘上有节、头部、导入表
-> Loader 读取 PE 结构并映射到虚拟内存
-> 调试器看到的是虚拟地址和运行时状态
-> 分析者通过 PE 工具、OllyDbg、IDA 建立文件和内存之间的对应关系
-> 代码注入和破解都是在修改程序控制流
```

### 知识点一：PE 文件格式

PE，Portable Executable，是 Windows 平台常见可执行文件格式。`.exe` 和 `.dll` 都是典型 PE 文件。ELF，Executable and Linkable Format，是 Unix/Linux 系统常见格式。

可执行文件不是只有机器码。一个 PE 文件还会包含：

- 程序代码。
- 已初始化数据。
- 资源，例如图标、菜单、位图、字体。
- 导入表，记录依赖哪些 DLL 和函数。
- 导出表，记录自己提供哪些函数。
- 重定位信息。
- TLS 等初始化相关信息。

PE 格式的意义是给操作系统 Loader 一个标准说明书：文件头在哪里，入口点在哪里，各个 section 在文件中和内存中分别在哪里，依赖哪些外部库，哪些地址需要重定位。

#### PE 与 ELF 的共同点

PE 和 ELF 是不同系统上的可执行文件格式，但思想相似：

- 前面的 Header 提供元数据。
- 后面的 section/segment 才是真正的代码、数据和资源。
- Loader 根据格式信息把文件映射到进程虚拟地址空间。
- 不同 section 映射到内存后可以有不同权限，例如代码节可读可执行，数据节可读可写。

PE 是本课程重点，ELF 只需要知道它在 Unix/Linux 中承担类似角色。

#### PE 典型结构

PE 常见结构：

- DOS Header 和 DOS Stub：历史兼容。DOS Header 会告诉 Loader 真正 PE Header 在哪里；DOS Stub 对现代 Windows Loader 基本无实际执行意义。
- PE Signature：`PE\0\0`，标识真正 PE 头开始。
- COFF File Header：记录 CPU 架构、section 数量、时间戳、Optional Header 大小、文件属性等。
- Optional Header：名字叫 Optional，但对可执行文件非常重要，记录如何加载，包括 `AddressOfEntryPoint`、`ImageBase`、`SectionAlignment`、`FileAlignment`、`SizeOfImage`、Data Directory 等。
- Section Table：记录各个 section 的名称、文件偏移、虚拟地址、大小、属性。
- Section 数据：真正的代码、数据、资源等内容。

Optional Header 中几个字段很重要：

| 字段 | 含义 |
|---|---|
| `ImageBase` | 模块希望被装入内存的基址 |
| `AddressOfEntryPoint` | 程序入口点的 RVA |
| `SectionAlignment` | section 在内存中的对齐粒度 |
| `FileAlignment` | section 在磁盘文件中的对齐粒度 |
| `DataDirectory` | 各种重要表的位置，如导入表、导出表、资源表、重定位表、TLS 表 |

如果对齐不正确，可能导致加载效率低、内存保护无法正确设置，甚至 Loader 无法直接映射。

#### 常见 PE 节

PE 文件被分成若干 section，不同内容放在不同节中：

| 节名 | 常见内容 | 复习记忆 |
|---|---|---|
| `.text` | 编译器产生的机器代码 | 反汇编和调试主要看这里 |
| `.data` | 已初始化数据，如全局变量、静态变量 | 可读写数据 |
| `.rdata` | 只读数据 | 字符串常量、只读表 |
| `.bss` | 未初始化数据 | 文件中不一定真正占空间，加载后内存中要分配 |
| `.rsrc` | 资源，如图标、菜单、位图 | Resource |
| `.idata` | 导入表，记录 DLL 和外来函数信息 | Import data，易考 |
| `.edata` | 导出表 | Export data |
| `.reloc` | 重定位信息 | Relocation |
| `.tls` | 线程局部存储和 TLS 回调相关信息 | Thread Local Storage |

易考：存放动态链接库等外来函数与文件信息的是 `.idata` 或导入表。

#### Loader 加载 PE 的核心步骤

PPT 中给出的加载核心步骤是：

1. 解析可执行文件结构。
2. 构建进程内存映像。
3. 地址重定位。
4. 解析外部依赖。
5. 运行初始化代码。
6. 启动程序入口。

展开理解：

1. Loader 根据 DOS Header 找到 PE Header，读取 `PE\0\0`，确认文件合法。
2. 读取 COFF File Header，知道 CPU 架构、section 数量等。
3. 读取 Optional Header，得到 `ImageBase`、入口点、对齐方式、Data Directory。
4. 为整个 Image 分配虚拟地址空间。
5. 读取 Section Table，把 `.text`、`.data`、`.rsrc` 等映射到内存。
6. 按 `ImageBase + VirtualAddress` 计算 section 在内存中的位置。
7. 查找 Import Table，加载所需 DLL，解析函数地址，填入 IAT。
8. 如果首选 `ImageBase` 被占用或 ASLR 生效，进行 relocation 修正。
9. 处理 TLS 等初始化逻辑。
10. 最终跳转到入口点执行。

这里的重点是：PE 文件不是直接“从第一字节开始执行”。操作系统先按 PE 结构加载和修正，最后才跳到入口点。

#### 导入表和 IAT

动态库相关内容是第三章重点之一。程序调用 `printf`、`MessageBoxA` 等函数时，这些函数代码通常不在主程序自己的 `.text` 节里，而在外部 DLL 中。

编译/链接阶段：

1. 程序记录自己依赖哪些动态库，例如 `kernel32.dll`、`msvcrt.dll`、`user32.dll`。
2. 程序为导入函数建立跳转表，即 IAT，Import Address Table，输入函数地址表。
3. 此时函数真实地址还未知。

运行时阶段：

1. Loader 加载所有需要的 DLL。
2. 为每个 DLL 分配加载地址。
3. 解析导入函数符号，例如 `printf = msvcrt.dll + 某个偏移`。
4. 把真实函数地址写入 IAT，例如 `IAT[printf] = 真实地址`。
5. 程序执行 `call [IAT printf]` 时，就能跳到正确 DLL 函数。

IAT 的安全意义：

- 程序兼容不同系统/DLL 版本，因为真实地址运行时才填。
- 分析者可以通过 IAT 看程序调用了哪些外部 API。
- 攻击者可能通过 IAT Hook 修改函数跳转目标。
- PE 注入实验中调用 `MessageBoxA` 能成功，是因为目标 PE 的导入表/IAT 已经包含相关 API。

### 加壳与脱壳

PPT 在 PE 文件格式后讲了加壳。加壳不是修改程序功能，而是改变可执行代码在磁盘文件中的形态。

加壳，简单说，是对 EXE/DLL 的代码和资源做压缩或加密，再附加一段壳代码 loader stub。加壳后的程序仍然能运行，但磁盘上的原始代码常常变成压缩或加密形式，静态反汇编不容易直接看到真实逻辑。

加壳常见目的：

- 保护软件版权。
- 增加逆向分析难度。
- 降低程序体积。
- 攻击者隐藏代码，躲避安全检测。

加壳的大致步骤：

1. 解析原始 PE 文件。
2. 压缩或加密 `.text`、`.rdata`、`.data` 等 section。
3. 插入加载器桩代码 loader stub。
4. 改变程序入口点，让壳代码先执行。

运行时过程：

```text
Windows Loader 加载加壳程序
-> 先执行壳代码 stub
-> stub 在内存中解密/解压原始程序
-> 修复必要结构
-> 跳回原始入口点 OEP
-> 原程序正常执行
```

压缩壳侧重减小体积；加密壳侧重保护和反调试，不同壳还可能加入注册限制、次数限制、时间限制等功能。反病毒和 Crack 分析中经常遇到节名古怪、入口点异常、导入表异常的加壳程序。

### 知识点二：虚拟内存

Windows 使用用户模式和内核模式隔离普通程序与操作系统核心。

| 模式 | 运行内容 | 权限 |
|---|---|---|
| 用户模式 User Mode | 普通用户程序 | 权限低，不能直接访问硬件和关键内核内存 |
| 内核模式 Kernel Mode | 操作系统内核、系统服务、设备驱动 | 权限高，可访问所有内存和硬件，执行特权指令 |

系统调用是用户态和内核态之间的桥。普通程序需要文件、网络、内存、进程等系统资源时，不能直接操作硬件，而是通过系统调用请求内核完成。

#### 虚拟地址与物理地址

Windows 内存可以分为两个层面：

- 物理内存：真实 RAM，对应物理地址。通常需要内核级别才能直接观察。
- 虚拟内存：进程看到的逻辑地址空间。用户态调试器看到的地址通常是虚拟地址。

用户程序使用的地址称为虚拟地址或逻辑地址。计算机物理内存访问地址称为物理地址或实地址。虚拟地址到物理地址的转换由 MMU，Memory Management Unit，结合页表完成。

为什么要区分虚拟内存和物理内存：

- 地址空间隔离：每个进程有独立虚拟地址空间，程序 A 不会直接改到程序 B 的数据。
- 内存保护：不同页可设置不同权限，例如只读、可写、可执行。
- 装载方便：程序不必关心自己最终被放到哪个物理地址。
- 按需加载：只把正在用的页加载到 RAM。
- 支持交换空间和共享内存。

32 位 Windows 经典语境下，一个进程可拥有 4GB 虚拟地址空间。现在 x64 已突破 4GB 限制，但“每个进程拥有独立虚拟地址空间”的机制仍然成立。

易考：用户模式下，用调试器看到的内存地址通常是虚拟内存地址，不是物理地址。

#### ASLR 与 relocation

ASLR，Address Space Layout Randomization，地址空间布局随机化。它随机化关键内存区域，例如 stack、heap、libc/DLL、code 的加载位置。

为什么需要 ASLR：

- 如果地址固定，攻击者更容易预测 shellcode、库函数或 gadget 地址。
- 覆盖返回地址时，攻击者可以写死跳转目标。

ASLR 生效后，Loader 会随机选择地址映射程序或 DLL，并通过 relocation 修正需要绝对地址的位置。CPU 不“理解 ASLR”，它只执行当前进程里的虚拟地址。随机化和修正工作由 Loader 和操作系统完成。

ASLR 不是绝对安全，常见绕过包括：

- 信息泄漏。
- 暴力尝试。
- 部分地址覆盖。

### PE 文件与虚拟内存的映射

调试漏洞时，经常需要在“文件位置”和“内存位置”之间转换。

两种常见需求：

1. 静态反汇编工具看到某条指令在 PE 文件中的位置，即文件偏移。我们想知道它运行时在内存中的虚拟地址。
2. 动态调试器看到某条指令的虚拟地址。我们想回到 PE 文件中找到对应机器码。

#### VA、RVA、文件偏移、ImageBase

| 概念 | 全称 | 含义 |
|---|---|---|
| VA | Virtual Address | 虚拟地址，PE 装入内存后的地址 |
| RVA | Relative Virtual Address | 相对虚拟地址，相对于 ImageBase 的偏移 |
| File Offset | 文件偏移 | 数据在磁盘 PE 文件中相对于文件开头的位置 |
| ImageBase | 装载基址 | PE 希望被装入内存的基地址 |

核心公式：

```text
VA = ImageBase + RVA
RVA = VA - ImageBase
```

默认情况下，EXE 的常见 ImageBase 是 `0x00400000`，DLL 常见 ImageBase 是 `0x10000000`。这些位置可以通过编译选项更改，实际运行时也可能因 ASLR 或地址占用而变化。

#### 文件对齐与内存对齐

文件偏移和 RVA 有一定一致性，但不能直接等同。原因是磁盘文件和内存中的 section 对齐粒度不同。

PPT 中给出的典型值：

- PE 文件中的数据按磁盘标准存放，常以 `0x200` 字节为基本单位组织。section 不足 `0x200` 会用 `0x00` 填充。
- 代码装入内存后，按内存标准存放，常以 `0x1000` 字节为基本单位组织。

为什么内存中的 section 更大：

- `.bss` 等未初始化数据在文件中不需要实际存储，但内存中需要分配空间。
- Loader 把 section 加载到内存时需要按页对齐。
- 文件中 section 可以较紧凑，内存中必须满足页和权限管理要求。

所以，不能简单把文件偏移当运行时地址。真实换算要结合 Section Table。

#### LordPE 例子

PPT 中用 LordPE 查看节信息：

- `VOffset` 是 RVA。
- `ROffset` 是文件偏移。

例子：`.text` 节的 `VOffset = 0x11000`，ImageBase 为 `0x400000`，则该节在内存中的虚拟地址为：

```text
0x400000 + 0x11000 = 0x411000
```

如果同一节在文件中的 `ROffset = 0x1000`，则可以用二进制编辑器在文件偏移 `0x1000` 附近找到对应机器码。这个例子就是为了说明：同一段代码，在文件中有文件偏移，在内存中有 VA/RVA，两者需要通过节表关联。

LordPE 还能查看：

- Section 信息。
- 导入表。
- 导出表。
- 地址换算。

导入表例子：PPT 中导入表在文件里的 `ROffset = 0x24000`，RVA 是 `0x25000`。打开目录表可以看到输入表 RVA 确实是 `0x25000`，再通过左侧按钮查看具体输入表内容。

常见 PE/二进制分析工具：

- LordPE：Windows 下 PE 信息查看、修改、脱壳辅助。
- PEView：直观显示 PE 文件结构。
- `readelf`：Linux 下查看 ELF。
- `objdump`：查看 section、反汇编。
- Cutter：跨平台 GUI 逆向工具。
- ImHex：逆向工程可视化和十六进制分析。
- IDA Pro：反汇编、反编译、分析多种可执行文件。

### 知识点三：调试分析工具

调试工具的目标是把程序运行时状态看见。静态工具看文件结构和代码逻辑，动态调试器看寄存器、内存、栈、指令执行和分支跳转。

#### OllyDbg

OllyDbg 是 32 位可视化汇编分析调试器，适合动态调试。它可以直接打开一个 exe 调试，也可以附加到已经运行的进程。

两种载入方式：

- 文件 -> 打开，快捷键 `F3`，打开可执行文件进行调试。
- 文件 -> 附加，附加到已经运行的进程。

常用快捷键：

| 快捷键 | 作用 |
|---|---|
| `F2` | 设置断点 |
| `F8` | 单步步过，遇到 `CALL` 不进入子函数 |
| `F7` | 单步步入，遇到 `CALL` 会进入子函数 |
| `F4` | 运行到选定位置 |
| `F9` | 运行 |
| `Alt+F9` | 执行到用户代码，用于从系统代码区域返回到被调程序区域 |
| `Ctrl+F9` | 执行到返回，在遇到 `ret` 时暂停 |

Trace 跟踪功能用于记录调试过程中执行过的指令，有助于分析断点之前发生了什么。Trace 可选择是否记录寄存器值。

#### IDA Pro

IDA，Interactive Disassembler，交互式反汇编工具，是逆向分析主流工具。它偏静态分析，也有调试能力。打开文件后，IDA 会分析函数、字符串、导入导出、控制流等。

IDA 常见视图和窗口：

- Proximity View：展示函数或代码块之间的关系。
- IDA View：主要反汇编窗口。
- Graph View：图形视图，把一个函数分成基本块，显示控制流。
- Text View：文本视图，显示完整反汇编清单，也能看数据部分。
- Names Window：全局名称窗口。
- Strings Window：字符串窗口。
- Functions Window：函数列表。
- Function Call Window：函数调用关系窗口。
- Pseudocode Window：反编译伪代码窗口。

Graph View 中：

- 函数被分成多个基本块。
- Yes 分支箭头通常为绿色。
- No 分支箭头通常为红色。
- 蓝色箭头表示指向下一个即将执行的块。

Text View 中：

- 虚拟地址常以 `[区域名称]:[虚拟地址]` 显示，例如 `.text:0040110C0`。
- 实线箭头通常表示非条件跳转。
- 虚线箭头通常表示条件跳转。
- 粗线跳转往往提示控制流回到某个地址，常见于循环。

Names Window 列举二进制文件的全局名称。名称是对虚拟地址的符号描述。课后题提到：Names 窗口中名称前的 `I` 通常表示 imported name，即导入名称。

Strings Window 显示二进制中提取的字符串以及地址。双击字符串会跳转到该字符串位置；结合交叉引用，可以追踪代码中哪些位置引用了该字符串。这在破解和漏洞分析里很常用，例如通过 “wrong password” 定位错误分支。

Functions Window 显示所有函数。条目可显示函数地址、所在 section、函数长度。例如在虚拟地址 `00401040` 的 `.text` 部分找到 `_main` 函数，该函数长度为 `0x50` 字节。

IDA 新版本支持反编译。选中汇编代码后按 `F5`，IDA 会在 Pseudocode 窗口中显示 C/C++ 风格伪代码。这有助于快速理解逻辑，但伪代码不是源码，仍需结合汇编判断。

### 知识点四：PE 文件代码注入示例

PPT 演示的是授权实验环境下对 Windows XP 扫雷程序 `winmine.exe` 做 PE 注入。目的不是让你背步骤，而是理解代码注入的三个关键动作：

1. 把新代码写入 PE 文件某个可用位置。
2. 让程序执行流先跳到新代码。
3. 新代码执行后跳回原始入口点，保证原程序继续运行。

实验目标：让扫雷程序运行前先弹出一个 `MessageBoxA` 对话框，显示注入提示，然后再正常启动扫雷。

#### 找入口点

用 OllyDbg 打开扫雷程序，程序停在入口点。PPT 中：

- LordPE 查看入口点 RVA 是 `0x00003E21`。
- 装载基址是 `0x01000000`。
- 因此入口点 VA 是 `0x01000000 + 0x00003E21 = 0x01003E21`。
- OllyDbg 中 EIP 也显示 `0x01003E21`，注释提示 `ModuleEntryPoint`。

这就是 VA/RVA 公式在实际调试中的使用。

#### 写入注入代码

实验在代码空白区域写入数据和汇编。计划注入代码功能：

- 构造标题字符串，例如 `PE Inject`。
- 构造内容字符串，例如 `You are Injected!`。
- 调用 `MessageBoxA`。
- 跳回原始入口点 `0x01003E21`。

`MessageBoxA` 函数原型：

```c
int MessageBoxA(
    HWND hWnd,
    LPCSTR lpText,
    LPCSTR lpCaption,
    UINT uType
);
```

参数含义：

- `hWnd`：所属窗口句柄，`NULL` 表示不属于任何窗口。
- `lpText`：消息框内容字符串指针。
- `lpCaption`：消息框标题字符串指针。
- `uType`：消息框风格，`NULL` 表示默认风格。

Windows API 中 `MessageBox` 会根据字符串类型选择 ASCII 版本 `MessageBoxA` 或 Unicode 版本 `MessageBoxW`。实验使用 `MessageBoxA`。

32 位常见调用约定下，参数从右到左压栈。因此调用代码形态是：

```asm
push 0              ; uType，默认风格
push 标题字符串地址 ; lpCaption
push 内容字符串地址 ; lpText
push 0              ; hWnd
call MessageBoxA
jmp  原始入口点
```

PPT 中示例地址：

```asm
push 0
push 0x01004AA7    ; 标题字符串地址
push 0x01004A9C    ; 内容字符串地址
push 0
call MessageBoxA
jmp 0x01003E21     ; 回到原始入口点
```

字符串后面要保留 `0x00` 结束符。`call MessageBoxA` 能成功，是因为 PE 输入表里已经有这个函数的入口地址或可通过导入机制定位。

#### 挂接入口点

只把代码写到空白区域还不够。代码必须被执行，才算真正注入。实验中通过 LordPE 修改程序入口点，让程序启动时先执行注入代码。

PPT 中注入代码起始 VA 是 `0x01004ABA`，ImageBase 是 `0x01000000`，所以新入口点 RVA 是：

```text
0x01004ABA - 0x01000000 = 0x00004ABA
```

因此只需要把 PE 入口点 RVA 改成 `0x00004ABA`。运行后流程变成：

```text
程序启动
-> 跳到注入代码
-> 弹出 MessageBoxA
-> jmp 回原始入口点 0x01003E21
-> 扫雷程序继续正常运行
```

这说明代码注入的本质不是“凭空多了代码”，而是修改了程序控制流。

### 知识点五：软件破解示例

PPT 的破解示例是一个简单密码验证程序。源代码逻辑大意：

```c
#define password "12345678"

bool verifyPwd(char *pwd) {
    int flag;
    flag = strcmp(password, pwd);
    return flag == 0;
}

void main() {
    char pwd[1024];
    while (1) {
        scanf("%s", pwd);
        if (verifyPwd(pwd)) {
            printf("passed\n");
            break;
        } else {
            printf("wrong password, please input again:\n");
        }
    }
}
```

程序的安全逻辑是：`strcmp` 返回 0 表示密码相等，`verifyPwd` 返回 true，进入 `passed` 分支。

PPT 讲了两种破解思路，本质都是修改控制流或返回值。

#### 破解方式一：改条件跳转

OllyDbg 中可以通过字符串快速定位关键分支：

1. 运行程序，输入错误密码，看到输出 `wrong password`。
2. 在 OllyDbg 反汇编窗口中，右键选择“查找 -> 所有引用的字符串”。
3. 用 `Ctrl+F` 搜索 `wrong`。
4. 双击定位到引用该字符串的代码位置。
5. 观察附近条件跳转，找到决定进入成功分支还是失败分支的指令。

PPT 中核心分支是 `jz`。如果 `jz` 条件成立，会跳到显示错误密码的分支。把 `jz` 改为 `jnz` 后，逻辑反过来：错误密码也会进入成功分支。

这个例子的本质：

```text
原程序：条件成立 -> 失败分支
修改后：条件不成立 -> 失败分支
结果：原本失败的输入可能进入成功分支
```

修改当前汇编代码后，只是在调试器内存中修改。如果要真正修改 exe 文件，需要把修改复制到可执行文件并保存。

#### 破解方式二：改函数返回值

第二种方式是进入 `verifyPwd` 函数，直接修改返回值。

关键点：

- `strcmp` 的返回值在 `EAX` 中。
- 程序把 `strcmp` 结果保存到局部变量 `flag`。
- 比较 `flag` 是否等于 0。
- `sete al` 根据比较结果设置 `AL`。相等则 `AL=1`，否则 `AL=0`。
- 函数返回值常通过 `EAX/AL` 表达。

典型汇编含义：

```asm
; strcmp 返回值在 eax
; flag = eax
xor eax, eax              ; eax 清零
cmp dword ptr [ebp-8], 0  ; 比较 flag 和 0
sete al                   ; 如果相等，则 al = 1
```

如果想强制函数返回 true，可以把相关代码改成：

```asm
mov al, 01
nop
```

这样不管 `strcmp` 结果如何，函数都返回真。

易考指令：

- `XOR EAX, EAX` 执行后，`EAX = 0`。
- `CMP EAX, EAX` 会设置相等标志。
- `CMP EAX, EAX; SETE AL` 后，因为比较相等，`AL = 1`。

### 易考点

- PE 是 Windows 平台 Portable Executable，ELF 是 Unix/Linux 常见 Executable and Linkable Format。
- `.idata` / 导入表保存外部 DLL 和函数信息。
- Loader 加载 PE 要解析头部、映射 section、处理导入表、重定位、TLS，最后跳到入口点。
- IAT 是 Import Address Table，运行时填入导入函数真实地址。
- 加壳会插入 loader stub，并改变入口点，让壳代码先执行。
- 用户态调试器看到的通常是虚拟地址。
- `VA = ImageBase + RVA`。
- 文件偏移和 RVA 不同，因为文件对齐和内存对齐不同。
- LordPE 中 `VOffset` 是 RVA，`ROffset` 是文件偏移。
- OllyDbg：`F2` 断点，`F7` 步入，`F8` 步过，`F9` 运行，`Ctrl+F9` 执行到返回。
- IDA 文本视图中实线箭头通常表示非条件跳转，虚线箭头表示条件跳转。
- IDA Names 窗口中 `I` 通常表示导入名称。
- IDA 按 `F5` 可生成伪代码。
- PE 注入实验中，修改入口点只是改 RVA，不是直接填 VA。
- `MessageBoxA(hWnd, lpText, lpCaption, uType)` 压栈顺序是 `uType`、`lpCaption`、`lpText`、`hWnd`。
- 软件破解常通过修改条件跳转或强制修改返回值实现。
- `XOR EAX, EAX` 后 `EAX=0`。
- `CMP EAX, EAX; SETE AL` 后 `AL=1`。

### 常见分析问题

1. 这个地址是文件偏移、RVA 还是 VA？
2. 当前代码属于主程序还是某个 DLL？
3. 这个函数是程序自己实现的，还是通过 IAT 调用的外部函数？
4. 当前代码所在 section 的权限是什么？
5. 程序入口点是否正常，是否可能被壳修改？
6. 条件跳转依赖哪个标志位，标志位由哪条指令设置？
7. 函数返回值放在哪个寄存器？
8. 如果修改了代码，是否已经保存回可执行文件？

### 复习自查

1. PE 文件为什么不能理解成一串纯机器码？
2. Loader 加载 PE 文件的主要步骤是什么？
3. IAT 解决了什么问题？为什么 API 函数地址运行前不确定？
4. 加壳程序为什么能先执行壳代码，再执行原程序？
5. 用户态和内核态有什么区别？
6. 为什么调试器看到的是虚拟地址？
7. VA、RVA、ImageBase、文件偏移分别是什么？
8. 为什么文件偏移不能直接当作内存地址？
9. OllyDbg 的 `F7` 和 `F8` 有什么区别？
10. IDA 的 Strings 窗口为什么能帮助定位关键逻辑？
11. PE 注入示例中，为什么注入代码最后要跳回原始入口点？
12. 软件破解示例中，修改条件跳转和修改返回值分别改变了什么？

---
