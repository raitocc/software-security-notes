# Lecture03 软件调试基础 - 逐页文字版

> 说明：本文件由 PDF 文本层逐页抽取生成，用于核对覆盖；复习请优先看同目录下的“复习讲义”。

## 来源：Lecture03-软件调试基础.pdf

### Lecture03-软件调试基础.pdf / Page 1

第三章 调试分析工具
知识点一：PE文件格式
知识点二：虚拟内存
知识点三：调试分析工具
知识点四：PE文件代码注入示例
知识点五：软件破解示例

### Lecture03-软件调试基础.pdf / Page 2

学习之前，回顾两个问题

常见寄存器用法
EAX\ECX
EIP
ESP EBP

常见汇编指令
MOV PUSH POP
CALL
RET

在了解VC源代码程序如何执行后，考虑可执行文件如何运行

### Lecture03-软件调试基础.pdf / Page 3

知识点一：PE文件格式

### Lecture03-软件调试基础.pdf / Page 4

PE文件格式
可执行文件之所以可以被操作系统加载且运行，是因为它们遵循相同的规
范。PE（Portable Executable）是Win32平台下可执行文件遵守的数据格式。
常见的可执行文件（如“*.exe”文件和“*.dll”文件）都是典型的PE文件。
一个可执行文件不光包含了二进制机器码，还会自带许多其他信息，如字
符串、菜单、图标、位图、字体等。PE文件格式规定了所有的这些信息在
可执行文件中如何组织。在程序被执行时，操作系统会按照PE文件格式的
约定去相应的地方准确地定位各种类型的资源，并分别装入内存的不同区
域。

### Lecture03-软件调试基础.pdf / Page 5

PE vs. ELF 格式

PE
(Portable Executable)
for Windows

ELF
(Executable and
Linkable Format)
for Unix

前面的结构提供元数据
后面的section才是程序真正的内容

### Lecture03-软件调试基础.pdf / Page 6

DOS Header和Stub:
主要是兼容DOS loader
历史原因保留
PE Signature:
标识真正PE header的开始
COFF File Header:
主要字段包括Machine、
NumberOfSections、
TimeDateStamp、
SizeOfOptionalHeader、
Characteristics等
Optional Header:
描述如何被加载，包括
AddressOfEntryPoint、
ImageBase、
SectionAlignment、
FileAlignment、SizeOfImage
以及data directory
（功能表入口）

PE vs. ELF 格式

### Lecture03-软件调试基础.pdf / Page 7

PE vs. ELF 格式

ELF Header:
e_ident（文件标识）, e_type
（文件类型）, e_machine
（CPU架构）, e_entry（程序
入口）, e_phoff（Program
Header偏移）, e_shoff
（Section Header偏移）等
Program Header Table:
p_type（segment类型）,
p_offset（文件偏移）,
p_vaddr（虚拟地址）,
p_filesz（文件大小）,
p_memsz（内存大小）,
p_flags（权限）,
p_align（对齐）

### Lecture03-软件调试基础.pdf / Page 8

PE vs. ELF 格式
Section Table:
包括典型
section: .text, .data, .rdata, .bss,
.rsrc, .reloc
后面是各个section的
真实数据
这些section在内存中
被映射至不同权限页
.text: RX
.data: RW
.rdata: R

### Lecture03-软件调试基础.pdf / Page 9

PE vs. ELF 格式
Section Table:
每个entry描述一个section，
包含：
sh_name（section名）,
sh_type（类型）, sh_flags
（属性）, sh_addr（虚拟地
址）, sh_offset（文件偏移）,
sh_size（大小）
后面是真正的数据
除列出条目外，还包括：
.dynamic（动态链接信
息）, .plt（延迟绑定函数
表）, .got（全局偏移表，包
含真实函数地址）

### Lecture03-软件调试基础.pdf / Page 10

PE文件格式把可执行文件分成若干个数据节（section）
不同的资源被存放在不同的节中

存放程序的资源
如图标、菜单等

rsrc

text

由编译器产生，存放
二进制的机器代码
反汇编和调试的对象

data

初始化的数据块
如宏定义、全局变量、
静态变量等

PE文件
动态链接库等
外来函数与文件信息
即输入表

idata

除此以外，还可能出现的节包括“.reloc”、“.edata”、“.tls”、“.rdata”等。

### Lecture03-软件调试基础.pdf / Page 11

可执行文件
executable

内存布局
memory layout

代码和数据
入内存
加载

### Lecture03-软件调试基础.pdf / Page 12

PE vs. ELF 格式

### Lecture03-软件调试基础.pdf / Page 13

加载的核心步骤
1. 解析可执行文件结构
2. 构建进程内存映像
3. 地址重定位
4. 解析外部依赖
5. 运行初始化代码
6. 启动程序入口

### Lecture03-软件调试基础.pdf / Page 14

告诉loader PE header在哪里；DOS Stub对Windows loader几乎无用
读取“PE\0\0”部分，确定是合法PE文件

基址地址

入口地址

读取基本文件信息：CPU架构、section数量等
获取重要的加载信息：包括ImageBase，AddressOfEntryPoint,
Section/FileAlignment, DataDirectory

section在内存/文件中的
对齐粒度

CPU和操作系统要求
地址按某个粒度对齐

各种表的位置
包括Export、Import、Resource、
Relocation、TLS表

不对齐将导致：
• 加载效率低
• 内存保护无法正确设置
• Loader无法直接映射

### Lecture03-软件调试基础.pdf / Page 15

告诉loader PE header在哪里；DOS Stub对Windows loader几乎无用
读取“PE\0\0”部分，确定是合法PE文件
读取基本文件信息：CPU架构、section数量等
获取重要的加载信息：包括ImageBase，AddressOfEntryPoint,
Section/FileAlignment, DataDirectory
Loader为整个Image分配虚拟地址空间，然后读取section table
按section table映射section到内存
内存地址 = ImageBase + VirtualAddress
找到import table，之后loader会加载DLL、找到函数地址，填入IAT

•
•
•
•
•

进程已加载很多模块
如果ImageBase被占用，loader会做地址修正（relocation）
ASLR（地址空间随机化）
同一DLL被加载多次
手动内存分配
地址空间碎片

### Lecture03-软件调试基础.pdf / Page 16

ASLR

（Address Space Layout Randomization）

为什么需要？

核心思想

如果地址固定，则攻击更容易

每次运行随机化关键内存区域
stack, heap, libc, code

•
•
•

预测shellcode地址
覆盖返回地址
跳转到攻击代码

随机地址程序怎么执行？

绝对安全了吗？

Loader会随机选择地址，把程
序映射到那里，然后修正需要
绝对地址的地方（relocation）

不是的！

CPU不关心ASLR！

（1）信息泄漏
（2）暴力破解
（3）部分覆盖

### Lecture03-软件调试基础.pdf / Page 17

告诉loader PE header在哪里；DOS Stub对Windows loader几乎无用
读取“PE\0\0”部分，确定是合法PE文件
读取基本文件信息：CPU架构、section数量等
获取重要的加载信息：包括ImageBase，AddressOfEntryPoint,
Section/FileAlignment, DataDirectory
Loader为整个Image分配虚拟地址空间，然后读取section table
按section table映射section到内存
内存地址 = ImageBase + VirtualAddress
找到import table，之后loader会加载DLL、找到函数地址，填入IAT
如果ImageBase被占用，loader会做地址修正（relocation）
TLS（thread local storage），线程独有的数据，利用TLS回调执行初始化
最终跳转至入口点

### Lecture03-软件调试基础.pdf / Page 18

关于动态库
编译/链接阶段
1. 记录依赖动态库
Import Table
kernel32.dll
msvcrt.dll
2. 为函数建立跳转表
IAT（Import Address Table）
call printf -> call [IAT printf]

此时函数地址仍未知

运行时阶段

1. 加载所有动态库
main.exe -> kernel32.dll ->
user32.dll
2. 为每个库分配地址
msvcrt.dll -> 0x7f2a40000000
3. 解析符号
printf -> msvcrt.dll + offset
真实地址：0x7f2a40123450
4. 写入跳转表
IAT[printf] = 0x7f2a40123450

### Lecture03-软件调试基础.pdf / Page 19

如果是正常编译出的标准PE文件，其节信息往往是大致相同的。
但这些section的名字只是为了方便人的记忆与使用，使用
Microsoft Visual C++中的编译指示符：

#pragma data_seg()
可以把代码中的任意部分编译到PE的任意节中
节名也可以自己定义，如果可执行文件经过了“加壳”处理，
PE的节信息就会变得非常“古怪”。在Crack和反病毒分析
中需要经常处理这类古怪的PE文件。

### Lecture03-软件调试基础.pdf / Page 20

加壳

全称应该是可执行程序资源压缩，是保护文件的常
用手段。 加壳过的程序可以直接运行，但是不能查
看源代码。要经过脱壳才可以查看源代码。

Change what executable code looks like,
not its purpose

### Lecture03-软件调试基础.pdf / Page 21

为什么要加壳？
• 保护软件版权
• 增加逆向分析难度
• 降低程序体积，提升加载速度
• 攻击者隐藏代码，躲避安全检测

### Lecture03-软件调试基础.pdf / Page 22

加壳其实是利用特殊的算法，对EXE、DLL文件里的代码、资源进行压
缩、加密。类似WINZIP 的效果，只不过这个压缩之后的文件，可以独
立运行。附加在原程序上的解压程序通过Windows加载器载入内存后，
先于原始程序执行，得到控制权，执行过程中对原始程序进行解密、还
原，还原完成后再把控制权交还给原始程序，执行原来的代码部分。

加上外壳后，原始程序代码在磁盘文件中一般是以加密
后的形式存在的，只在执行时在内存中还原，这样就可
以比较有效地防止对程序文件的非法修改和静态反编译。

### Lecture03-软件调试基础.pdf / Page 23

加壳工具通常分为压缩壳和加密壳两类。

压缩壳的特点是减小软件体积大小，加密保护不
是重点。
加密壳种类比较多，不同的壳侧重点不同，
一些壳单纯保护程序，另一些壳提供额外的
功能，如提供注册机制、使用次数、时间限
制等。

### Lecture03-软件调试基础.pdf / Page 24

主要步骤
1. 解析原始可执行文件
2. 压缩/加密程序sections（.text, .rdata, .data）
3. 插入加载器桩代码（loader stub）
4. 改变程序入口点

### Lecture03-软件调试基础.pdf / Page 25

做什么？
桩代码作为
入口点

原始程序

加壳程序

### Lecture03-软件调试基础.pdf / Page 26

单选题

存放着所使用的动态链接库等外来函数与文件的信息的PE文件的
节是

A

code

B

edata

C

data

D

idata
提交

### Lecture03-软件调试基础.pdf / Page 27

知识点二：虚拟内存

### Lecture03-软件调试基础.pdf / Page 28

Windows安全模式

为了防止用户程序访问并篡改操作系统的关键部分，Windows使用了2种处理
器存取模式：用户模式和内核模式。用户程序运行在用户模式，而操作系统
代码（如系统服务和设备驱动程序）则运行在内核模式。在内核模式下程序
可以访问所有的内存和硬件，并使用所有的处理器指令。操作系统程序比用
户程序有更高的权限，使得系统设计者可以确保用户程序不会意外的破坏系
统的稳定性。

### Lecture03-软件调试基础.pdf / Page 29

用户态
•
•
•

隔离的环境
• 每个进程独有内存空间
无直接硬件访问
• 只有跟内核交互
安全执行
• 错误和崩溃不影响系统

内核态
•
•

系统调用
“桥梁性质”

•

直接的硬件交互
• IO设备、内存、处理器
完整的系统控制
• 管理进程调度、设备驱动
高权限指令执行
• 执行指令影响系统级行为

### Lecture03-软件调试基础.pdf / Page 30

虚拟内存

Windows的内存可以被分为两个层面：物理内存和虚拟内存。其中，物理内
存非常复杂，需要进入Windows内核级别ring0才能看到。通常，在用户模式
下，用调试器看到的内存地址都是虚拟内存。
用户编制程序时使用的地址称为虚拟地址或逻辑地址，其对应的存储空间称
为虚拟内存或逻辑地址空间；而计算机物理内存的访问地址则称为实地址或
物理地址，其对应的存储空间称为物理存储空间或主存空间。程序进行虚地
址到实地址转换的过程称为程序的再定位。

### Lecture03-软件调试基础.pdf / Page 31

为什么要区分
虚拟和物理内存
•

内存隔离角度
Ø 进程有独立的虚拟地址空间
Ø 程序A不会改到B的数据
Ø 虚拟内存通过页表 + MMU保证隔离

•

内存装载角度
Ø 无需了解物理地址
Ø 避免许多重编译/定位的麻烦

•

物理内存容量角度
Ø 允许只把正在用的页加载到RAM

### Lecture03-软件调试基础.pdf / Page 32

核心思想
CPU访问虚拟地址
硬件翻译为物理地址
翻译由MMU完成
Memory Management Unit

关键特征
1. 地址空间隔离
2. 内存保护
3. 按需加载
4. 交换空间
5. 共享内存

### Lecture03-软件调试基础.pdf / Page 33

进程空间

在Windows系统中，在运行PE文件时，操作系统会自动加载该文件到内存，
并为其映射出4GB的虚拟存储空间，然后继续运行，这就形成了所谓的进程
空间。用户的PE文件被操作系统加载进内存后，PE对应的进程支配了自己独
立的4GB虚拟空间。在这个空间中定位的地址称为虚拟内存地址（Virtual
Address，VA）。
到了现在，系统运行在X64架构的硬件上，可访问的内存也突破了以前4GB
的限制，但是独立的进程拥有独立的虚拟地址空间的内存管理机制还在沿用。

### Lecture03-软件调试基础.pdf / Page 34

PE文件与虚拟内存的映射
在调试漏洞时，可能经常需要做这样两种操作：

（1）

静态反汇编工具看到的PE文件中某条指令的位
置是相对于磁盘文件而言的，即所谓的文件偏移，
我们可能还需要知道这条指令在内存中所处的位
置，即虚拟内存地址。

（2）

反之，在调试时看到的某条指令的地址是虚
拟内存地址，我们也经常需要回到PE文件中
找到这条指令对应的机器码。

### Lecture03-软件调试基础.pdf / Page 35

指令地址换算
b/w PE与内存
某条指令 i 在PE中的地址

计算相对偏移

该指令 i 在内存中的地址

### Lecture03-软件调试基础.pdf / Page 36

需要弄清楚PE文件地址和虚拟内存地址之间的映射关系，先看几个重要的概念：
相对虚拟地址是内
存地址相对于映射
基址的偏移量

相对虚拟地址
（Relative Virtual
Address， RVA）

文件偏移地址
（File Offset）

数据在PE文件中的地
址叫文件偏移地址，
这是文件在磁盘上存
放时相对于文件开头
的偏移

重要概念：

PE文件中的指令被
装入内存后的地址

虚拟内存地址
（Virtual Address，
VA）

装载基址
（Image Base）

PE装入内存时的基地址。
默认情况下，EXE文件
在内存中的基地址是
0x00400000，DLL文件
是0x10000000。
这些位置可以通过修改
编译选项更改

### Lecture03-软件调试基础.pdf / Page 37

相对基址ImageBase的偏移

文件中section的起始地址

内存中section的起始地址
VirtualAddress（连在一起）表示一个字段
指令在文件中的偏移

### Lecture03-软件调试基础.pdf / Page 39

虚拟内存地址、映射基址、相对虚拟内存地址三者之间有如下关系：
)*

VA = Image Base + RVA
+,-.

+,
!"*!!!"
!"*!!!"

$%&'&"

$%&'&"

!"#!!!!"

$E%&'&"

!"#!!!!"

$')"'"

!"#!!!!"

-./0"12

#$%&
'!"#$B&'#EB(
)*))+)))))

$E%&'&"

!"*!!!"

$')"'"

I--.34
)!"

-./0"12

由于操作系统在进行装载时“基本”上保持PE中的各种数据结构，所以文件偏移地址和RVA有很大的一致性。

### Lecture03-软件调试基础.pdf / Page 40

差异
由于文件数据的存放单位与内存数据存放单位不同而造成一些差异：

PE文件中的数据按照磁盘数据标准存
放，以0x200字节为基本单位进行组织。
当一个数据节（section）不足0x200字
节时，不足的地方将被0x00填充：当一
个 数 据 节 超 过 0x200 字 节 时 ， 下 一 个
0x200块将分配给这个节使用。因此PE
数据节的大小永远是0x200的整数倍。

当代码装入内存后，将按照内存数
据标准存放，并以0x1000字节为基
本单位进行组织。类似的，不足将
被补全，若超出将分配下一个
0x1000为其所用。因此，内存中的
节总是0x1000的整数倍。

### Lecture03-软件调试基础.pdf / Page 41

为什么内存的数据节更大？

### Lecture03-软件调试基础.pdf / Page 42

为什么内存的数据节更大？

1. 未初始化数据（.bss）
Ø 文件不需要存储
Ø 内存需要分配空间
2. 内存对齐
Ø Loader把section加载入内存需按页对齐
Ø 文件中section可以很紧凑，内存必须按页扩展

### Lecture03-软件调试基础.pdf / Page 43

LordPE是一款功能强大的PE文件分析、修改、脱壳软件。LordPE是查看
PE格式文件信息的首选工具，并且可以修改相关信息。

### Lecture03-软件调试基础.pdf / Page 44

单击“PE Editor”按钮，选择需要查看的PE文件，如下图所示：

查看节信息
查看导入
导出表
地址换算

### Lecture03-软件调试基础.pdf / Page 45

点击Sections按钮，可以查看节信息：

在上图中，VOffset是RVA（相对虚拟地址），ROffset是文件偏移。也就是，在系统进程
中，代码（.text节）将被加载到0x400000+0x11000=0x411000的虚拟地址中（装载基址
+RVA）。而在文件中，可以使用二进制文件打开，看到对应的代码在0x1000位置处。

### Lecture03-软件调试基础.pdf / Page 46

使用Lord PE查看导入表信息

导 入 表 在 文 件 里 的 偏 移 地 址 ROffset 为
0x24000，RVA是0x25000。打开目录表可以
看到，可以看到输入表的RVA确实是0x25000。
点左侧按钮L可以查看具体输入表里的内容。

### Lecture03-软件调试基础.pdf / Page 47

IAT(Import Address Table:输入函数地址表)表信息

认识IAT表。每个API函数在对应的进程空间
中都有其相应的入口地址。众所周知，操作系
统动态库版本的更新，其包含的API函数入口
地址通常也会改变。由于入口地址的不确定性，
程序在不同的电脑上很有可能会出错，为了解
决程序的兼容问题，操作系统就必须提供一些
措施来确保程序可以在其他版本的Windows操
作系统，以及DLL版本下也能正常运行。这时 基于导入表可以定位IAT的具体信息，相关工具
可以帮助直接查看IAT表的相关内容。
IAT表就应运而生了。

### Lecture03-软件调试基础.pdf / Page 48

PEView工具—直观显示PE文件内容

### Lecture03-软件调试基础.pdf / Page 49

1. LordPE (Windows)
2. readelf (Linux自带)
3. objdump (Linux，看section，反汇编)
4. Cutter (Linux/Windows/MacOS，GUI支持)
5. ImHex（逆向工程可视化）
6. IDA Pro（反汇编，decompiler，可分析多种executable）

### Lecture03-软件调试基础.pdf / Page 50

单选题

用户模式下，用调试器看到的内存地址都是

A

真实地址

B

物理地址

C

虚拟内存

D

以上都不对
提交

### Lecture03-软件调试基础.pdf / Page 51

知识点三：调试分析工具

### Lecture03-软件调试基础.pdf / Page 52

OllyDbg
是一种具有可视化界面的 32 位汇编—分析调试器，适合动态调试。
OllyDBG版的发布版本是个ZIP 压缩包，解压就可以使用了。

### Lecture03-软件调试基础.pdf / Page 53

OllyDbg的基本调试方法
OllyDBG 有两种方式来载入程序进行调试
一种是点击菜单文件
打开( 快捷键是F3) 来打开可执行文件进行调试；

另一种是点击菜单文件
附加来附加到一个己运行的进程上进行调试，要附加的程序必须己运行。

### Lecture03-软件调试基础.pdf / Page 54

比如我们选择一个 test.exe 来调试，通过菜单 文件->打开 来载入这个程序，OllyDBG 中显示的
内容将会是这样：

### Lecture03-软件调试基础.pdf / Page 55

快捷键

F2

设置断点。

F8

单步步过。执行一条
指令，遇到 CALL 等
子程序不进入其代码。

F7

单步步入。功能同单
步步过(F8)类似，区别
是遇到 CALL 等子程
序时会进入其中，进
入后首先会停留在子
程序的第一条指令上。

F4

运行到选定位置。

### Lecture03-软件调试基础.pdf / Page 56

快捷键
ALT+
F9

CTR+
F9

F9

运行。

执行到用户代码。可用于
从系统领空快速返回到我
们调试的程序领空。

执行到返回。此命令在执行到一个 ret
(返回指令)指令时暂停，常用于从系统
领空返回到我们调试的程序领空。

### Lecture03-软件调试基础.pdf / Page 57

跟踪
使用调试功能时通常会碰到在断点处无法定位入口的情况，即无法确定前序执行指
令，通过Trace（跟踪）功能可以记录调试过程中执行的指令，用于分析前序执行指
令。Trace记录可选择是否记录寄存器的值。

### Lecture03-软件调试基础.pdf / Page 58

IDA PRO
简称IDA（Interactive Disassembler），是一个世界顶级的交互式反汇编工具，
是逆向分析的主流工具。

IDA使用File菜单中的Open选项，可以打开一个计划逆
向分析的可执行文件，打开的过程是需要耗费一些时
间的。IDA会对可执行文件进行分析。一旦打开成功，
会提示你是否进入Proximity view。通常都会点Yes，按
默认选项进入。

### Lecture03-软件调试基础.pdf / Page 59

如下图的Proximity view：

### Lecture03-软件调试基础.pdf / Page 60

反汇编窗口
也叫IDA-View窗口，是操作和分析二进制文件的主要工具。
反汇编窗口有三种显示格式

视图间可以切换：在上图的Proximity view视图中，点选一个块，比如_main函数块，在其
上点右键，可以看到Text view和Graph view等选项。通过右键可以实现不同视图的切换。

### Lecture03-软件调试基础.pdf / Page 61

图形视图：将一个函数分解为许多基本块，类似程序流程图类似，生动的显示
该函数由一个块到另一个块的控制流程。
如下图所示的_main函数的图形视图：

Yes边的箭头默认为绿色，No边的箭头默认为红色。蓝色箭头表示指向下一个即将执行的块

### Lecture03-软件调试基础.pdf / Page 62

文本视图：文本视图则呈现一个程序的完整反汇编代码清单（而在图形模式下
一次只能显示一个函数），用户只有通过这个窗口才能查看一个二进制文件的
数据部分。如下图所示的文本视图：

通常虚拟地址以
[区域名称]：[虚拟地址]
这种格式显示，
如.txt:0040110C0。

实线箭头表示非条件跳转，虚线箭头则表示条件跳转。如果一个跳转将控制权交给程序中的某个地址，这时会使用粗
线，出现这类逆向流程，通常表示程序中存在循环。

### Lecture03-软件调试基础.pdf / Page 63

通过菜单Views Open subviews可以打开更多的窗口。
Names窗口：列举二进制文件的所有全局名称。名称是指对一个程序虚拟地址的符号描述。
Names窗口显示的名称采用了颜色和字母编码，其编码方案如下：

### Lecture03-软件调试基础.pdf / Page 64

Strings窗口

显示从二进制文件中提取出的字符串，以及每个字符串所在的地址。与双
击Names窗口中的名称得到的结果类似，双击Strings窗口中的任何字符串，
反汇编窗口将跳转到该字符串所在的地址。将Strings窗口与交叉引用结合，
可以迅速定义感兴趣的字符串，并追踪到程序中任何引用该字符串的位置。

### Lecture03-软件调试基础.pdf / Page 65

Function name窗口
该窗口显示所有的函数。点击函数名称，可以快速导航到反汇编视图中的该函
数区域。该窗口中的条目如下：

这一行信息指出：用户可以在二进制文件中虚拟地址为00401040
的.text部分中找到_main函数，该函数长度为0x50字节。

### Lecture03-软件调试基础.pdf / Page 66

Function call窗口
函数调用（Function call）窗口将显示所有函数的调用关系。如下图：

### Lecture03-软件调试基础.pdf / Page 67

反编译
新版本的IDA增加了反编译功能，加强了分析能力。
在IDA View窗口下制定汇编代码，按快捷键F5，IDA会将当前所在位置的汇编
代码编译成C/C++形式的代码，并在Pseudocode窗口中显示，如下图所示。

### Lecture03-软件调试基础.pdf / Page 68

知识点四：PE文件代码注入示例

### Lecture03-软件调试基础.pdf / Page 69

演示内容及实验环境
利用PE文件输入表API实现代码注入：让目标程序运行之前，先运行我们注入
的代码，注入的代码将运行PE文件输入表里包含的API。
目标PE文件为Windows XP下的扫雷程序，使用的工具包括OllyDBG和LordPE。
扫雷游戏程序位置：在Windows下找到附件里的扫雷游戏，右键属性可以看到
具体文件的位置，即C:\WINDOWS\system32\winmine.exe。

### Lecture03-软件调试基础.pdf / Page 70

用OllyDBG打开扫雷程序

程序会停下来，自动停下来的这一行代码位置就是程序入口点。可以通过LordPE文件来查看，得知程序
入口点的RVA是0x00003E21，同时也可以看到装载基址是0x01000000（扫雷程序是C++语言编写）；也
可以通过右侧寄存器EIP的值0x01003E21可以观察到注释信息里，提示是ModuleEntryPoint。
反汇编区域继续往下翻页，可以看到相关的导入表动态链接库及其相关函数的信息。

### Lecture03-软件调试基础.pdf / Page 71

在空白代码区编写要注入的代码
在代码区可以找到大量的空白代码区域，如果我们往这里头植入代码，直接
修改PE文件相关跳转地址（入口点或特定函数的IAT的跳转地址），就可以
实现相关的植入代码的执行。
本实验：演示让扫雷程序运行之前，先运行我们注入的代码，注入的代码将
调用PE文件输入表里包含的MessageBox函数，弹出对话框，显示相关信息。

### Lecture03-软件调试基础.pdf / Page 72

1. 编辑和注入代码
MSDN对MessageBox函数的解释如下：
int MessageBox(
HWND hWnd,

// handle to owner window

LPCTSTR lpText,

// text in message box

LPCTSTR lpCaption,
UINT uType

// message box title

// message box style

);
l hWind：消息框所属窗口的句柄，如果为NULL，消息框则不属于任何窗口。
l lpTex：字符串指针，所指字符串会在消息框中显示。
l lpCaption：字符串指针，所指字符串将成为消息框的标题。
l uType：消息框的风格（单按钮、多按钮等），NULL代表默认风格。
系统中并不存在真正的MessageBox函数，调用最终都将由系统按照参数中的字符串的类型选
择“A”类函数（ASCII）或者“W”类函数（UNICODE）调用，我们使用MessageBoxA.

### Lecture03-软件调试基础.pdf / Page 73

编辑和注入代码
计划注入的代码功能为：弹出对话框，显示“You are Injected!”。
首先，要构造相关的字符串，然后，构造函数调用的相关汇编代码。
在代码空白区域每一行位置，点鼠标右键，选择 编辑à二进制编辑
（不同的OllyDBG版本可能稍有差异）

### Lecture03-软件调试基础.pdf / Page 74

编辑和注入代码
在弹出的编辑界面里，输入ASCII码“PE Inject”，将“保持代码空间大小”
去掉选中状态，状态如下：

按快捷键CTRL+A（分析），显示为ASCII码。再加入另外一条语句“You
are Injected!” ：

### Lecture03-软件调试基础.pdf / Page 75

编辑和注入代码
上面的每个语句后面都留了一行00。因为，字符串后面是需要结束符0x00的。
接下来，构造函数调用的代码，包括：
push 0（默认风格）;
push 0x01004AA7（标题字符串地址）;
push 0x01004A9C（内容字符串地址）;
push 0（窗口归属）;
call MessageBoxA
注意，直接双击要修改的当前行，就进入修改汇编代码的状态，如下：

### Lecture03-软件调试基础.pdf / Page 76

编辑和注入代码
我们输入的汇编指令call MessageBoxA之所以后面能成功运行，也是因为PE
文件的输入表里已经有这个函数的入口地址了。以上代码完成输入后，结果
如下：

### Lecture03-软件调试基础.pdf / Page 77

2. 挂接代码及完成跳转
我们首先继续输入一条指令 jmp 0x01003E21。这句话意思是我们运行完我
们注入的弹出对话框之后，会跳转到我们原来的这个PE文件的入口点，继续
运行。结果如下图：

### Lecture03-软件调试基础.pdf / Page 78

2. 挂接代码及完成跳转
注意，上述修改是在原始文件副本里修改的，如果要保存修改，需要：
（1）点鼠标右键，选择“编辑à复制所有修改到可执行文件”，会弹出一个对
话框，包含所有修改后的代码；
（2）在这个对话框空白处继续点右键“编辑à保存文件”，弹出保存文件的界
面，在这个里面选择保存类型为“可执行文件或DLL”，输入新的文件名，比如
winmine1.exe，点保存即可。
到此，文件修改完毕，但是如果直接运行这个扫雷程序，并没有发生任何变
化。 因为，我们只是编辑了一段代码，只有这些代码被运行了才算真正被注入。

### Lecture03-软件调试基础.pdf / Page 79

2. 挂接代码及完成跳转
利用LordPE文件，我们更改一下程序入口点，为我们的程序的起始位置，即我
们编辑的代码段的第一个push 0的位置，地址为0x01004ABA，因为只需要更
改RVA，就修改为0x00004ABA即可，如下图：

保存后运行，可以看到弹出右侧对话框，之后出现扫雷程序。

### Lecture03-软件调试基础.pdf / Page 80

知识点五：软件破解示例

### Lecture03-软件调试基础.pdf / Page 81

OllyDBG示例
本节将对一个简单的密码验证程序，使用OllyDBG进行破解。具体程序如下

#include <iostream>
using namespace std;
#define password "12345678"
bool verifyPwd(char * pwd)
{
int flag;
flag=strcmp(password, pwd);
return flag==0;
}

### Lecture03-软件调试基础.pdf / Page 82

void main()
{
bool bFlag;
char pwd[1024];
printf("please input your password:\n");
while (1)
{
scanf("%s",pwd);
bFlag=verifyPwd(pwd);
if (bFlag)
{
printf("passed\n");
break;
}else{
printf("wrong password, please input again:\n");
}
}
}

### Lecture03-软件调试基础.pdf / Page 83

破解对象是该程序生成的Debug模式的exe程序。
对得到的exe程序（假定不知道上面的源代码），有多种方式实现破解:
一种方式是使用OllyDBG
通过运行程序，观察关键信息，通过对关键信息定位，来得到关键分支
语句，通过对该分支语句进行修改，达到破解的目的;
另外一种方式
可以通过IDA Pro来观察代码结构，确定函数入口地址，对函数体返回值
进行更改。

### Lecture03-软件调试基础.pdf / Page 84

运行程序，输入一个密码，发现运行结果如下：

### Lecture03-软件调试基础.pdf / Page 85

在OllyDBG中，为了尽快定位到分支语句处，在反汇编窗口，
点右键，选择“查找→所有引用的字符串”功能：

### Lecture03-软件调试基础.pdf / Page 86

然后，使用快捷键，Ctrl+F打开搜索窗口，输入wrong，点确
定后，将定位出错信息的哪一行代码：

### Lecture03-软件调试基础.pdf / Page 87

双击这一行代码，就会定位反汇编中的相应代码处

### Lecture03-软件调试基础.pdf / Page 88

破解方式一
观察反汇编语言，可知核心分支判断在于：

如果jz条件成立，则跳转到0041364b处，即显示错误密码分支语句中。如果将jz该指
令改为jnz，则程序截然相反。输入了错误密码，将进入验证成功的分支中。
双击jz密码一行，对其进行修改：

### Lecture03-软件调试基础.pdf / Page 89

点修改当前汇编代码即可。

注意：
此时并没有真正修改二进制文件中的有关代码，如果想要修改二进制
文件中的代码，需要在反汇编窗口，点右键，选择“编辑->复制当前
修改到可执行文件”。保存后的可执行文件，将是破解后的文件。

### Lecture03-软件调试基础.pdf / Page 90

破解方式二
更改函数。通过分析汇编语句，可知，验证命令使用的是verifyPwd函数，点右
键选择跟随，逐步进入该函数：

### Lecture03-软件调试基础.pdf / Page 91

函数的返回值通过eax寄存器来完成的，核心语句即sete al。
对于函数中的代码：

### Lecture03-软件调试基础.pdf / Page 92

被解释为汇编语言：
//将strcmp函数调用后的返回值（存在eax中）赋值给
变量flag

//将eax的值清空

//将flag的值与0进行比较，即flag==0;
//注意cmp运算的结果只会影响一些状态寄存器的值

//sete是根据状态寄存器的值，如果相等，则设置，如
果不等，则不设置

### Lecture03-软件调试基础.pdf / Page 93

要想更改该语句，在cmp dword ptr [ebp-8], 0处开始更改，将其更改为：
mov al,01。取消保持代码空间大小，如果新代码超长，将无法完成更改。

### Lecture03-软件调试基础.pdf / Page 94

并将sete al改为NOP。
得到结果如下：

运行结果校验破解正确性

### Lecture03-软件调试基础.pdf / Page 95

单选题

1分

XOR EAX, EAX 汇编语句执行后，EAX寄存器的值为

A

0

B

1

C

2

D

FFFFFFFF
提交

### Lecture03-软件调试基础.pdf / Page 96

单选题

1分

CMP EAX, EAX；SETE AL执行后AL寄存器的值为

A

0

B

1

C

2

D

FFFFFFFF
提交

