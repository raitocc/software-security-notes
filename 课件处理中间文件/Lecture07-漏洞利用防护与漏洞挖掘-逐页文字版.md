# Lecture07 漏洞利用防护与漏洞挖掘 - 逐页文字版

> 说明：本文件由 PDF 文本层逐页抽取生成，用于核对覆盖；复习请优先看同目录下的“复习讲义”。

## 来源：lecture07part1-漏洞利用防护.pdf

### lecture07part1-漏洞利用防护.pdf / Page 1

漏洞利用的防护

### lecture07part1-漏洞利用防护.pdf / Page 2

1

覆盖返回地址

2

劫持EIP/RIP

3

跳转到shellcode

4

执行恶意代码

### lecture07part1-漏洞利用防护.pdf / Page 4

核心

不是修复漏洞，而是让漏洞利用更难

### lecture07part1-漏洞利用防护.pdf / Page 5

问题
直接修复漏洞不是更好吗？

### lecture07part1-漏洞利用防护.pdf / Page 6

问题
直接修复漏洞不是更好吗？
修复漏洞其实是第一优先级
但无法做到所有漏洞都立刻
发现并修复

### lecture07part1-漏洞利用防护.pdf / Page 7

1 ASLR

地址空间分布随机化ASLR(addressspace layout randomization)是一
项通过将系统关键地址随机化,从而使攻击者无法获得需要跳转
的精确地址的技术。
Shellcode需要调用一些系统函数才能实现系统功能达到攻击目的，
因为这些函数的地址往往是系统DLL（如kernel32. Dll）、可执行文
件本身、栈数据或PEB（Process Environment Block，进程环境块）
中的固定调用地址，所以为shellcode的调用提供了方便。

### lecture07part1-漏洞利用防护.pdf / Page 8

1 ASLR

核心思想：模块地址随机变化
随机什么？
（1）Stack起始地址
（2）Heap分配地址
（3）DLL加载地址
（4）程序代码基址

### lecture07part1-漏洞利用防护.pdf / Page 9

1 ASLR

核心思想：模块地址随机变化
防什么？
• ret2libc

地址跳转

• ROP
• shellcode跳转

### lecture07part1-漏洞利用防护.pdf / Page 10

1 ASLR

核心思想：模块地址随机变化
如何突破？
• 信息泄漏
• brute force
• 非ASLR模块

### lecture07part1-漏洞利用防护.pdf / Page 11

1 ASLR

核心思想：模块地址随机变化
如果地址随机化，CPU如何执行？

### lecture07part1-漏洞利用防护.pdf / Page 12

1 ASLR

核心思想：模块地址随机变化
如果地址随机化，CPU如何执行？
CPU只需知道当前进程映射后的虚拟地址
ASLR随机化后，Loader负责修正地址，CPU只执行修正后地址
CPU执行的是运行时地址，而非编译时地址

### lecture07part1-漏洞利用防护.pdf / Page 13

对于ASLR技术，微软从操作系统加载时的地址变化和可执行程序编译时的
编译器选项两个方面进行了实现和完善。
系统加载地址变化
ü ASLR随机化的关键系统地址包括: PE文件(exe文件和dll文件)映像加载地址、堆栈基址、
堆地址、PEB和TEB（Thread Environment Block，线程环境块）地址等。
ü 在Windows Vista上，当程序启动将执行文件加载到内存时，操作系统通过内核模块提供的
ASLR功能，在原来映像基址的基础上加上一个随机数作为新的映像基址。
ü 随机数的取值范围限定为1至254，并保证每个数值随机出现。
编译器选项-DYNAMICBASE
VS 2005及更高版本提供了选项/DYNAMICBASE，使用了该选项之后，编译后的程序每次运
行时，其内部的栈等结构的地址都会被随机化。

### lecture07part1-漏洞利用防护.pdf / Page 14

实验四：在Windows 7及以后的操作系统里运行下述程序，查看地址变化情
#define DLL_NAME
"kernel32.dll"
况
unsigned long gvar = 0;
void PrintAddress() {
printf("PrintAddress的地址:%p \n", PrintAddress);
gvar++;
}
int main(){
HINSTANCE handle;
handle = LoadLibrary(DLL_NAME);
if (!handle) {
printf(" load dll erro !"); exit(0);
}
printf("Kernel32.dll文件库的地址： 0x%x\n", handle);
void *pvAddress = GetProcAddress(handle, "LoadLibraryW");
printf("LoadLibrary函数地址：%p \n", pvAddress);
PrintAddress();
printf("变量gvar的地址：%p \n", &gvar);
system("pause");
return 0;
}

GetProcAddress函数
可以获得DLL中的函数地址

### lecture07part1-漏洞利用防护.pdf / Page 15

2 GS Stack protection

GS Stack Protection技术是一项缓冲区溢出的检测防护技术。VC++编译器
中提供了一个/GS编译选项，在使用VC7.0、Visual Studio 2005及后续版
本编译时都支持该选项，如选择该选项，编译器针对函数调用和返回时
添加保护和检查功能的代码，在函数被调用时，在缓冲区和函数返回地
址增加一个32位的随机数security_cookie，在函数返回时，调用检查函数
检查security_cookie的值是否有变化。

### lecture07part1-漏洞利用防护.pdf / Page 16

2 GS Stack protection

为什么也叫stack canary？
Canary in a
coal mine
预警信号

### lecture07part1-漏洞利用防护.pdf / Page 17

2 GS Stack protection

防什么？
覆盖返回地址

### lecture07part1-漏洞利用防护.pdf / Page 18

2 GS Stack protection

防什么？
覆盖返回地址
插入随机值
例如，0x3fa912bc
函数进入时，编译器自动插入
返回时检查

### lecture07part1-漏洞利用防护.pdf / Page 19

2 GS Stack protection

如何突破？
• 信息泄漏
• 非连续写（只修改返回地址，不碰cookie）
• 非ret路径利用（如SEH利用 ）

### lecture07part1-漏洞利用防护.pdf / Page 20

3

DEP
数据执行保护DEP (data execution prevention) 技术可以限制内存堆栈区的代
码为不可执行状态，从而防范溢出后代码的执行。
Windows操作系统中，默认情况下将包含执行代码和DLL文件的.text段即代码段的
内存区域设置为可执行代码的内存区域。其他的内存区域不包含执行代码，应该
不能具有代码执行权限，但是Windows XP及其之前的操作系统，没有对这些内存
区域的代码执行进行限制。因此，对于缓冲区溢出攻击，攻击者能够对内存的堆
栈或堆的缓冲区进行覆盖操作，并执行写入的shellcode代码。
启用DEP机制后，DEP机制将这些敏感区域设置不可执行的non-executable标
志位，因此在溢出后即使跳转到恶意代码的地址，恶意代码也将无法运行，
从而有效地阻止了缓冲区溢出攻击的执行。

### lecture07part1-漏洞利用防护.pdf / Page 21

3

DEP

核心思想：数据区不能执行代码
• stack
• heap
• bss

RW

RWX

### lecture07part1-漏洞利用防护.pdf / Page 22

3

DEP

作为shellcode写入栈中

### lecture07part1-漏洞利用防护.pdf / Page 23

3

DEP

核心思想：数据区不能执行代码
• stack
• heap
• bss

RW

RWX

不能执行栈/堆上的shellcode

### lecture07part1-漏洞利用防护.pdf / Page 24

3

DEP

如何突破？
• ret2libc
• ROP（Return Oriented Programming）
• VirtualProtect（先改权限再跳转）

重用已有代码，而非注入新代码

### lecture07part1-漏洞利用防护.pdf / Page 25

3

DEP

DEP分为软件DEP和硬件DEP。硬件DEP需要CPU的支持,需要CPU在页
表增加一个保护位NX(no execute)，来控制页面是否可执行。现在CPU
一般都支持硬件NX，所以现在的DEP保护机制一般都采用的硬件DEP，
对于DEP设置non-executable标志位的内存区域，CPU会添加NX保护位
来控制内存区域的代码执行。
此外，Visual Studio编译器提供了一个链接标志/NXCOMPAT，可以在生成目
标应用程序的时候使程序启用DEP保护。

### lecture07part1-漏洞利用防护.pdf / Page 26

4

SafeSEH
SEH
SEH（Structured Exception Handler）是Windows异常处理机制所采用的重要数据结构链表。
程序设计者可以根据自身需要，定义程序发生各种异常时相应的处理函数，保存在SEH中。
通过精心构造，攻击者通过缓冲区溢出覆盖SEH中异常处理函数句柄，将其替换为指向恶意
代码shellcode的地址，并触发相应异常，从而使程序流程转向执行恶意代码。
SafeSEH
SafeSEH就是一项保护SEH函数不被非法利用的技术。微软在编译器中加入了/SafeSEH选项,
采用该选项编译的程序将PE文件中所有合法的SEH异常处理函数的地址解析出来制成一张
SEH函数表，放在PE文件的数据块中,用于异常处理时候进行匹配检查。

### lecture07part1-漏洞利用防护.pdf / Page 27

4

SafeSEH
在该PE文件被加载时，系统读出该SEH函数表的地址，使用内存中的一个
随机数加密，将加密后的SEH函数表地址、模块的基址、模块的大小、合
法SEH函数的个数等信息，放入ntdll.dll的SEHIndex结构中。
在PE文件运行中，如果需要调用异常处理函数，系统会调用加解密函数解
密从而获得SEH函数表地址，然后针对程序的每个异常处理函数检查是否
在合法的SEH函数表中，如果没有则说明该函数非法，将终止异常处理。
接着要检查异常处理句柄是否在栈上，如果在栈上也将停止异常处理。这
两个检测可以防止在堆上伪造异常链和把shellcode放置在栈上的情况，最
后还要检测异常处理函数句柄的有效性。
从Vista开始，由于系统PE文件在编译时都采用SafeSEH编译选项，因此以
前那种通过覆盖异常处理句柄的漏洞利用技术，也就不能正常使用了。

### lecture07part1-漏洞利用防护.pdf / Page 28

5

SEHOP
结构化异常处理覆盖保护SEHOP（Structured Exception Handler Overwrite
Protection）是微软针对SEH攻击提出的一种安全防护方案。

SEH攻击是指通过栈溢出或者其他漏洞，使用精心构造的数据覆盖SEH
上面的某个函数或者多个函数，从而控制EIP（控制程序执行流程）。

### lecture07part1-漏洞利用防护.pdf / Page 29

5

SEHOP
SEHOP的核心是检测程序栈中的所有SEH结构链表的完整性，来判
断应用程序是否受到了SEH攻击。
SEHOP针对下列条件进行检测，包括：
SEH结构都必须在栈上，最后一个SEH结构也必须在栈上；
所有的SEH结构都必须是4字节对齐的；
SEH结构中异常处理函数的句柄handle（即处理函数地址）必须不在栈上；
最后一个SEH结构的handle必须是ntdll!FinalExceptionHandler函数F等。

### lecture07part1-漏洞利用防护.pdf / Page 30

单选题

1分

能有效阻止非代码区的植入的恶意代码执行的技术是

A

ASLR

B

GS

C

SEHOP

D

DEP
提交

### lecture07part1-漏洞利用防护.pdf / Page 31

需要说明的是，虽然微软启用了GS、DEP、ASLR、SafeSEH、SEHOP等漏
洞利用的防护技术，然而攻击者也在陆续发现着其他的漏洞利用手段，突
破微软的防护技术。用魔高一尺道高一丈来描述两者间在漏洞利用技术上
的对抗，一点也不为过。
接下来，介绍一些进一步的漏洞利用技术。

### lecture07part1-漏洞利用防护.pdf / Page 32

返回导向编程
Return-Oriented Programming
(ROP)

### lecture07part1-漏洞利用防护.pdf / Page 33

ROP为什么重要？
ESP

注入shellcode
1

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

2

Buffer[40…43]机器码

覆写为buffer地址

Flag
EBP

前栈帧EBP
返回地址
高地址

### lecture07part1-漏洞利用防护.pdf / Page 34

数据区代码
不可执行

ROP为什么重要？
ESP

注入shellcode
1

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

2

Buffer[40…43]机器码

覆写为buffer地址

Flag

DEP阻止
shellcode执行

EBP

前栈帧EBP
返回地址
高地址

### lecture07part1-漏洞利用防护.pdf / Page 35

ROP为什么重要？

### lecture07part1-漏洞利用防护.pdf / Page 36

ROP为什么重要？

栈中存的是
已有指令地址
而非注入指令

### lecture07part1-漏洞利用防护.pdf / Page 37

ROP为什么重要？

栈中存的是
已有指令地址
而非注入指令

绕过DEP

### lecture07part1-漏洞利用防护.pdf / Page 38

1

基本思想
ROP

ROP的全称为Return-oriented programming（返回导向编程）；
是一种新型的基于代码复用技术的攻击，它从已有的库或可执行文件中提取指
令片段，构建恶意代码。

### lecture07part1-漏洞利用防护.pdf / Page 39

1

基本思想
ROP

ROP的全称为Return-oriented programming（返回导向编程）；
是一种新型的基于代码复用技术的攻击，它从已有的库或可执行文件中提取指
令片段，构建恶意代码。
ROP的基本思想

借助已存在的代码块(也叫配件，Gadget)，这些配件来自程序已经加载的模块；
在已加载的模块中找到一些以retn结尾的配件，把这些配件的地址布置在堆栈
上, 当控制EIP并返回时候, 程序就会跳去执行这些小配件；
这些小配件是在别的模块代码段, 不受DEP的影响。

### lecture07part1-漏洞利用防护.pdf / Page 40

ROP Gadget
程序中已有的一小段指令

pop rdi;

通常以ret结尾

ret

### lecture07part1-漏洞利用防护.pdf / Page 41

ROP Gadget
程序中已有的一小段指令

pop rdi;

通常以ret结尾

ret

为什么以ret结尾？

从栈中取
下一个地址执行

### lecture07part1-漏洞利用防护.pdf / Page 42

ROP链

### lecture07part1-漏洞利用防护.pdf / Page 43

ROP链

构成新程序

### lecture07part1-漏洞利用防护.pdf / Page 44

对于ROP技术，可以总结为如下三点：

1

ROP通过ROP链（retn）实现有序汇编指令的执行。

2

ROP链由一个个ROP小配件（Gadget，相当于一个小节点）
组成。

3

ROP小配件由“目的执行指令+retn指令组成”。

### lecture07part1-漏洞利用防护.pdf / Page 45

2

示例

???????=>表示当前返回地址里包含的指令及跳转到该指令处执行

示例5-11（指针只执行retn）：初始状
态：即将执行retn命令，此时ESP指向
返回地址，而且返回地址以及后面4个
地址里都通过溢出覆写了多个RETN指
令的地址。

初始状态，EIP指针指向命令retn
ESP -> ???????? => RETN
???????? => RETN
???????? => RETN
???????? => RETN

……
%&%%'(')**
%&%%'(')**

-.I
-.I

!"#$%&'(
%&%%'(')**
%&%%'(**+%
%&%%')"*%%

!"#$

……
%&%%'(**+%
%&%%'(**+%

!"#$

%&%%')"*%%,,!"#$

### lecture07part1-漏洞利用防护.pdf / Page 46

下面来演示“指令+retn”的配件的用法，实现一定的逻辑。示例5-7（指针指向一些指令+retn）：

ESP -> ???????? => POP EAX # RETN
ffffffff => we put this value in EAX
???????? => INC EAX # RETN
???????? => XCHG EAX,EDX # RETN
在这个例子中：
[1].执行retn之后，EIP指向了指令段“POP EAX #RETN”，ESP指向了高地址中的“0xffffffff”。此时，执
行POP EAX，结果是将0xffffffff赋值给EAX；
[2].然后执行RETN的时候，EIP指向了“INC EAX # RETN”，ESP指向了下一个高地址。此时，执行INC
EAX，这样EAX的值将由0xffffffff变为0；
[3].再执行RETN的时候，EIP指向了“XCHG EAX,EDX # RETN”，ESP指向了下一个高地址。此时，执行
XCHG EAX,EDX，这样EDX的值就变为0。
可见，通过上面的ROP指令段，我们实现了将EDX置0的结果。

### lecture07part1-漏洞利用防护.pdf / Page 48

通过代码重用
打开终端
绕过DEP

system("/bin/sh")

### lecture07part1-漏洞利用防护.pdf / Page 49

来自libc

Buffer溢出覆写已有代码/数据地址

### lecture07part1-漏洞利用防护.pdf / Page 50

函数返回跳转执行
system

### lecture07part1-漏洞利用防护.pdf / Page 51

没有使用CALL
自行压入返回地址

可用exit地址代替

“/bin/sh”

### lecture07part1-漏洞利用防护.pdf / Page 52

system("/bin/sh")
exit()

### lecture07part1-漏洞利用防护.pdf / Page 53

来自libc

栈上的payload明明是我写入的
DEP为什么没防住？

### lecture07part1-漏洞利用防护.pdf / Page 54

来自libc

只有数据写入栈中
0xf7e12420对应libc中system
而libc的.text代码是可执行的

### lecture07part1-漏洞利用防护.pdf / Page 55

3

基于ROP的漏洞利用

ROP可以通过一些小配件构建期待的目标指令序列，但是因为它严重依赖内存中
已存在的代码序列，因此，构建复杂和大规模的代码序列是非常难的。
在实际应用中，基于ROP编写的代码序列可以利用有限的编码完成下述目标来达
到攻击的目的：

1

调用相关API关闭或绕过DEP保护。相关的API包括SetProcessDEPPlolicy、
VirtualAlloc、NtSetInformationProcess、VirtualProtect等，比如VirtualProtect函数
可以将内存块的属性修改为Executable。

2

实现地址跳转，直接转向不受DEP保护的区域里保存的shellcode执行。

3

调用相关API将shellcode写入不受DEP保护的可执行内存。进而，配
合基于ROP编写的地址跳转指令，完成漏洞利用。

### lecture07part1-漏洞利用防护.pdf / Page 56

单选题

1分

执行右侧命令后，
EDX寄存器值为

A

0

B

1

C

2

D

3

ESP -> ???????? => POP EAX # RETN
0x00000001
???????? => ADD EAX, 2 # RETN
???????? => XCHG EAX,EDX # RETN

提交

### lecture07part1-漏洞利用防护.pdf / Page 57

知识点七：地址定位技术

### lecture07part1-漏洞利用防护.pdf / Page 58

根据软件漏洞触发条件的不同，内存给调用函数分配内存的
方式不同，shellcode的植入地址也不相同。下面根据
shellcode代码不同的定位方式，介绍三种漏洞利用技术。

### lecture07part1-漏洞利用防护.pdf / Page 59

1

静态shellcode地址的利用技术
如果存在溢出漏洞的程序，是一个操作系统每次启动都要加载的程序，操
作系统启动时为其分配的内存地址一般是固定的，则函数调用时分配的栈
帧地址也是固定的。

这种情况下，溢出后写入栈帧的shellcode代码其内存地址也是静态不变的，所
以可以直接将shellcode代码在栈帧中的静态地址覆盖原有返回地址。在函数返
回时，通过新的返回地址指向shellcode代码地址，从而执行shellcode代码。

### lecture07part1-漏洞利用防护.pdf / Page 60

在shellcode为静态地址时，缓冲区溢出前后内存中栈帧的变化示意图参见下图。
esp

esp

ebp

局部变量n

Shellcode

……

Shellcode

局部变量1

Shellcode

前一个栈帧指针

ebp

低

Shellcode

返回地址

覆盖后的返回地址

实参1

实参1

……

……

实参n

实参n

……

……

高
溢出前

溢出后

### lecture07part1-漏洞利用防护.pdf / Page 61

2

基于跳板指令的地址定位技术

有些软件的漏洞存在于某些动态链接库中，它们在进程运行时被动态加载，
因而在下一次被重新装载到内存中时，其在内存中的栈帧地址是动态变化的，
则植入的shellcode代码在内存中的起始地址也是变化的。此外，如果在使用
ASLR技术的操作系统中，地址会因为引入的随机数每次发生变化。

此时，需要让覆盖返回地址后新写入的返回地址能够自动定位到shellcode的起始地址。

### lecture07part1-漏洞利用防护.pdf / Page 62

为了解决这个问题，可以利用esp寄存器的特性实现：
p 在函数调用结束后，被调用函数的栈帧被释放，esp寄存器中的栈顶指针
指向返回地址在内存高地址方向的相邻位置。
p 可见，通过esp寄存器，可以准确定位返回地址所在的位置。
利用这种特性，可以实现对shellcode的动态定位，具体步骤如下：
第一步，找到内存中任意一个汇编指令jmp esp，这条指令执行后可跳转到
esp寄存器保存的地址，下面准备在溢出后将这条指令的地址覆盖返回地址。

### lecture07part1-漏洞利用防护.pdf / Page 63

第二步，设计好缓冲区溢出漏洞利用程序中的输入数据，使缓冲区溢出
后，前面的填充内容为任意数据，紧接着覆盖返回地址的是jmp esp指令
的地址，再接着覆盖与返回地址相邻的高地址位置并写入shellcode代码。
第三步，函数调用完成后函数返回，根据返回地址中指向的jmp esp指令
的地址去执行jmp esp操作，即跳转到esp寄存器中保存的地址，而函数返
回后esp中保存的地址是与返回地址相邻的高地址位置，在这个位置保存
的是shellcode代码，则shellcode代码被执行。

### lecture07part1-漏洞利用防护.pdf / Page 64

上述方法使用jmp esp指令做为跳板，实现了在栈帧动态分配
的情况下，可以自动跳回shellcode的地址并执行。

对于查找jmp esp的指令地址，可以在系统常用的user32.dll等动态链接库，
或者其他被所有程序都加载的模块中查找，这些动态链接库或者模块加载
的基地址始终是固定的。
虽然采用了ASLR技术，高版本windows系统有很多并没有受到ASLR保护
的动态链接库或者系统函数，可以用来查找固定不变的jmp esp等指令。

### lecture07part1-漏洞利用防护.pdf / Page 65

以jmp esp做为跳板定位shellcode的内存地址示意图见下图。
……
局部变量n

NOP

……

NOP

局部变量1

NOP

前一个栈帧指针

NOP

返回地址

覆盖后的返回地址

实参1

Shellcode

……

Shellcode

实参n

Shellcode

……

Shellcode

溢出前

溢出后

……
jmp esp
……

esp

除了jmp esp之外，mov eax,esp和jmp eax等指令序列也可以实现进入栈区的功能。

### lecture07part1-漏洞利用防护.pdf / Page 66

#include <stdio.h>
#include <windows.h>
#define DLL_NAME "user32.dll" //此处定义需要查找的dll名字
int main()
{
BYTE *ptr;
int position,address;
HINSTANCE handle;
BOOL done_flag = FALSE;
handle = LoadLibraryA(DLL_NAME); //LoadLibraryA 是调用dll的
函数名
if(!handle) //若没找到则进入该if
{
printf(" load dll error!");
getchar();
return 0;
}
ptr = (BYTE*)handle;
printf("start at 0x%x\n",handle);

for(position = 0 ; !done_flag ; position++)
{
__try
{
if(ptr[position] == 0xFF && ptr[position+1] == 0xE4)
//jmp esp 的机器码为 E4FF
{
address = (int)ptr + position;
printf("jmp esp found at 0x%x\n",address);
}
}
__except(2)
{
address = (int)ptr + position;
printf("END of 0x%x\n",address);
done_flag = TRUE;
}
}
getchar();
return 0;
}

通过上述程序运行就可以得到很多jmp esp的指令地址—XP下有效（没有ASLR）

### lecture07part1-漏洞利用防护.pdf / Page 67

3

内存喷洒技术

有些特殊的软件漏洞，不支持或者不能实现精确定位shellcode。同时，
存在漏洞的软件其加载地址动态变化，采用shellcode的静态地址覆盖
方法难以实施。由于堆分配地址随机性较大，为了解决shellcode在堆
中的定位以便触发，可以采用heap spray的方法。
内存喷射技术的代表是堆喷洒Heap spray，也称为堆喷洒技术，是在shellcode
的前面加上大量的滑板指令（slide code），组成一个非常长的注入代码段。然
后向系统申请大量内存，并且反复用这个注入代码段来填充。这样就使得内存空
间被大量的注入代码所占据。攻击者再结合漏洞利用技术，只要使程序跳转到堆
中被填充了注入代码的任何一个地址，程序指令就会顺着滑板指令最终执行到
shellcode代码。

### lecture07part1-漏洞利用防护.pdf / Page 68

滑板指令

滑板指令（slide code）是由大量NOP(no-operation)空指令0x90填充组成
的指令序列，当遇到这些NOP指令时，CPU指令指针会一个指令接一个指
令的执行下去，中间不做任何具体操作，直到“滑”过最后一个滑板指令
后，接着执行这些指令后面的其他指令，往往后面接着的是shellcode代码。

随着一些新的攻击技术的出现，滑板指令除了利用NOP指令填充外，也逐
渐开始使用更多的类NOP指令，譬如0x0C，0x0D（回车、换行）等。

### lecture07part1-漏洞利用防护.pdf / Page 69

Heap Spray技术通过使用类NOP指令来进行覆盖，对shellcode地址的跳转准确
性要求不高了，从而增加了缓冲区溢出攻击的成功率。然而，Heap Spray会导致
被攻击进程的内存占用非常大，计算机无法正常运转，因而容易被察觉。
它一般配合堆栈溢出攻击，不能用于主动攻击，也不能保证成功。

针对Heap Spray，对于windows系统比较好的系统防范办法是开启DEP功能，即使被绕过，被
利用的概率也会大大降低。

### lecture07part1-漏洞利用防护.pdf / Page 70

单选题

1分

下列不可以作为跳板的指令是

A

Jmp esp

B

Mov eax, esp; jmp eax

C

Mov eax, ebp; jmp ebp

提交

### lecture07part1-漏洞利用防护.pdf / Page 71

知识点八：API函数自搜索技术

### lecture07part1-漏洞利用防护.pdf / Page 72

API函数自搜索技术

前面的Shellcode都采用硬编址的方式来调用相应API函数。首先，获取所
要使用函数的地址，然后将该地址写入ShellCode，从而实现调用。如果系
统版本变了，很多函数的地址往往会发生变化，那么调用肯定就会失败了。
编写通用shellcode，shellcode自身就必须具备动态的自动搜索所需API函数
地址的能力，即API函数自搜索技术。
以 MessageBoxA 函 数 的 调 用 的 shellcode 为 例 ， 来 解 释 通 用 型 shellcode 的 编 写 逻 辑 。
MessageBoxA位于user32.dll中，用于弹出消息框。
LoadLibraryA位于kernel32.dll中，用于加载user32.dll。

### lecture07part1-漏洞利用防护.pdf / Page 73

通用型Shellcode的编写逻辑

调用MessageBoxA函数，应该先使用LoadLibrary(“user32.dll”) 装载
user32.dll。定位LoadLibrary函数的步骤如下：
Ø 第一步：定位kernel32.dll。
Ø 第二步：解析kernel32.dll的导出表
Ø 第三步：搜索定位LoadLibrary等目标函数。
Ø 第四步：基于找到的函数地址，完成Shellcode的编写。
难点在于第一步到第三步，即如何实现API函数自搜索。
所有的Win32程序都会自动加载ntdll.dll以及kernel32.dll这两个最基础的动态链接
库，接下来，我们看看怎么完成对kernel32.dll里的API的搜索。

### lecture07part1-漏洞利用防护.pdf / Page 74

（1）定位kernel32.dll
[1] 首先通过段选择字FS在内存中找到当前的线程环境块TEB。
[2] 线程环境块偏移地址为0x30的地址存放着指向进程环境块PEB的指针。
[3] 进程环境块中偏移地址为0x0c的地方存放着指向PEB_LDR_DATA结构体的
指针，其中，存放着已经被进程装载的动态链接库的信息。
[4] PEB_LDR_DATA结构体偏移位置为0x1C的地址存放着指向模块初始化链表
的头指针 InInitializationOrderModuleList。
[5] 模块初始化链表InInitializationOrderModuleList中按顺序存放着PE装入运行时
初始化模块的信息，第一个链表结点是ntdll.dll，第二个链表结点就是kernel32。
[6] 找到属于kernel32.dll的结点后，在其基础上再偏移0x08就是kernel32.dll在内
存中的加载基地址。

### lecture07part1-漏洞利用防护.pdf / Page 75

（1）定位kernel32.dll
上述过程细节读者暂时不需要去深究，如上复杂的操作可以用如下简单的代码来实现：

int main()
{
_asm
{
mov eax, fs:[0x30] ;PEB的地址
mov eax, [eax + 0x0c] ; PEB_LDR_DATA结构体的地址
mov esi, [eax + 0x1c] ; InInitializationOrderModuleList地址
lodsd
mov eax, [eax + 0x08] ;eax就是kernel32.dll的地址
}
return 0;
}

### lecture07part1-漏洞利用防护.pdf / Page 76

（2）定位kernel32.dll的导出表
找到了kernel32.dll，由于它也是属于PE文件，那么我们可以根据PE文件的结构
特征，定位其导出表，进而定位导出函数列表信息，然后进行解析、遍历搜索，
找到我们所需要的API函数。
定位导出表及函数列表的步骤如下：

[7] 从kernel32.dll加载基址算起，偏移0x3c的地方就是其PE头的指针。
[8] PE头偏移0x78的地方存放着指向函数导出表的指针。
[9] 获得导出函数偏移地址（RVA）列表、导出函数名列表：
• 导出表偏移0x1c处的指针指向存储导出函数偏移地址（RVA）的列表。
• 导出表偏移0x20处的指针指向存储导出函数函数名的列表。

### lecture07part1-漏洞利用防护.pdf / Page 77

（2）定位kernel32.dll的导出表
定位导出表及函数列表，可以用如下简单的代码来实现：

mov ebp, eax
mov

eax,[ebp+0x3C]

mov

ecx,[ebp+eax+0x78]

add

ecx,ebp

mov

ebx,[ecx+0x20]

add

ebx,ebp

//将kernel32.dll基地址赋值给ebp
//dll的PE头的指针（相对地址）
//导出表的指针（相对地址）
// 得到导出表的内存地址
//导出函数名列表指针
//导出函数名列表指针的基地址

### lecture07part1-漏洞利用防护.pdf / Page 78

（3）搜索定位目标函数

可以通过遍历两个函数相关列表，算出所需函数的入口地址：
Ø 函数的RVA地址和名字按照顺序存放在上述两个列表中，可以在名称列
表中定位到所需的函数是第几个，然后在地址列表中找到对应的RVA。
Ø 获得RVA后，再加上前边已经得到的动态链接库的加载地址，就获得了
所需API此刻在内存中的虚拟地址，这个地址就是最终在ShellCode中调
用时需要的地址。
按照这个方法，就可以获得kernel32.dll中的任意函数。

### lecture07part1-漏洞利用防护.pdf / Page 79

1. 定位Kenerl32.dll

2. 定位导出表及函数列表

3. 遍历函数列表、定位函数

### lecture07part1-漏洞利用防护.pdf / Page 80

完整API函数自搜索代码。基于上述流程找到函数的入口地址，
之后就可以编写自己的shellcode，如课本示例5-11，各位同学
还需要自行阅读了解代码的工作原理。

### lecture07part1-漏洞利用防护.pdf / Page 81

完整的通用型Shellcode
基于上面提到的自定义API搜索技术，可以在找到函数入口地址之后，进行利用。
int main(){
__asm {
CLD
//清空标志位DF
push 0x1E380A6A
//压入MessageBoxA的hash-->user32.dll
push 0x4FD18963
//压入ExitProcess的hash-->kernel32.dll
push 0x0C917432
//压入LoadLibraryA的hash-->kernel32.dll
mov esi,esp
//esi=esp,指向堆栈中存放LoadLibraryA的地址
lea edi,[esi-0xc]
//空出8字节应该是为了兼容性 1. ESI保存三个哈希值地址
//======开辟一些栈空间
xor
ebx,ebx
mov
bh,0x04
sub
esp,ebx
//esp-=0x400

//======压入"user32.dll"
mov
bx,0x3233
push ebx
//"\0 32"
push 0x72657375
//"user"
2. 保存user32.dll字符串地址
push esp
xor
edx,edx
//edx=0
//======找kernel32.dll的基地址
mov
ebx,fs:[edx+0x30]
//[TEB+0x30]-->PEB
mov
ecx,[ebx+0xC]
//[PEB+0xC]--->PEB_LDR_DATA
mov
ecx,[ecx+0x1C]
mov
ecx,[ecx]
//进入链表第一个就是ntdll.dll
mov
ebp,[ecx+0x8]
//ebp= kernel32.dll的基地址
3. EBP保存kernel32.dll基地址

### lecture07part1-漏洞利用防护.pdf / Page 82

//======是否找到了自己所需全部的函数
依次取入栈的函数哈希
最后一个是MessageBoxA
find_lib_functions:
lodsd //即mov eax,[esi],esi+=4, 第一次取LoadLibraryA的hash
MessageBoxA？的hash比较
cmp eax,0x1E380A6A
//与MessageBoxA
（最后一个）
jne find_functions //如果没有找到最后一个函数，继续找
xchg
call [edi-0x8] //LoadLibraryA("user32") |
6. 是,将执行Loadlibrary(user32.dll）
xchg eax,ebp
//ebp=userl32.dll基地址,eax=MessageBoxA的hash <-- |
3.不是

//======导出函数名列表指针
find_functions:
pushad
//保护寄存器
mov eax,[ebp+0x3C]
//dll的PE头
mov ecx,[ebp+eax+0x78] //导出表的指针
add ecx,ebp //ecx=导出表的基地址
mov ebx,[ecx+0x20]//导出函数名列表指针
add ebx,ebp //ebx=导出函数名列表指针的基地址
xor edi,edi
//======找下一个函数名
next_function_loop:
inc edi
mov esi,[ebx+edi*4] //从列表数组中读取函数名
add esi,ebp
//esi = 函数名称所在地址
cdq
//edx = 0

//======函数名的hash运算
hash_loop:
movsx eax,byte ptr[esi]
cmp al,ah
//字符串结尾就跳出当前函数
jz
compare_hash
ror edx,7
add edx,eax
inc
esi
jmp hash_loop
//======比较找到的当前函数的hash是否是自己想找的
compare_hash:
4. 如果不是要找的函数，跳
为LoadLibraryA的hash
cmp edx,[esp+0x1C] //栈+1c
转到前面继续判断
jnz
next_function_loop
mov ebx,[ecx+0x24] //ebx = 顺序表的相对偏移量
add ebx,ebp
//顺序表的基地址
mov di,[ebx+2*edi] //匹配函数的序号
mov ebx,[ecx+0x1C] //地址表的相对偏移量
add ebx,ebp
//地址表的基地址
add ebp,[ebx+4*edi] //函数的基地址
xchg eax,ebp
//eax<==>ebp 交换
pop edi
5. 找到了存到EDI寄存器位置，
stosd
//把找到的函数保存到
edi的位置
如果不是最后一个，跳转到前面
push edi
find_lib_functions位置继续
popad
//一次性完成多个寄存器状态保存和恢复
cmp eax,0x1e380a6a //找到函数MessageBox后，跳出循环
jne
find_lib_functions

### lecture07part1-漏洞利用防护.pdf / Page 83

//======让他做些自己想做的事
function_call:
xor
ebx,ebx
push ebx
push 0x74736577
push 0x74736577 //push "westwest"
mov
eax,esp
push ebx
push eax
push eax
push ebx
call [edi-0x04]
//MessageBoxA(NULL,"westwest","westwest",NULL)
push ebx
call [edi-0x08]
//ExitProcess(0);
nop
nop
nop
nop
}
return 0;
}

这个完整的代码，实现了MessageBox弹窗
的功能，而且可以在任意系统里执行。
前面将找到的函数地址保存到了EDI所保存
的地址区域，是按照前面依次入栈的顺序，
即MessageBox、ExitProcess和Loadlibrary的
入口地址。
整个Shellcode用nop指令作为结束。
之后，可以用上述Shellcode提取方式来得
到对应的机器码。

### lecture07part1-漏洞利用防护.pdf / Page 84

知识点十：绕过其它安全防护

### lecture07part1-漏洞利用防护.pdf / Page 85

漏洞又称为脆弱性，本书的⼀个观点就是只要有不健壮的地⽅，就存在被利用的可
能。正所谓道⾼⼀尺、魔⾼⼀丈，接下来，我们简要介绍对于GS安全机制、ASLR
机制、SEH保护机制等安全防护策略的绕过策略。
1

绕过GS安全机制

Visual Studio在实现GS安全机制的时候，除了增加Cookie，还会对栈中变量进
行重新排序，比如：将字符串缓冲区分配在栈帧的最高地址上，因此，当字符串
缓冲区溢出，就不能覆盖本地变量了。
但是，考虑到效率问题，它仅按照函数隐患及危害程度进行选择性保护，因此有
一部分函数可能没有得到有效的保护。比如：结构成员因为互操作性问题而不能
重新排列，因此当它们包含缓冲区时，这个缓冲区溢出就可以将之后其它成员覆
盖和控制。

### lecture07part1-漏洞利用防护.pdf / Page 86

1

绕过GS安全机制

正是因为GS安全机制存在这些缺陷，所以聪明的攻击者构造出了各种办法来绕过
GS保护机制。David Litchfield在2003年提出了一个技术来绕过GS保护机制：如
果Cookie被一个不同的值覆盖了，代码会检查是否安装了安全处理例程，如果没
有，系统的异常处理器就将接管它。
如果黑客覆盖掉了一个异常处理结构，并在Cookie被检查前触发一个异常，这时
栈中虽然仍然存在Cookie，但是还是可以被成功溢出。这个方法相当于是利用
SEH进行漏洞攻击。可以说，GS安全机制最重要的一个缺陷是没有保护异常处理
器，但这点上虽然有SEH保护机制作为后盾，但SEH保护机制也是可以被绕过的。

### lecture07part1-漏洞利用防护.pdf / Page 87

2

ASLR缺陷和绕过方法

ASLR通过增加随机偏移使得攻击变得非常困难。但是，ASLR技术存在很多脆弱性：
（1）为了减少虚拟地址空间的碎片，操作系统把随机加载库文件的地址限制为8位，即
地址空间为256，而且随机化发生在地址前两个最有意义的字节上；
（2）很多应用程序和DLL模块并没有采用/DYNAMICBASE的编译选项；
（3）很多应用程序使用相同的系统DLL文件，这些系统DLL加载后地址就确定下来了，
对于本地攻击，攻击者还是很容易就能获得所需要的地址，然后进行攻击。
针对这些缺陷，还有一些其他绕过方法，比如攻击未开启地址随机化的模块（作为跳板）、
堆喷洒技术、部分返回地址覆盖法等。

### lecture07part1-漏洞利用防护.pdf / Page 88

3

SEH保护机制缺陷和绕过方法

当一个进程中存在一个不是/SafeSEH编译的DLL或者库文件的时候，整个SafeSEH机制
就可能失效。因为/SafeSEH编译选项需要.NET的编译器支持，现在仍有大量第三方库和
程序没有使用该编译器编译或者没有启动/SafeSEH选项。
目前，较为可行的绕过SafeSEH的方法有：
p 利用未开启SafeSEH的模块作为跳板绕过：可以在未启用SafeSEH的模块里找一些跳转
指令，覆盖SEH函数指针，由于这些指令在未启用SafeSEH的模块里，因此异常触发时，
可以执行到这些指令。
p 利用加载模块之外的地址进行绕过：可以利用加载模块之外的地址，包括从堆中进行绕
过或者其他一些特定内存绕过，具体不展开介绍。

## 来源：lecture07part2-漏洞挖掘.pdf

### lecture07part2-漏洞挖掘.pdf / Page 1

漏洞挖掘

### lecture07part2-漏洞挖掘.pdf / Page 2

Where are we?

如何利用漏洞进行攻击

### lecture07part2-漏洞挖掘.pdf / Page 3

Where are we?

如何发现漏洞

### lecture07part2-漏洞挖掘.pdf / Page 4

知识点一：方法概述

### lecture07part2-漏洞挖掘.pdf / Page 5

漏洞挖掘方法分类

静态分析技术
•
•
•
•
•
•
•
•
•

数据流分析
控制流分析
污点分析
符号执行
程序切片
抽象解释
模式匹配
代码特性
机器学习/LLM辅助

### lecture07part2-漏洞挖掘.pdf / Page 6

漏洞挖掘方法分类

静态分析技术
•
•
•
•
•
•
•
•
•

数据流分析
控制流分析
污点分析
符号执行
程序切片
抽象解释
模式匹配
代码特性
机器学习/LLM辅助

动态分析技术
•
•
•
•
•
•
•

模糊测试
动态污点分析
Concolic execution
运行时监控+sanitizer
DAST/IAST
沙箱执行
动态exploit验证

### lecture07part2-漏洞挖掘.pdf / Page 7

漏洞挖掘方法分类

静态分析技术
•
•
•
•
•
•
•
•
•

数据流分析
控制流分析
污点分析
符号执行
程序切片
抽象解释
模式匹配
代码特性
机器学习/LLM辅助

动态分析技术
•
•
•
•
•
•
•

模糊测试
动态污点分析
Concolic execution
运行时监控+sanitizer
DAST/IAST
沙箱执行
动态exploit验证

混合分析技术
•
•
•
•

模糊测试 + 符号执行
静态 + 动态污点分析
LLM + 程序分析
二进制 + 运行时轨迹分析

### lecture07part2-漏洞挖掘.pdf / Page 8

静态分析 vs. 动态分析

静态分析技术

无需运行程序
效率高、消耗低
召回率高
精度低
（误报率高）

### lecture07part2-漏洞挖掘.pdf / Page 9

静态分析 vs. 动态分析

静态分析技术

动态分析技术

无需运行程序

需要运行程序

效率高、消耗低

开销大、检测时间长

召回率高

召回率低

精度低

精度高

（误报率高）

### lecture07part2-漏洞挖掘.pdf / Page 10

漏洞挖掘方法分类

静态分析技术
•
•
•
•
•
•
•
•
•

数据流分析
控制流分析
污点分析
符号执行
程序切片
抽象解释
模式匹配
代码特性
机器学习/LLM辅助

动态分析技术
•
•
•
•
•
•
•

模糊测试
动态污点分析
Concolic execution
运行时监控+sanitizer
DAST/IAST
沙箱执行
动态exploit验证

混合分析技术
•
•
•
•

模糊测试 + 符号执行
静态 + 动态污点分析
LLM + 程序分析
二进制 + 运行时轨迹分析

### lecture07part2-漏洞挖掘.pdf / Page 11

符号执行

用符号变量代替具体输入
沿程序路径推导约束条件
通过约束求解器生成能够触发目标路径的具体输入

### lecture07part2-漏洞挖掘.pdf / Page 12

符号执行

做什么？
不只告诉你：这里可能有漏洞
什么输入可以打到这里

### lecture07part2-漏洞挖掘.pdf / Page 13

符号执行

做什么？
不只告诉你：这里可能有漏洞
什么输入可以打到这里
适合利用分析（exploitability analysis）

### lecture07part2-漏洞挖掘.pdf / Page 14

2. 符号执行

符号执行（Symbolic Execution）的基本思路是使用符号值替代具体
值，模拟程序的执行 。在模拟程序运行的过程中，符号执行引擎会收

集程序中的语义信息，探索程序中的可达路径、分析程序中隐藏的错
误。
动态符号执行结合了真实执行和传统符号执行技术的优点，在真实执
行的过程中同时进行符号执行，可以在保证测试精度的前提下提升了
执行效率。

### lecture07part2-漏洞挖掘.pdf / Page 15

符号执行基本原理
符号执行三个关键点是变量符号化、程序执⾏模拟和约束求解。
变量符号化是指用一个符号值表示程序中的变量，所有与被符号化的变量相关的变
量取值都会用符号值或符号值的表达式表示。
程序执行模拟最重要的是运算语句和分支语句的模拟：
p 对于运算语句，由于符号执行使用符号值替代具体值，所以无法直接计算得到一
个明确的结果，需要使用符号表达式的方式表示变量的值。
p 对于分支语句，每当遇到分支语句，原先的一条路径就会分裂成多条路径，符号
执行会记录每条分支路径的约束条件。最终，通过采用合适的路径遍历方法，符
号执行可以收集到所有执行路径的约束条件表达式。

### lecture07part2-漏洞挖掘.pdf / Page 16

符号执行基本原理
约束求解主要负责路径可达性进⾏判定及测试输⼊⽣成的工作。对一条路径的约束
表达式，可以采用约束求解器进行求解：
p 如有解，该路径是可达的，可以得到到达该路径的输入；
p 如无解，该路径是不可达的，也无法生成到达该路径的输入。
符号执行有代价小、效率⾼的优点，然而由于程序执行的可能路径随着程序规模的
增大呈指数级增长，从而导致符号执行技术在分析输入和输出之间关系时，存在一
个路径状态空间的路径爆炸问题。由于符号执行技术进行路径敏感的遍历式检测，
当程序执行路径的数量超过约束求解工具的求解能力时，符号执行技术将难以分析。

### lecture07part2-漏洞挖掘.pdf / Page 17

符号执行的应用
符号执行已经广泛应用在软件测试、漏洞挖掘和软件破解等。
p 在软件测试中，符号执行可以获得程序执行路径的集合、路径的约束条件和输
出的符号表达式，可以使用约束求解器求解出满足约束条件的各个路径的输入
值，用于创建高覆盖率的测试用例。符号执行与模糊测试的结合也是当前流行
的软件测试技术。
p 在漏洞挖掘中，通过符号执行技术可以获得漏洞监测点的变量符号表达式，结
合路径约束条件、变量符号表达式和漏洞分析规则，可以通过约束求解的方法
来求解是否存在满足或违反漏洞分析规则的值。
p 符号执行还可以用于搜索特定目标代码的到达路径，进而计算该路径的输入，
用在面向特定任务（比如代码破解）的程序分析中。

### lecture07part2-漏洞挖掘.pdf / Page 18

举例（漏洞挖掘-检测是否数组越界）：
void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)
i = i % 10;
a[i] = 1;
}
}

数组越界？

### lecture07part2-漏洞挖掘.pdf / Page 19

举例（漏洞挖掘-检测是否数组越界）：
void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)

为输入引入符号

i = i % 10;
a[i] = 1;
}

条件约束

}

true

i
i0

### lecture07part2-漏洞挖掘.pdf / Page 20

举例（漏洞挖掘-检测是否数组越界）：
void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)
i = i % 10;
a[i] = 1;
}

条件约束

}

i0 > 0

i
i0

### lecture07part2-漏洞挖掘.pdf / Page 21

举例（漏洞挖掘-检测是否数组越界）：
void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)
i = i % 10;
a[i] = 1;
}
}

条件约束
i0 > 0 and i0 > 10

i
i0

### lecture07part2-漏洞挖掘.pdf / Page 22

举例（漏洞挖掘-检测是否数组越界）：
void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)

1

i = i % 10;

2

a[i] = 1;

2条路径

}
}

1

条件约束
i0 > 0 and i0 > 10 and i0%10 >= 10
路径约束

漏洞约束

i
i0

### lecture07part2-漏洞挖掘.pdf / Page 23

举例（漏洞挖掘-检测是否数组越界）：
void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)

1

i = i % 10;

2

a[i] = 1;

2条路径

}
}

1
2

条件约束
i0 > 0 and i0 > 10 and i0%10 >= 10
I0 > 0 and i0 <= 10 and i0 >= 10

i
i0
i0

### lecture07part2-漏洞挖掘.pdf / Page 24

举例（漏洞挖掘-检测是否数组越界）：

约束求解器

void testme(int i) {
int a[10];
if (i > 0) {
if (i > 10)

1

i = i % 10;

2

a[i] = 1;

2条路径

}
}

1
2

条件约束
i0 > 0 and i0 > 10 and i0%10 >= 10
I0 > 0 and i0 <= 10 and i0 >= 10

i
i0
i0

i = 10

### lecture07part2-漏洞挖掘.pdf / Page 25

举例（漏洞挖掘-检测是否数组越界）：
int a[10];
scanf("%d", &i);
if (i > 0) {
if (i > 10)
i = i % 10;
a[i] = 1;
}

p 在a[i]=1语句处存在可能数组越界的情况
访问越界的约束条件是x>=10
p 整段代码存在两个if分支，经过符号执行，知道到达a[i]=1
语句处有2条路径：
① 路径约束条件为x>0∧x<=10，此时变量i的符号表达式为x；
② 路径约束条件为x>0∧x>10，此时变量i的符号表达式为x%10。

p 得到两个判定条件：
① (x>0∧x<=10)∧(x>=10) --有解 x=10 满足越界条件 存在漏洞
② (x>0∧x>10)∧x%10>=10)

### lecture07part2-漏洞挖掘.pdf / Page 26

局限

•
•
•
•
•

路径爆炸
约束求解开销
环境建模困难
指针分析
多线程与并发爆炸

### lecture07part2-漏洞挖掘.pdf / Page 27

研究现状

•
•
•
•

结合动态执行
融合模糊测试
目标化符号执行
LLM + 符号执行

### lecture07part2-漏洞挖掘.pdf / Page 28

研究现状
真实执行快速推进状态
符号执行求解关键分支

•
•
•
•

结合动态执行
融合模糊测试
目标化符号执行
LLM + 符号执行

模糊测试跑不进的深路径
用符号执行解出

只对高风险代码区域运行

LLM缩小搜索空间

### lecture07part2-漏洞挖掘.pdf / Page 29

污点分析

把不可信输入标记为污点（tainted）
持续跟踪它在程序中的传播过程
检查是否到达危险操作点（sink）

### lecture07part2-漏洞挖掘.pdf / Page 30

污点分析

何时使用？
• 输入驱动型漏洞检测
• RCE/利用链分析
• 大规模代码库审计
• 隐私数据泄漏

### lecture07part2-漏洞挖掘.pdf / Page 31

污点分析

何时使用？
• 输入驱动型漏洞检测
• RCE/利用链分析
• 大规模代码库审计
• 隐私数据泄漏

•
•
•
•
•
•

SQL注入
XSS
命令注入
SSRF
路径穿越
反序列化漏洞

### lecture07part2-漏洞挖掘.pdf / Page 32

污点分析

### lecture07part2-漏洞挖掘.pdf / Page 33

污点分析

1

用户输入，标记

2

taint(name)=true

3

污点继续传播
taint(sql)=true

传播至危险sink
报告

### lecture07part2-漏洞挖掘.pdf / Page 34

局限

•
•
•
•
•

误报高
动态调用解析困难
跨函数跨库传播分析困难
别名/指针问题
不擅长发现业务逻辑漏洞

### lecture07part2-漏洞挖掘.pdf / Page 35

局限

•
•
•
•
•

误报高
动态调用解析困难
跨函数跨库传播分析困难
别名/指针问题
不擅长发现业务逻辑漏洞

过滤函数不应被标记
但可能缺乏对sanitize建模

• npm ecosystem
• reflection
• eval

例如下面的调用链

### lecture07part2-漏洞挖掘.pdf / Page 36

研究现状

•
•
•
•
•

路径敏感污点分析
静动态混合
与模糊测试结合
LLM + 污点分析
结果排序

### lecture07part2-漏洞挖掘.pdf / Page 37

研究现状
结合路径条件判断是否真实可达
（path-sensitive taint analysis）

•
•
•
•
•

静态分析 + 动态验证
路径敏感污点分析
静动态混合
与模糊测试结合
LLM + 污点分析 污点分析指出哪些输入字段
最影响sink；提升fuzzing效率
结果排序

LLM推断
source/sink/sanitizier

结果很多，如何排序？

### lecture07part2-漏洞挖掘.pdf / Page 38

污点分析
污点分析（Taint Analysis）通过标记程序中的数据（外部输⼊数据或
者内部数据）为污点，跟踪程序处理污点数据的内部流程，进⽽帮助
⼈们进⾏深⼊的程序分析和理解。
污点分析可以分为污点传播分析（静态分析）和动态污点分析（动态
分析）。静态污点分析技术在检测时并不真正运行程序，而是通过模
拟程序的执行过程来传播污点标记，而动态污点分析技术需要运行程
序，同时实时传播并检测污点标记。

### lecture07part2-漏洞挖掘.pdf / Page 39

基本思想
首先，确定污点源，即污点分析的目标来源。通常来讲，污点源表示了程序外部数据
或者用户所关心的程序内部数据，是需要进行标记分析的输入数据。
然后，标记和分析污点。对污点源在内存中进行标记、计算涉及到污点的执行过程。

!"#$%&'()*+,-()./01).*2'34##############
5"#6%&'()*+,-()./01)+'(70/834#####
!!
9"#:%$;<=##############################################
>"#?%:@6;<=###########################################
A"#:%<###################################################
!!
B"#&0(0#?##############################################

!$% '(
5$%

+,!$-. 0
+,5$-. 0
+,9$
0
!$56

89

第六行，验证程序的执行流程将被外部数据任意控制，发生了典型的“控制流劫持”。

### lecture07part2-漏洞挖掘.pdf / Page 40

污点分析核心要素
以上实例介绍了污点分析方法的基本原理，可以看出有如下三个核心要素：
p 污点源：是污点分析的目标来源（Source点），通常表示来自程序外部的不可
信数据，包括硬盘文件内容、网络数据包等。
p 传播规则：是污点分析的计算依据，通常包括污点扩散规则和清除规则，其中
普通赋值语句、计算语句可使用扩散规则，而常值赋值语句则需要利用清楚规
则进行计算。
p 污点检测：是污点分析的功能体现，其通常在程序执行过程中的敏感位置
（Sink点）进行污点判定，而敏感位置主要包括程序跳转以及系统函数调用等。

### lecture07part2-漏洞挖掘.pdf / Page 41

优缺点

污点分析的核心是分析输入参数和执行路径之间的关系，它适用于由输
入参数引发漏洞的检测，比如SQL注入漏洞等。

污点分析技术具有较高的分析准确率，然而针对大规模代码的分析，由
于路径数量较多，因此其分析的性能会受到较大的影响。

### lecture07part2-漏洞挖掘.pdf / Page 42

单选题

1分

模拟程序运行的过程中，收集程序中的语义信息，探索程序中的可达路径、
分析程序中隐藏的错误的方法是

A

模糊测试

B

词法分析

C

污点分析

D

符号执行
提交

### lecture07part2-漏洞挖掘.pdf / Page 43

知识点二：词法分析

### lecture07part2-漏洞挖掘.pdf / Page 44

1. 基本概念

词法分析通过对代码进行基于文本或字符标识的匹配分析对比，以
查找符合特定特征和词法规则的危险函数、API或简单语句组合。
主要思想是将代码文本与归纳好的缺陷模式（比如边界条
件检查）进行匹配，以此发现漏洞。
优点：算法简单，检测性能较高
缺点：只能进行表面的词法检测，不能进行语义方面的深
层次分析，因此可以检测的安全缺陷和漏洞较少，会出现
较高的漏报和误报，尤其对于高危漏洞无法进行有效检测。

### lecture07part2-漏洞挖掘.pdf / Page 45

2. 漏洞挖掘实践
实践1

基于词法分析和逆向分析的可执行代码静态检测

核心思想：根据二进制可执行文件的格式特征，从二进制文件的头部、符号表以及调
试信息中提取安全敏感信息（识别危险函数），来分析文件中是否存在安全缺陷。
第一步

找到敏感函数，比如memcpy、strcpy等

第二步

回溯函数的参数

第三步

判断栈与操作参数的大小关系，以定位是否发生了溢出漏洞

### lecture07part2-漏洞挖掘.pdf / Page 46

实验一：基于IDA Pro分析给定的可执行文件是否存在溢出漏洞
对于findoverflow.exe，是通过vc6代码生成的Release版本：
#include <stdio.h>
#include <string.h>
void makeoverflow(char *b){
char des[5];
strcpy(des,b);
}
void main(int argc,char *argv[]){
if(argc>1)

{

if(strstr(argv[1],"overflow")!=0)
makeoverflow(argv[1]);
} else
printf("usage: findoverflow XXXXX\n");
}

### lecture07part2-漏洞挖掘.pdf / Page 47

第一步：使用IDA打开所生成的exe文件

通过该视图，可见，主要有一个main函数，在该函数中可能有跳转，
调用了sub_401000函数、_strstr函数和_printf函数。此外，还定义了两
个字符串常量，aUsageFindoverf，在其上点右键->Text view，可以看到：

### lecture07part2-漏洞挖掘.pdf / Page 48

打开main函数汇编代码如下：

[ ]的作用是什么？
是什么方式寻址？

为何要加4？之前为什么要将esi入栈？

ESI已经是argV的值，指针+4，相当于argV[1]的地址

### lecture07part2-漏洞挖掘.pdf / Page 49

注意

通常在IDA的反汇编中，arg_x表示函数参数x的位置，var_8
表示局部变量的位置；[]是内存寻址，[x+arg_x]通常表达的
就是arg_x的地址值。
由release和debug生成的汇编代码是截然不同的，release版本
非常简洁，执行效率优先，debug版本则基本严格按照语法结
构，而且增加了很多方便调试的附加信息。

### lecture07part2-漏洞挖掘.pdf / Page 50

第二步：定位敏感函数
在主函数中，Printf函数无任何格式化参数存在。因此，存在
敏感函数的可能在于sub_401000函数中，打开该函数的代码
如左图：

p 一个输入参数arg_0，一个局部变量var_8。
p 通过“lea edx, [esp+8+var_8]”和“mov edi,
edx”可知，向目标寄存器存储了目标字符串
的地址，为局部变量var_8；
p 通过“mov edi,[esp+10h+arg_0]”以及后面的
“mov esi, edi”，可知，将函数的输入参数
作为源字符串。
p 那么到底是否发生了溢出呢？

### lecture07part2-漏洞挖掘.pdf / Page 51

通过“sub esp 8”可以知道栈大小为8，因此，函数的局部变量var_8的大小最
大就是8。这样的话，可以得到sub_401000函数的代码结构大致如下：

如果输入的字符串的长度大于8，就可能发生溢出了——需要验证。
为什么是8，二不是源代码里的5？

### lecture07part2-漏洞挖掘.pdf / Page 52

打开DOS对话框，运行示例程序，如果不给任何参数的话，会提示：usage:
findoverflow XXXXX
如果输入参数，比如：findoverflow ssssssssss。却可以运行成功。
这是为什么呢？回顾逆向的反汇编代码，可以知道：

### lecture07part2-漏洞挖掘.pdf / Page 53

由于程序需要先判断是否包含子串overflow，因此，需要构造的输入需要满足这个条件。

### lecture07part2-漏洞挖掘.pdf / Page 54

实验二：使用Bugscam脚本来替代手工过程完成漏洞挖掘
Bugscam是一个IDA工具的idc脚本的轻量级的漏
洞分析工具，通过检测栈溢出漏洞的诸如
strcpy,sprintf危险函数的位置，然后根据这些函
数的参数，确定是否有缓冲区漏洞。下载网址：
https://sourceforge.net/projects/bugscam/
1、将Bugscam文件解压放到任意地方，然后修
改globalvar.idc文件中头行的bugscam_dir为你的
bugscam目录的全路径（路径不能含有中文）。

#include <stdio.h>
#include <windows.h>
void vul(char*bu1){
char a[200];
lstrcpy(a,bu1);
printf("%s",a);
return; }
void main(){
char b[1024];
memset(b,'l',sizeof(b));
vul(b); }

### lecture07part2-漏洞挖掘.pdf / Page 55

实验二：使用Bugscam脚本来替代手工过程完成漏洞挖掘
2、启动ida，加载任意一个x86程序文件（本例为idc.exe），然后打开脚本文件run_analysis.idc，
运行即可，等待分析完毕，最后的分析报告结果保存在reports目录中的html文件中。

其中，Severity是威胁等级，越高说明漏洞危险级别越高。本例的程序中，lstrcpyA函数存在溢出漏洞，
地址401010处的代码可能将向目标203字节的区域写入1024字节的数据。

### lecture07part2-漏洞挖掘.pdf / Page 56

知识点三：数据流分析

### lecture07part2-漏洞挖掘.pdf / Page 57

1. 基本概念

数据流分析是⼀种用来获取相关数据沿着程序执⾏路径流动的信息
分析技术，分析对象是程序执⾏路径上的数据流动或可能的取值。

按照分析程序路径的深度，将数据流分析分为过程内分析和过程间分析

### lecture07part2-漏洞挖掘.pdf / Page 58

数据流分析方法分类

p过程内分析只针对程序中函数内的代码进行分析，又分为：
ü 流不敏感分析（flow insensitive）：按代码行号从上而下进行分析；
ü 流敏感分析（flow sensitive）：首先产生程序控制流图（Control Flow Graph，
CFG），再按照CFG的拓扑排序正向或逆向分析；
ü 路径敏感分析（path sensitive）：不仅考虑到语句先后顺序，还会考虑语句可达性，
即会沿实际可执行到路径进行分析。

p过程间分析则考虑函数之间的数据流，即需要跟踪分析目标数据在函数
之间的传递过程。
ü 上下文不敏感分析：忽略调用位置和函数参数取值等函数调用的相关信息。
ü 上下文敏感分析：对不同调用位置调用的同一函数加以区分。

### lecture07part2-漏洞挖掘.pdf / Page 59

程序代码模型
数据流分析使用的程序代码模型主要包括程序代码的中间表示以及一些关键的数据结构，
利用程序代码的中间表示可以对程序语句的指令语义进行分析。
抽象语法树。是程序抽象语法结构的树状表现
形式，其每个内部节点代表一个运算符，该节

!""#$%&

点的子节点代表这个运算符的运算分量。通过
描述控制转移语句的语法结构，抽象语法树在
一定程度上也描述了程序的过程内代码的控制
流结构。
举例，对于表达式“1+3*(4-1)+2”，其抽象语法树为：

!""#$%&
(

'
)*+#$,&

-

.*/#$0&
1

(

### lecture07part2-漏洞挖掘.pdf / Page 60

程序代码模型
三地址码。三地址码（Three address code，TAC）是一种中间语言，由一组类似于汇编语言的指令组成，
每个指令具有不多于三个的运算分量。每个运算分量都像是一个寄存器。
通常的三地址码指令包括下面几种：
ü x = y op z ：表示 y 和 z 经过 op 指示的计算将结果存⼊ x
ü x = op y ：表示 y 经过操作 op 的计算将结果存⼊ x

for (i = 0; i < 10; ++i) {
b[i] = i*i;
}

ü x = y ：表示赋值操作
ü goto L ：表示⽆条件跳转
ü if x goto L ：表示条件跳转

t1 := 0

; initialize i

L1: if t1 >= 10 goto L2 ; conditional jump
t2 := t1 * t1
; square of i

ü x = y[i] ：表示数组赋值操作
ü x = &y 、 x = *y ：表示对地址的操作
ü param x1, param x2, call p：表示过程调用 p(x1, x2)
L2:

t3 := t1 * 4
t4 := b + t3

; word-align address
; address to store i*i

*t4 := t2
t1 := t1 + 1

; store through pointer
; increase i

goto L1

; repeat loop

### lecture07part2-漏洞挖掘.pdf / Page 61

程序代码模型
控制流图。控制流图（Control Flow Graph，CFG）通常是指用于描述程序过程内的
控制流的有向图。控制流由节点和有向边组成。节点可以是单条语句或程序代码段。有
向边表示节点之间存在潜在的控制流路径。
(a)有一个if-then-else语句；(b)有一个while循环;(c)
有两个出口的自然环路;(d)有两个入口的循环。

调用图。调用图（Call Graph，CG）是描述程序中过程之间的调用和被调用关系的有
向图，满足如下原则：对程序中的每个过程都有一个节点；对每个调用点都有一个节点；
如果调用点c调用了过程p，就存在一条从c的节点到p的节点的边。

### lecture07part2-漏洞挖掘.pdf / Page 62

2. 基于数据流的漏洞分析流程
基于数据流的漏洞分析技术是通过分析软件代码中变量的取值变化和语句的执⾏情
况，来分析数据处理逻辑和程序的控制流关系，从⽽分析软件代码的潜在安全缺陷。

基于数据流的漏洞分析的一般流程为：
p 首先，进行代码建模，将代码构造为抽象语法树或程序控制流图；
p 然后，追踪获取变量的变化信息，根据漏洞分析规则检测安全缺陷和漏洞。
基于数据流的漏洞分析非常适合检查因控制流信息非法操作而导致的安全问题，如
内存访问越界、常数传播等。由于对于逻辑复杂的软件代码，其数据流复杂，并呈
现多样性的特点，因而检测的准确率较低，误报率较高。

### lecture07part2-漏洞挖掘.pdf / Page 63

示例一：检测指针变量的错误使用
int contrived(int *p, int *w, int x) {
int *q;
if (x) {
kfree(w); // w free
q = p;
}else
q=w;
return *q; // p use after free
}
int contrived_caller(int *w, int x, int *p) {
kfree(p); // p free
[...]
int r = contrived(p, w, x);
[...]
return *w; // w use after free
}

在检测指针变量的错误使用时，我们关心
的是变量的状态。左侧代码可能出现
use-after-free漏洞。
漏洞分析规则。下面是用于检测指针变量
错误使用的检测规则：
v 被分配空间 ==> v.start
v.start: {kfree(v)} ==> v.free
v.free: {*v} ==> v.useAfterFree
v.free: {kfree(v)} ==> v.doubleFree

### lecture07part2-漏洞挖掘.pdf / Page 64

示例一：检测指针变量的错误使用
代码建模。这里我们采用路径敏感的数据流分析，控制流图如下

!

"#$#%&
'()("

*

+,$--."/

p.free

!

"#$#%&
"('()

*

534&:@

);<=
+,$--.'/
p.free, w.free

0

0

1234$56-7."(&'(&)/

8

$-49$3&:'

@&<&"

p.useAfterfree, w.free

p.free, w.free, q.free

!"#$%&'()*!+,,(%

)<=
>

8
?

!"#$%&'()

$-49$3&:@

@<'

### lecture07part2-漏洞挖掘.pdf / Page 65

示例一：检测指针变量的错误使用
漏洞分析。分析过程从函数contrived_caller的入口点开始，可知调用函数contrived的
时候p的状态为p.free。分析函数contrived中的两条路径：
² 1->2->3->4->6：在进行到6时，6的前置条件是p.free、w.free、q.free，此时语
句return *q将触发use-after-free规则并设置q.useAfterFree状态。然后返回到函数
contrived_caller的4，其前置条件为p.useAfterFree、w.free，此时语句return *w
设置w.useAfterFree。因此，存在use-after-free漏洞。
² 1->2->5->6：该路径是安全的。

### lecture07part2-漏洞挖掘.pdf / Page 66

示例二：检测缓冲区溢出
在检测缓冲区溢出时，我们关心的是变量的取值，并在一些预定义的敏感操作所
在的程序点上，对变量的取值进行检查。下面是一些记录变量的取值的规则。

char s[n];

// len(s) = n

strcpy(des, src);

// len(des) > len(src)

strncpy(des, src, n); // len(des) > min(len(src), n)
s = "foo";

// len(s) = 4

strcat(s, suffix);

// len(s) = len(s) + len(suffix) - 1

fgets(s, n, ...);

// len(s) > n

### lecture07part2-漏洞挖掘.pdf / Page 67

单选题

1分

有关基于数据流的漏洞分析流程，说法错误的是

A

关心的是变量的取值

B

通常需要对程序进行代码建模

C

通过敏感函数的识别和参数分析发现漏洞

D

适合检查因控制流信息非法操作而导致的安全问题
提交

### lecture07part2-漏洞挖掘.pdf / Page 68

程序切片（slicing）

做什么？
只保留与某个危险点的相关代码
把非相关代码剪掉
本质是依赖关系追踪

### lecture07part2-漏洞挖掘.pdf / Page 69

依赖关系

数据依赖
Data dependency

控制依赖
Control dependency

谁给危险变量赋值了？

某条语句执行取决于谁？

### lecture07part2-漏洞挖掘.pdf / Page 70

依赖关系

数据依赖
Data dependency

控制依赖
Control dependency

谁给危险变量赋值了？

某条语句执行取决于谁？

buf <- input

system(cmd) <- isAdmin(user)

### lecture07part2-漏洞挖掘.pdf / Page 71

依赖关系

利用数据依赖识别
untrusted input reaches SQL sink

### lecture07part2-漏洞挖掘.pdf / Page 72

程序切片 for debugging
1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
13.
14.
15.
16.

### lecture07part2-漏洞挖掘.pdf / Page 73

程序切片 for debugging

静态切片

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
13.
14.
15.
16.

### lecture07part2-漏洞挖掘.pdf / Page 74

程序切片 for debugging

静态切片

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
13.
14.
15.
16.

### lecture07part2-漏洞挖掘.pdf / Page 75

程序切片 for debugging

静态切片

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
13.
14.
15.
16.

### lecture07part2-漏洞挖掘.pdf / Page 76

动态切片

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
13.
14.
15.
16.

2,1,3

程序切片 for debugging

x
x
x
x
x
x

x

### lecture07part2-漏洞挖掘.pdf / Page 77

动态切片

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
13.
14.
15.
16.

2,1,3

程序切片 for debugging

x
x
x
x
x
x

不必考虑切片外的代码
降低调试难度
x

### lecture07part2-漏洞挖掘.pdf / Page 78

局限

•
•
•
•
•

Slice太大
路径不精确
指针/别名问题
动态行为不精准分析
二进制代码的复杂性

### lecture07part2-漏洞挖掘.pdf / Page 79

当前进展
先slice再LLM分析

• 切片 + 深度学习/LLM
• 切片 + 历史CVE检索
• Assembly slicing +
vulnerability localization

二进制代码的slicing场景

先提取CVE slice
再搜相似slices

### lecture07part2-漏洞挖掘.pdf / Page 80

知识点四：模糊测试

### lecture07part2-漏洞挖掘.pdf / Page 81

模糊测试
（fuzzing testing, or fuzzing）

做什么？
自动生成大量异常、边界、变异输入
来执行程序
观察是否崩溃、异常、或出现危险行为

### lecture07part2-漏洞挖掘.pdf / Page 82

模糊测试
（fuzzing testing, or fuzzing）

核心思想
制造“非正常输入”
触发行为异常

### lecture07part2-漏洞挖掘.pdf / Page 83

模糊测试
（fuzzing testing, or fuzzing）

核心思想
制造“非正常输入”
•
触发行为异常
•
•
•
•
•
•

缓冲区溢出
整数溢出
越界访问
use-after-free
空指针解引用
parser bug
协议状态机错误

### lecture07part2-漏洞挖掘.pdf / Page 84

模糊测试
（fuzzing testing, or fuzzing）

输入

程序执行

行为检测

### lecture07part2-漏洞挖掘.pdf / Page 85

模糊测试
（fuzzing testing, or fuzzing）

输入
检测方式

程序执行
•
•
•
•
•

行为检测

Crash
Hang
Assertion failure
Sanitizer alert
Memory corruption

### lecture07part2-漏洞挖掘.pdf / Page 86

模糊测试分类

黑盒

覆盖引导

定向

语法引导

不看代码
只疯狂喂输入

优先保留能走到
新代码/路径的
输入

不随机找bug
定向
针对危险位置

针对结构化输入

### lecture07part2-漏洞挖掘.pdf / Page 87

模糊测试分类

•
•
•
•
•

HTML
PDF
SQL
Network protocol
JSON/XML

黑盒

覆盖引导

定向

语法引导

不看代码
只疯狂喂输入

优先保留能走到
新代码/路径的
输入

不随机找bug
定向
针对危险位置

针对结构化输入

### lecture07part2-漏洞挖掘.pdf / Page 88

模糊测试例子

### lecture07part2-漏洞挖掘.pdf / Page 89

模糊测试例子

Sanitizer

### lecture07part2-漏洞挖掘.pdf / Page 90

ASAN

### lecture07part2-漏洞挖掘.pdf / Page 91

ASAN

高开销
用于bug检测

1

buf溢出会访问redzone区域
2

Shadow
Memory

3

程序执行
crash

### lecture07part2-漏洞挖掘.pdf / Page 92

模糊测试分类

黑盒

覆盖引导

定向

语法引导

不看代码
只疯狂喂输入

优先保留能走到
新代码/路径的
输入

不随机找bug
定向
针对危险位置

针对结构化输入

### lecture07part2-漏洞挖掘.pdf / Page 109

1. 模糊测试
模糊测试(Fuzzing)是一种自动化或半自动化的安全漏洞
1

检测技术，通过向目标软件输入大量的畸形数据并监测
目标系统的异常来发现潜在的软件漏洞。
模糊测试属于黑盒测试的一种，它是一种有效的动态漏

2

洞分析技术，黑客和安全技术人员使用该项技术已经发
现了大量的未公开漏洞。

3

它的缺点是畸形数据的生成具有随机性，而随机性造成
代码覆盖不充分导致了测试数据覆盖率不高。

### lecture07part2-漏洞挖掘.pdf / Page 110

模糊测试分类
基于生成的模糊测试

它是指依据特定的文件格式或者协议规范组合生成测试用例，该方法的关键
点在于既要遵守被测程序的输入数据的规范要求，又要能变异出区别于正常
的数据。
基于变异的模糊测试

它是指在原有合法的测试用例基础上，通过变异策略生成新的测试用例。变
异策略可以是随机变异策略、边界值变异策略、位变异策略等等，但前提条
件是给定的初始测试用例是合法的输入。

### lecture07part2-漏洞挖掘.pdf / Page 111

模糊测试步骤
（1）确定测试对象和输入数据

由于所有可被利用的漏洞都是由于应用程序接受了用户输入的数据造成的，
并且在处理输入数据时没有首先过滤非法数据或者进行校验确认。对模糊测
试来说首要的问题是确定可能的输入数据，畸形输入数据的枚举对模糊测试
至关重要。
（2）生成模糊测试数据

一旦确定了输入数据，接着就可以生成模糊测试用的畸形数据。根据目标程
序及输入数据格式的不同，可相应选择不同的测试数据生成算法。

### lecture07part2-漏洞挖掘.pdf / Page 112

模糊测试步骤
（3）检测模糊测试数据

检测模糊测试数据的过程首先要启动目标程序，然后把生成的测试数据输入
到应用程序中进行处理。

（4）监测程序异常

在模糊测试过程中，一个非常重要但却经常被忽视的步骤是对程序异常的监
测。实时监测目标程序的运行，就能追踪到引发目标程序异常的源测试数据。

### lecture07part2-漏洞挖掘.pdf / Page 113

模糊测试步骤

一旦监测到程序出现的异常，还需要进一步确定所发现的异常
情况是否能被进一步利用。这个步骤不是模糊测试必需的步骤，
只是检测这个异常对应的漏洞是否可以被利用。这个步骤一般
由手工完成，需要分析人员具备深厚的漏洞挖掘和分析经验。

### lecture07part2-漏洞挖掘.pdf / Page 114

所有类型的模糊测试技术

!"!"##

除了最后一步确定可利用性外，所有其它的四个阶段都
是必须的。上述典型的fuzzing测试流程如左图所示。
$%!"&'

$%&'()*!

/+01+,

2
()&'*+,.

3

尽管模糊测试对安全缺陷和漏洞的检测能力很强，但并
不是说它对被测软件都能发现所有的错误，原因就是它
测试样本的生成方式具有随机性。

### lecture07part2-漏洞挖掘.pdf / Page 115

2. 智能模糊测试

p 模糊测试方法是应用最普遍的动态安全检测方法，但由于模糊测试数
据的生成具有随机性，缺乏对程序的理解，测试的性能不高，并且难
以保证一定的覆盖率。
p 为了解决这个问题，引入了基于符号执行、污点传播分析等可进行程
序理解的方法，在实现程序理解的基础上，有针对性的设计测试数据
的生成，从而实现了比传统的随机模糊测试更高的效率，这种结合了
程序理解和模糊测试的方法，称为智能模糊测试(smart Fuzzing)技
术。

### lecture07part2-漏洞挖掘.pdf / Page 116

智能模糊测试具体的实现步骤如下

（1）反汇编
智能模糊测试的前提，是对可执行代码
进行输入数据、控制流、执行路径之间
相关关系的分析。为此，首先对可执行
代码进行反汇编得到汇编代码，在汇编
代码的基础上才能进行上述分析。

（2）中间语言转换
从汇编代码中直接获取程序运行的内部
信息，工作量较大，为此，需要将汇编
代码转换成中间语言，由于中间语言易
于理解，所以为可执行代码的分析提供
了一种有效的手段。

### lecture07part2-漏洞挖掘.pdf / Page 117

（3）采用智能技术分析输入数据和执行路径的关系

这一步是智能模糊测试的关键，它通过符号执行和约束求解技术、污点传播分析、执行路
径遍历等技术手段，检测出可能产生漏洞的程序执行路径集合和输入数据集合。例如，利
用符号执行技术在符号执行过程中记录下输入数据的传播过程和传播后的表达形式，并通
过约束求解得到在漏洞触发时执行的路径与原始输入数据之间的联系，从而得到触发执行
路径异常的输入数据。

### lecture07part2-漏洞挖掘.pdf / Page 118

（4）利用分析获得的输入数据集合，对执行路径集合进行测试
采用上述智能技术获得的输入数据集合进行
安全检测，使后续的安全测试检测出安全缺
陷和漏洞的机率大大增加。与传统的随机模
糊测试技术相比，这些智能模糊测试技术的
应用，由于了解了输入数据和执行路径之间
的关系，因而生成的输入数据更有针对性，
减少了大量无关测试数据的生成，提高了测
试的效率。此外，在触发漏洞的同时，智能
模糊测试技术包含了对漏洞成因的分析，极
大减少了分析人员的工作量。

### lecture07part2-漏洞挖掘.pdf / Page 119

智能模糊测试的核心思想
在于以尽可能小的代价找出程序中最有可能产生漏洞的执行路径集合，从而避免了
盲目地对程序进行全路径覆盖测试，使得漏洞分析更有针对性。
智能模糊测试技术的提出，反映了软件安全性测试由模糊化测试向精确化测试转变
的趋势。是典型的技术融合的漏洞挖掘测试方法。

### lecture07part2-漏洞挖掘.pdf / Page 120

3. 模糊测试实践
（1）使用工具
p 用来实现Fuzzing测试的工具叫做Fuzzer。
p 成品的Fuzzer工具很多，许多是非常优秀
的。Fuzzer根据测试类型可以分为很多类，
常见的分类包括：文件型Fuzzer、网络型
Fuzzer、接口型Fuzzer等。
p 右侧工具可以生成多个文件测试用例，发
现了Office2003的典型漏洞。

### lecture07part2-漏洞挖掘.pdf / Page 121

（2）自己动手写Fuzzer
使用模糊测试工具在很多时候不能解决所有问题
比如：被测试的目标程序对测试数据有一定的要求，而实际的
Fuzzer不能灵活调整发送的测试数据；被测试的目标程序过于
简单或者难，而现有的Fuzzer程序不能提供适合的测试。
作为漏洞发掘者我们最好能学会编写一个Fuzzer，这样就可以
随时随地的进行安全测试。而事实上，目前的多数漏洞挖掘过
程，是需要自己手动编写Fuzzer来完成。

### lecture07part2-漏洞挖掘.pdf / Page 122

对于目标的可执行文件overflow.exe文件，是由如下程序生成的exe程序：

#include <stdio.h>
#include <string.h>
void overflow(char *b){
char des[50];
strcpy(des,b);
}
void main(int argc,char *argv[]){
if(argc>1)
{
overflow(argv[1]);
} else
printf("usage: overflow XXXXX\n");
}

### lecture07part2-漏洞挖掘.pdf / Page 123

书写Fuzzer
在明确了输入的要求和暴力测试的循环条件后，可以写出如下的代码：

void main(int argc,char *argv[]){
char *testbuf=" ";
char buf[1024];
memset(buf,0,1024);
if(argc>1) {
for(int i=20;i<50;i=i+2) {
testbuf=new char[i];
memset(testbuf,'c',i);
memcpy(buf,testbuf,i);
ShellExecute(NULL,"open",argv[1],buf,NULL,SW_NORMAL);
delete testbuf;}
}
else printf("Fuzzing X \n其中X为被测试目标程序所在路径");
}

### lecture07part2-漏洞挖掘.pdf / Page 124

以上代码

通过一个for循环（循环次数根据实际情况去设计）构造不同的字符串作为输入，
通过“ShellExecute(NULL,"open",argv[1],buf,NULL,SW_NORMAL); “
来实现对目标程序的模糊测试。
上述Fuzzer的调用格式为：Fuzzing X 。X表示目标程序。
请完成上述实验并进行结果验证。

### lecture07part2-漏洞挖掘.pdf / Page 125

知识点五：AFL模糊测试框架

### lecture07part2-漏洞挖掘.pdf / Page 126

AFL模糊测试框架
AFL是一款基于覆盖引导（Coverage-guided）的模糊测
1

试工具，它通过记录输入样本的代码覆盖率，从而调整
输入样本以提高覆盖率，增加发现漏洞的概率。
AFL主要用于C/C++程序的测试，被测程序有无程序源

2

码均可，有源码时可以对源码进行编译时插桩，无源码
可以借助QEMU的User-Mode模式进行二进制插装。

3

支持多平台（ARM、X86、X64）、多系统（Linux、
BSD、Windows、MacOS），性能高。

### lecture07part2-漏洞挖掘.pdf / Page 127

AFL
American Fuzz Lop(AFL) is one of the most famous CGF.

AFLFast

FairFuzz

CollAFL

QSYM

……

### lecture07part2-漏洞挖掘.pdf / Page 128

Coverage-based Greybox Fuzzing
Coverage-based Greybox Fuzzing is the state of the art method to find vulnerabilities.

Mutation

### lecture07part2-漏洞挖掘.pdf / Page 129

AFL mutation

Deterministic stage

Havoc stage

Bit flip
Byte flip
Arithmetic operation
Interesting value replacement
Token replacement

Randomly mangled input

### lecture07part2-漏洞挖掘.pdf / Page 130

Deterministic Byte Flip

Random (Havoc)

### lecture07part2-漏洞挖掘.pdf / Page 131

Deterministic mutation

+
New coverage

x

+
New coverage

x

x

x

x

+
New coverage

L: length of input
Number of new inputs generated by bit flip = L * 8 * 3

### lecture07part2-漏洞挖掘.pdf / Page 132

AFL工作流程
p 从源码编译程序时进行插桩，
以记录代码覆盖率；
p 选择一些输入文件作为初始
测试集加入输入队列；
p 将队列中的文件按策略进行
“突变”；
p 如果经过变异文件更新了覆
盖范围，则保留在队列中;
p 循环进行，期间触发了
crash的文件会被记录下来。

### lecture07part2-漏洞挖掘.pdf / Page 133

AFL安装
p 在Kali 2021下，利用sudo apt-get install afl即可安装。
p 查看路径可以看到afl安装的文件：ls /usr/bin/afl*

• afl-gcc和afl-g++分别对应的是gcc和g++的封装。
• afl-fuzz是AFL的主体，用于对目标程序进行fuzz。
• afl-analyze可以对用例进行分析，看能否发现用例中有意义的字段。
• afl-tmin和afl-cmin对用例进行简化。
• afl-showmap用于对单个用例进行执行路径跟踪。

### lecture07part2-漏洞挖掘.pdf / Page 134

AFL模糊测试
以一个白盒模糊测试为例。
（1）创建本次实验的程序：新建文件夹
demo，并创建实验的程序Test.c，该代
码编译后得到的程序如果被传入
“deadbeef”则会终止，如果传入其他
字符会原样输出。
使用afl的编译器编译，可以使模糊测试
过程更加高效。
命令：afl-gcc -o test test.c

### lecture07part2-漏洞挖掘.pdf / Page 135

AFL模糊测试
编译后会有插桩符号，使用下面的命令可以验证这一点。
命令：readelf -s ./test | grep afl

### lecture07part2-漏洞挖掘.pdf / Page 136

AFL模糊测试
（2）创建测试用例
首先，创建两个文件夹in和out，分别存储模糊测试所需的输入和输出相关的内容。
命令：mkdir in out
然后，在输入文件夹中创建一个包含字符串“hello”的文件。
命令：echo hello> in/foo
foo就是我们的测试用例，里面包含初步字符串hello。AFL会通过这个语料进行变
异，构造更多的测试用例。
（3）启动模糊测试
运行如下命令，开始启动模糊测试（@@表示目标程序需要从文件读取输入）：
命令：afl-fuzz -i in -o out -- ./test @@

### lecture07part2-漏洞挖掘.pdf / Page 137

AFL模糊测试
（4）分析crash
观察fuzzing结果，如有crash，定位问题。
p 在 out 文 件 夹 的 crashes 子
文件夹里面是产生crash的
样例，hangs里面是产生超
时的样例。
p 通常，得到crash样例后，
可以将这些样例作为目标测
试程序的输入，重新触发异
常并跟踪运行状态，进行分
析、定位程序出错的原因或
确认存在的漏洞类型。

### lecture07part2-漏洞挖掘.pdf / Page 138

局限

•
•
•
•

路径深度问题
结构化输入难
逻辑漏洞难发现
Crash ≠ exploitable

### lecture07part2-漏洞挖掘.pdf / Page 139

局限

•
•
•
•

漏洞难触发

路径深度问题
Mutation易把结构搞坏
结构化输入难
Fuzzing擅长找memory corruption
逻辑漏洞难发现
Crash ≠ exploitable
Crash不等于高价值漏洞

### lecture07part2-漏洞挖掘.pdf / Page 140

研究现状

• LLM + fuzzing
• 特定领域fuzzing
• PoC自动生成

### lecture07part2-漏洞挖掘.pdf / Page 141

研究现状

• LLM + fuzzing
• 特定领域fuzzing
• PoC自动生成
• Kernel fuzzing
不只crash而是自动生成
可复现漏洞输入

•
•
•
•
•

• Smart contract fuzzing
• Web API fuzzing
• DL framework fuzzing

Seed generation
Harness generation
Directed Mutation
Crash explanation
PoC generation

