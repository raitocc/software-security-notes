# Lecture04 缓冲区溢出漏洞 - 逐页文字版

> 说明：本文件由 PDF 文本层逐页抽取生成，用于核对覆盖；复习请优先看同目录下的“复习讲义”。

## 来源：lecture04-缓冲区溢出漏洞.pdf

### lecture04-缓冲区溢出漏洞.pdf / Page 1

第四章 软件漏洞
漏洞基本概念
栈溢出漏洞
堆溢出漏洞
其它溢出漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 2

攻击者实施攻击的基本流程是什么？

### lecture04-缓冲区溢出漏洞.pdf / Page 4

攻击的
生命周期模型
建立长期控
制（后门/
木马）

发钓鱼邮件
恶意文件到
网站

收集信息

制作攻击工
具（漏洞+
利用代码）

漏洞被利用
执行攻击

真正实施破坏
（窃取数据、
勒索等）

远程操控被
攻陷的系统

### lecture04-缓冲区溢出漏洞.pdf / Page 5

攻击的
生命周期模型
建立长期控
制（后门/
木马）

发钓鱼邮件
恶意文件到
网站

收集信息

制作攻击工
具（漏洞+
利用代码）

漏洞被利用
执行攻击

真正实施破坏
（窃取数据、
勒索等）

远程操控被
攻陷的系统

### lecture04-缓冲区溢出漏洞.pdf / Page 6

技术层面的流程

1

信息
收集

2

漏洞
发现

3

利用
开发

4

初始
入侵

5 后渗透

6

清理
痕迹

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 7

技术层面的流程

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

4

初始
入侵

5 后渗透

6

清理
痕迹

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 8

技术层面的流程

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

内存破坏（overflow，
UAF），逻辑缺陷（认
证绕过，权限提升），
misconfiguration

4

初始
入侵

5 后渗透

6

清理
痕迹

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 9

技术层面的流程
控制RIP/EIP，随意读
写，绕过保护（ASLR、
DEP、stack canaries）

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

内存破坏（overflow，
UAF），逻辑缺陷（认
证绕过，权限提升），
misconfiguration

4

初始
入侵

5 后渗透

6

清理
痕迹

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 10

技术层面的流程
控制RIP/EIP，随意读
写，绕过保护（ASLR、
DEP、stack canaries）

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

内存破坏（overflow，
UAF），逻辑缺陷（认
证绕过，权限提升），
misconfiguration

4

初始
入侵

得到Shell、注⼊代码、
执⾏payload

5 后渗透

6

清理
痕迹

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 11

技术层面的流程
控制RIP/EIP，随意读
写，绕过保护（ASLR、
DEP、stack canaries）

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

内存破坏（overflow，
UAF），逻辑缺陷（认
证绕过，权限提升），
misconfiguration

4

权限提升、持续控制
（reboot保护、加后
门）、横向处理（攻击
⽹络中其他机器）、数
据渗出、维护通信

初始
入侵

得到Shell、注⼊代码、
执⾏payload

5 后渗透

6

清理
痕迹

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 12

技术层面的流程
控制RIP/EIP，随意读
写，绕过保护（ASLR、
DEP、stack canaries）

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

内存破坏（overflow，
UAF），逻辑缺陷（认
证绕过，权限提升），
misconfiguration

4

权限提升、持续控制
（reboot保护、加后
门）、横向处理（攻击
⽹络中其他机器）、数
据渗出、维护通信

初始
入侵

得到Shell、注⼊代码、
执⾏payload

5 后渗透

6

清理
痕迹

删除⽇志、
模糊化等

7 武器化

### lecture04-缓冲区溢出漏洞.pdf / Page 13

技术层面的流程
控制RIP/EIP，随意读
写，绕过保护（ASLR、
DEP、stack canaries）

1

信息
收集

什么软件，版本？什么
⽹络服务？端⼝？什么
technologies（ 什么 web
架构、库等）

2

漏洞
发现

3

利用
开发

内存破坏（overflow，
UAF），逻辑缺陷（认
证绕过，权限提升），
misconfiguration

4

权限提升、持续控制
（reboot保护、加后
门）、横向处理（攻击
⽹络中其他机器）、数
据渗出、维护通信

初始
入侵

得到Shell、注⼊代码、
执⾏payload

5 后渗透

6

清理
痕迹

删除⽇志、
模糊化等

7 武器化

利⽤包装为⼯具，
开发蠕⾍等

### lecture04-缓冲区溢出漏洞.pdf / Page 14

漏洞、利用与破坏

### lecture04-缓冲区溢出漏洞.pdf / Page 15

漏洞（Vulnerability）

漏洞也称为脆弱性(Vulnerability)，是计算机系统的硬件、软件、协
议在系统设计、具体实现、系统配置或安全策略上存在的缺陷。这些
缺陷一旦被发现并被恶意利用，就会使攻击者在未授权的情况下访问
或破坏系统，从而影响计算机系统的正常运行甚至造成安全损害。
p 对于漏洞有多种称呼，包括Hole, Error, Fault, Weakness, Failure
等，这些称呼都不能涵盖漏洞的含义（脆弱性）。
p 软件漏洞专指计算机系统中的软件系统漏洞。

### lecture04-缓冲区溢出漏洞.pdf / Page 16

漏洞（Vulnerability）

In computer security, vulnerabilities are flaws or weaknesses
in a system's design, implementation, or management
that can be exploited by a malicious actor to compromise its security

可被利用来破坏软件安全的缺陷

### lecture04-缓冲区溢出漏洞.pdf / Page 17

漏洞（Vulnerability）

有什么类型？

### lecture04-缓冲区溢出漏洞.pdf / Page 18

漏洞类型

内存安全漏洞

输入校验与
注入漏洞

身份认证与
授权漏洞

加密类漏洞

竞争与并发类
漏洞

业务逻辑漏洞

信息泄漏漏洞

配置与部署
漏洞

反序列化与类
型混淆漏洞

拒绝服务漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 19

Buffer Overflow, UAF,
Double Free, Out-ofBounds Read/Write,
Uninitialized Memory
Use

漏洞类型

内存安全漏洞

输入校验与
注入漏洞

身份认证与
授权漏洞

加密类漏洞

竞争与并发类
漏洞

业务逻辑漏洞

信息泄漏漏洞

配置与部署
漏洞

反序列化与类
型混淆漏洞

拒绝服务漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 20

Buffer Overflow, UAF,
Double Free, Out-ofBounds Read/Write,
Uninitialized Memory
Use

漏洞类型
SQL injection, command
injection, cross-site
scripting (XSS), XML
eternal entity (XXE)

内存安全漏洞

输入校验与
注入漏洞

身份认证与
授权漏洞

加密类漏洞

竞争与并发类
漏洞

业务逻辑漏洞

信息泄漏漏洞

配置与部署
漏洞

反序列化与类
型混淆漏洞

拒绝服务漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 21

Buffer Overflow, UAF,
Double Free, Out-ofBounds Read/Write,
Uninitialized Memory
Use

漏洞类型
SQL injection, command
injection, cross-site
scripting (XSS), XML
eternal entity (XXE)

认证绕过、弱⼝令
Privilege Escalation,
Access Control Bypass

内存安全漏洞

输入校验与
注入漏洞

身份认证与
授权漏洞

加密类漏洞

竞争与并发类
漏洞

业务逻辑漏洞

信息泄漏漏洞

配置与部署
漏洞

反序列化与类
型混淆漏洞

拒绝服务漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 22

Buffer Overflow, UAF,
Double Free, Out-ofBounds Read/Write,
Uninitialized Memory
Use

漏洞类型
SQL injection, command
injection, cross-site
scripting (XSS), XML
eternal entity (XXE)

认证绕过、弱⼝令
Privilege Escalation,
Access Control Bypass

内存安全漏洞

输入校验与
注入漏洞

身份认证与
授权漏洞

加密类漏洞

竞争与并发类
漏洞

业务逻辑漏洞

信息泄漏漏洞

配置与部署
漏洞

反序列化与类
型混淆漏洞

拒绝服务漏洞

敏感信息泄漏（调试、
错误信息）、内存数据
泄漏、地址信息泄漏
（绕过ASLR）

默认密码、权限配置错
误、暴露不必要的服务
/端⼝

不安全反序列化、
类型混淆

资源耗尽、
算法复杂度攻击、
死循环/崩溃触发

### lecture04-缓冲区溢出漏洞.pdf / Page 23

Buffer Overflow, UAF,
Double Free, Out-ofBounds Read/Write,
Uninitialized Memory
Use

漏洞类型
SQL injection, command
injection, cross-site
scripting (XSS), XML
eternal entity (XXE)

认证绕过、弱⼝令
Privilege Escalation,
Access Control Bypass

内存安全漏洞

输入校验与
注入漏洞

身份认证与
授权漏洞

加密类漏洞

竞争与并发类
漏洞

业务逻辑漏洞

信息泄漏漏洞

配置与部署
漏洞

反序列化与类
型混淆漏洞

拒绝服务漏洞

敏感信息泄漏（调试、
错误信息）、内存数据
泄漏、地址信息泄漏
（绕过ASLR）

默认密码、权限配置错
误、暴露不必要的服务
/端⼝

不安全反序列化、
类型混淆

资源耗尽、
算法复杂度攻击、
死循环/崩溃触发

### lecture04-缓冲区溢出漏洞.pdf / Page 24

内存安全漏洞

是什么？为什么？

### lecture04-缓冲区溢出漏洞.pdf / Page 25

内存安全漏洞
Ø

Ø

Ø

空间错误
•

缓冲区溢出（Buffer Overflow）

•

越界读写（Out-of-bounds read/write）

时间错误
•

释放后使用（Use-after-free）

•

双重释放（Double free）

初始化错误
•

使用未初始化内存（Use of uninitialized memory）

### lecture04-缓冲区溢出漏洞.pdf / Page 26

内存安全漏洞
Ø

Ø

可导致全系统破坏
•

内存漏洞是随意代码执行（arbitrary code execution）的主要诱因

•

可覆盖返回地址、劫持控制流

破坏核心安全防护
•

ASLR、DEP/NX、stack canaries

•

双重释放（Double free）

Ø

大量CVE实例的根因

Ø

与其他手段配合
•

Ø

信息泄漏、ROP

C/C++程序较难避免
•

人工内存管理、指针计算、缺乏运行时检查

### lecture04-缓冲区溢出漏洞.pdf / Page 28

缓冲区溢出漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 29

缓冲区溢出漏洞
缓冲
区

缓冲区是一块连续的内存区域，用于存放程序
运行时加载到内存的运行代码和数据。

### lecture04-缓冲区溢出漏洞.pdf / Page 30

缓冲区溢出漏洞
缓冲
区

缓冲区是一块连续的内存区域，用于存放程序
运行时加载到内存的运行代码和数据。

缓冲
区溢
出

缓冲区溢出是指程序运行时，向固定大小的缓
冲区写入超过其容量的数据，多余的数据会越
过缓冲区的边界覆盖相邻内存空间，从而造成
溢出。

### lecture04-缓冲区溢出漏洞.pdf / Page 31

缓冲区溢出攻击是指发生缓冲区溢出时，溢出的
缓冲区
溢出攻
击

数据会覆盖相邻内存空间的返回地址、函数指针、
堆管理结构等合法数据，从而使程序运行失败、
或者发生转向去执行其它程序代码、或者执行预
先注入到内存缓冲区中的代码。

缓冲区溢出后执行的代码，会
以原有程序的身份权限运行。

### lecture04-缓冲区溢出漏洞.pdf / Page 33

覆写后
跳转地址
执行非预期代码
如恶意代码

### lecture04-缓冲区溢出漏洞.pdf / Page 34

是缺乏类型安全功能的程序设计语言（C、C++等）
造成缓冲
区溢出的
根本原因

出于效率的考虑，部分函数不对数组边界条件和函
数指针引用等进行边界检查。例如，C 标准库中和
字符串操作有关的函数，像strcpy，strcat，sprintf，
gets等函数中，数组和指针都没有自动边界检查。

程序员开发时必须自己进行边界检查，防范数据溢出，否则所开发的程序就
存在缓冲区溢出的安全隐患，而实际上这一行为往往被程序员忽略或者检查
不充分。
缓冲区溢出通常包括栈溢出、堆溢出、异常处理SEH结构溢出、单字节溢出等

### lecture04-缓冲区溢出漏洞.pdf / Page 35

单选题

漏洞也称为

A

Hole

B

Fault

C

Vulnerability

D

Weakness
提交

### lecture04-缓冲区溢出漏洞.pdf / Page 36

单选题

漏洞也称为

A

Hole

B

Fault

C

Vulnerability

D

Weakness
提交

### lecture04-缓冲区溢出漏洞.pdf / Page 37

知识点二：栈溢出漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 38

1. 基本概念
栈溢出漏洞：

栈溢出漏洞，即发生在栈区的溢出漏洞。被调用的子函数中写
入数据的长度，大于栈帧的基址到esp之间预留的保存局部变量
的空间时，就会发生栈的溢出。要写入数据的填充方向是
从低地址向高地址增长，多余的数据就会越过栈帧的基址，覆
盖基址以上的地址空间。

### lecture04-缓冲区溢出漏洞.pdf / Page 39

下面的程序演示了一个溢出漏洞，代码如下

void why_here(void)
{ printf("why u r here?!\n");
exit(0);
}
void f()
{ int buff[1];
buff[2] = (int)why_here;
}
int main(int argc, char * argv[])
{
f();
return 0;
}

2

栈溢出漏洞示例

### lecture04-缓冲区溢出漏洞.pdf / Page 40

下面的程序演示了一个溢出漏洞，代码如下

void why_here(void)
{ printf("why u r here?!\n");
exit(0);
}
void f()
{ int buff[1];
buff[2] = (int)why_here;
}
int main(int argc, char * argv[])
{
f();
return 0;
}

2

栈溢出漏洞示例

### lecture04-缓冲区溢出漏洞.pdf / Page 41

下面的程序演示了一个溢出漏洞，代码如下

void why_here(void)
{ printf("why u r here?!\n");
exit(0);
}
void f()
{ int buff[1];
buff[2] = (int)why_here;
}
int main(int argc, char * argv[])
{
f();
return 0;
}

2

栈溢出漏洞示例

buff是长度为1的数组

### lecture04-缓冲区溢出漏洞.pdf / Page 42

下面的程序演示了一个溢出漏洞，代码如下

void why_here(void)
{ printf("why u r here?!\n");
exit(0);
}
void f()
{ int buff[1];
buff[2] = (int)why_here;
}
int main(int argc, char * argv[])
{
f();
return 0;
}

2

栈溢出漏洞示例

buff[2]可以被赋值吗？
这条语句改变了什么？

### lecture04-缓冲区溢出漏洞.pdf / Page 43

如程序所示，主函数将调用函数f，并没有调用why_here函数，但是运行结果如下：

### lecture04-缓冲区溢出漏洞.pdf / Page 44

在函数f中，所声明的数组buff长度为1，但是由于没有对访问下标的值进
行校验，程序中对数组外的内存进行了读写，这是一个典型的溢出漏洞。

void f()
{
int buff[1];
buff[2] = (int)why_here;
}

低地址
BUFF

buff[0]
EBP
返回地址

高地址

### lecture04-缓冲区溢出漏洞.pdf / Page 45

在函数f中，所声明的数组buff长度为1，但是由于没有对访问下标的值进
行校验，程序中对数组外的内存进行了读写，这是一个典型的溢出漏洞。

void f()
{
int buff[1];
buff[2] = (int)why_here;
}

低地址
BUFF

buff[0]
EBP
返回地址

buff[1]（四字节）
buff[2]（四字节）

局部变量从低到高地址

高地址

### lecture04-缓冲区溢出漏洞.pdf / Page 46

在函数f中，所声明的数组buff长度为1，但是由于没有对访问下标的值进
行校验，程序中对数组外的内存进行了读写，这是一个典型的溢出漏洞。

void f()
{
int buff[1];
buff[2] = (int)why_here;
}

低地址
BUFF

buff[0]
buff[1]（四字节）
EBP
why_here地址 buff[2]（四字节）

高地址

地址覆写
buff[2]=（int）why_here

局部变量从低到高地址

### lecture04-缓冲区溢出漏洞.pdf / Page 47

在函数f中，所声明的数组buff长度为1，但是由于没有对访问下标的值进
行校验，程序中对数组外的内存进行了读写，这是一个典型的溢出漏洞。

void f()
{
int buff[1];
buff[2] = (int)why_here;
}

低地址
BUFF

buff[0]
EBP
why_here地址

覆写后会发生什么？

高地址

### lecture04-缓冲区溢出漏洞.pdf / Page 48

在函数f中，所声明的数组buff长度为1，但是由于没有对访问下标的值进
行校验，程序中对数组外的内存进行了读写，这是一个典型的溢出漏洞。

低地址

void f()
{
int buff[1];
buff[2] = (int)why_here;
}

BUFF

buff[0]
EBP
why_here地址

f()函数结束后会弹出why_here的地址
作为function call的返回地址

高地址

### lecture04-缓冲区溢出漏洞.pdf / Page 49

在函数f中，所声明的数组buff长度为1，但是由于没有对访问下标的值进
行校验，程序中对数组外的内存进行了读写，这是一个典型的溢出漏洞。

低地址

void f()
{
int buff[1];
buff[2] = (int)why_here;
}

BUFF

buff[0]
EBP
why_here地址

f()函数结束后会弹出why_here的地址
作为function call的返回地址
随后，程序执行why_here，而不是返回main！

高地址

### lecture04-缓冲区溢出漏洞.pdf / Page 50

如程序所示，主函数将调用函数f，并没有调用why_here函数，但是运行结果如下：

### lecture04-缓冲区溢出漏洞.pdf / Page 51

覆写后
跳转地址
执行非预期代码
如恶意代码

### lecture04-缓冲区溢出漏洞.pdf / Page 52

此例中执行结果可能不确定：
• 栈布局可能不同，取决于
Ø 编译器
Ø 优化级别
Ø 架构（32位 vs. 64位）
• 保护机制
Ø 栈溢出保护（stack canaries）
Ø 地址随机化（ASLR）
Ø 数据执行保护（DEP）

### lecture04-缓冲区溢出漏洞.pdf / Page 53

bufferoverflow.c

gcc –o bufferoverflow bufferoverflow.c
./bufferoverflow

Linux AArch64
ARM 64位

### lecture04-缓冲区溢出漏洞.pdf / Page 54

bufferoverflow.c

gcc –o bufferoverflow bufferoverflow.c
./bufferoverflow

Linux AArch64
ARM 64位

### lecture04-缓冲区溢出漏洞.pdf / Page 55

bufferoverflow.c

gcc –o bufferoverflow bufferoverflow.c
./bufferoverflow

未成功覆盖地址，
打印“why u r here?”

Linux AArch64
ARM 64位

### lecture04-缓冲区溢出漏洞.pdf / Page 56

uname –m: aarch64 (64-bit ARM)
指针8字节
buff[2] = (int) why_here;

### lecture04-缓冲区溢出漏洞.pdf / Page 57

uname –m: aarch64 (64-bit ARM)
指针8字节
buff[2] = (int) why_here;
地址截断
why_here地址：
buff[2]：

0x00007ffb12345678
0x12345678

未成功改写为
why_here地址

### lecture04-缓冲区溢出漏洞.pdf / Page 58

uname –m: aarch64 (64-bit ARM)
指针8字节
buff[2] = (int) why_here;
地址截断
why_here地址：
buff[2]：

0x00007ffb12345678
0x12345678

未成功改写为
why_here地址

改为long类型
避免地址截断

### lecture04-缓冲区溢出漏洞.pdf / Page 59

bufferoverflow_long.c

gcc –o bufferoverflow_long bufferoverflow_long.c
./bufferoverflow_long

什么都没输出 …

### lecture04-缓冲区溢出漏洞.pdf / Page 60

bufferoverflow_long.c

核心问题
buff[2] 未触及返回地址

### lecture04-缓冲区溢出漏洞.pdf / Page 61

bufferoverflow_long.c

核心问题
buff[2] 未触及返回地址

栈保护机制
Stack Canary

### lecture04-缓冲区溢出漏洞.pdf / Page 62

去除栈保护
bufferoverflow_long.c

gcc -fno-stack-protector \
–o bufferoverflow_long bufferoverflow_long.c
./bufferoverflow_long

“Why u r here?!”
出现

### lecture04-缓冲区溢出漏洞.pdf / Page 63

3. 溢出漏洞利用示例
a

修改返回地址
栈的存取采用先进后出的策略，程序用它来保存函数调用时的有关信息，如函数参
数、返回地址，函数中的非静态局部变量存放在栈中。如果返回地址被覆盖，当覆
盖后的地址是一个无效地址，则程序运行失败。如果覆盖返回地址的是恶意程序的
入口地址，则源程序将转向去执行恶意程序。
下面以一段程序为例说明栈溢出的原理。

void stack_overflow(char* argument)
{
char local[4];
for(int i = 0; argument[i];i++)
local[i] = argument[i];
}

### lecture04-缓冲区溢出漏洞.pdf / Page 64

函数stack_overflow被调用时堆栈布局如下图所示。图中
local是栈中保存局部变量的缓冲区，根据char local[4]预
先分配的大小为4个字节，当向local中写入超过4个字节
的字符时，就会发生溢出。
低

高

local

低

AAAA

前一个栈帧指针

BBBB

返回地址

CCCC

argument

高

DDDD

如果CCCC地址为攻击代码的入口地址，就会调用攻击代码

### lecture04-缓冲区溢出漏洞.pdf / Page 65

b

覆盖邻接变量

### lecture04-缓冲区溢出漏洞.pdf / Page 66

b

覆盖邻接变量

void main()
{
int valid_flag = 0;
char password[1024];
while(1)
{
printf("please input password: ");
scanf("%s", password);
valid_flag = verify_password(password);
if(valid_flag)
{
printf ("incorrect password!\n\n");
}
else
{
printf("Congratulation! You have
passed the verification!\n");
break;
}
}
}

### lecture04-缓冲区溢出漏洞.pdf / Page 67

b

覆盖邻接变量

void main()
{
int valid_flag = 0;
char password[1024];
while(1)
{
printf("please input password: ");
1
scanf("%s", password);
valid_flag = verify_password(password);
if(valid_flag)
2
{
printf ("incorrect password!\n\n");
3
}
else
{
printf("Congratulation! You have
passed the verification!\n");
break;
}
}
}

错误密码可“骗过”检查

### lecture04-缓冲区溢出漏洞.pdf / Page 68

#include <stdio.h>
#include <iostream>
#define PASSWORD "12345678"
int verify_password(char * password)
{
int authenticated;
char buffer[8];
authenticated = strcmp(password, PASSWORD);
strcpy(buffer, password);
return authenticated;
}

### lecture04-缓冲区溢出漏洞.pdf / Page 69

#include <stdio.h>
#include <iostream>
#define PASSWORD "12345678"
buffer长度只有8字节
int verify_password(char * password)
而strcpy不检查长度
{
很容易造成buffer溢出！
int authenticated;
char buffer[8];
authenticated = strcmp(password, PASSWORD);
strcpy(buffer, password);
return authenticated;
}

### lecture04-缓冲区溢出漏洞.pdf / Page 70

#include <stdio.h>
#include <iostream>
#define PASSWORD "12345678"
int verify_password(char * password)
{
一个错误的password
int authenticated;
会使authenticated为1
char buffer[8];
authenticated = strcmp(password, PASSWORD);
strcpy(buffer, password);
return authenticated;
}

### lecture04-缓冲区溢出漏洞.pdf / Page 71

#include <stdio.h>
#include <iostream>
#define PASSWORD "12345678"
int verify_password(char * password)
{
一个错误的password
int authenticated;
会使authenticated为1
char buffer[8];
authenticated = strcmp(password, PASSWORD);
如何溢出让
strcpy(buffer, password);
authenticated为0？
return authenticated;
}

### lecture04-缓冲区溢出漏洞.pdf / Page 73

先写满8字节buffer，
然后溢出覆写authenticated

### lecture04-缓冲区溢出漏洞.pdf / Page 74

先写满8字节buffer，
然后溢出覆写authenticated
1

用一个大于”12345678”
的password
将authenticated置为1

authenticated=0x00000001

[01][00][00][00]

最低字节

### lecture04-缓冲区溢出漏洞.pdf / Page 75

先写满8字节buffer，
然后溢出覆写authenticated
2

用一个刚好为8字节的字符串
例如“22334455”

[2][2][3][3][4][4][5][5][\0]

### lecture04-缓冲区溢出漏洞.pdf / Page 76

先写满8字节buffer，
然后溢出覆写authenticated
2

用一个刚好为8字节的字符串
例如“22334455”

[2][2][3][3][4][4][5][5][\0]
第9字节溢出
刚好覆写authenticated第一字节

### lecture04-缓冲区溢出漏洞.pdf / Page 77

2

先写满8字节buffer，
然后溢出覆写authenticated
Off-by-one Overflow
[2][2][3][3][4][4][5][5][\0]
用一个刚好为8字节的字符串
例如“22334455”
第9字节溢出
刚好覆写authenticated第一字节

### lecture04-缓冲区溢出漏洞.pdf / Page 80

局部变量的栈中顺序
由编译器决定

•
•
•
•

优化访问
对齐要求
寄存器分配
调试信息

### lecture04-缓冲区溢出漏洞.pdf / Page 81

是缺乏类型安全功能的程序设计语言（C、C++等）
造成缓冲
区溢出的
根本原因

出于效率的考虑，部分函数不对数组边界条件和函
数指针引用等进行边界检查。例如，C 标准库中和
字符串操作有关的函数，像strcpy，strcat，sprintf，
gets等函数中，数组和指针都没有自动边界检查。

### lecture04-缓冲区溢出漏洞.pdf / Page 82

是缺乏类型安全功能的程序设计语言（C、C++等）
造成缓冲
区溢出的
根本原因

出于效率的考虑，部分函数不对数组边界条件和函
数指针引用等进行边界检查。例如，C 标准库中和
字符串操作有关的函数，像strcpy，strcat，sprintf，
gets等函数中，数组和指针都没有自动边界检查。
strcpy

拷贝字符无安全检查

strncpy

最多拷贝n个字符 （拷贝后结尾可能无\0）

snprintf

往字符串里格式化写入，并限制写入长度

### lecture04-缓冲区溢出漏洞.pdf / Page 83

是缺乏类型安全功能的程序设计语言（C、C++等）
造成缓冲
区溢出的
根本原因

出于效率的考虑，部分函数不对数组边界条件和函
数指针引用等进行边界检查。例如，C 标准库中和
字符串操作有关的函数，像strcpy，strcat，sprintf，
gets等函数中，数组和指针都没有自动边界检查。

但如果我每次强制缓冲区的边界检查，
问题是不就解决了？

### lecture04-缓冲区溢出漏洞.pdf / Page 84

理论上是的，但现实很难做到

### lecture04-缓冲区溢出漏洞.pdf / Page 85

理论上是的，但现实很难做到
• 程序不知道该检查什么边界
•

buffer是指针，指向边界大小不确定

•

if (len < 100) { … } len可能来自攻击者

•

多在入口处，中间会经历多次运算/偏移

•

多线程；数据替换

•

UAF, Double free

•

性能下降，代码复杂度增加

•

覆盖函数指针，结构体字段

• 边界检查的局限

• 检查不覆盖所有路径
• 时间差问题

• 只解决空间错误，而非时间错误
• 性能与成本

• 即便不越界，也可能危险

### lecture04-缓冲区溢出漏洞.pdf / Page 86

工业界做法

语言层面
Rust语言

编译器/运行时
•
•
•

ASAN（检测越界，UAF）
CFI （控制流完整性）
Bounds checking（MPX、
SoftBound）

其他防护
•
•
•

ASLR
DEP/NX
Shadow Stack

### lecture04-缓冲区溢出漏洞.pdf / Page 87

ASAN

### lecture04-缓冲区溢出漏洞.pdf / Page 88

ASAN

对原始程序进行插桩
（instrumentation）

### lecture04-缓冲区溢出漏洞.pdf / Page 89

ASAN

1

buf溢出会访问redzone区域
2

Shadow
Memory

3

程序执行
crash

### lecture04-缓冲区溢出漏洞.pdf / Page 90

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

### lecture04-缓冲区溢出漏洞.pdf / Page 91

堆溢出漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 92

1. 简单示例
堆溢出漏洞：

堆溢出是指在堆中发生的缓冲区溢出。堆溢出后，数据可以覆
盖堆区的不同堆块的数据，带来安全威胁。我们将通过下面一
个简单例子，来演示一个简单的堆溢出漏洞：该漏洞在产生溢
出的时候，将覆盖一个目标堆块的块身数据。
示例：从堆区申请两个堆块，处于低地址的buf1和处于高地址的buf2。buf2存储
了一个名为myoutfile 的字符串，用来存储文件名。buf1用来接收输入，同时将这
些输入字符在程序执行过程中写入到buf2 存储的文件名myoutfile 所指向的文件中。

### lecture04-缓冲区溢出漏洞.pdf / Page 93

堆溢出
特点：发生在动态分配内存阶段，破坏堆结构或对象数据

### lecture04-缓冲区溢出漏洞.pdf / Page 94

堆溢出
特点：发生在动态分配内存阶段，破坏堆结构或对象数据
覆盖相邻对象

### lecture04-缓冲区溢出漏洞.pdf / Page 95

堆溢出
特点：发生在动态分配内存阶段，破坏堆结构或对象数据
覆盖相邻对象

相邻对象：普通数据、函数指针、C++虚表、堆元数据

### lecture04-缓冲区溢出漏洞.pdf / Page 96

堆溢出
特点：发生在动态分配内存阶段，破坏堆结构或对象数据
覆盖相邻对象

相邻对象：普通数据、函数指针、C++虚表、堆元数据

### lecture04-缓冲区溢出漏洞.pdf / Page 97

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];

else

char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);

fd=fopen(buf2,"a");
if(fd==NULL) {
fprintf(stderr,"%s 打开错误\n",buf2);
if(diff<=strlen(bufchar))
printf("提示:buf1内存溢出!\n");
getchar();
exit(1);
}
fprintf(fd,"%s\n\n",buf1);
fclose(fd);
if(diff<=strlen(bufchar)){
printf("提示:buf1已溢出，溢出部分覆盖buf2中的
myoutfile\n");
getchar();
return 0;
}

diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

if(argc<2){
printf("请输入要写入文件%s的字符串:\n",buf2);
gets(bufchar);
strcpy(buf1,bufchar);
}

strcpy(buf1,argv[1]);

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);

### lecture04-缓冲区溢出漏洞.pdf / Page 98

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];
char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);
diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);

### lecture04-缓冲区溢出漏洞.pdf / Page 99

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];
char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);
diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

if(argc<2){
printf("请输入要写入文件%s的字符串:\n",buf2);
gets(bufchar);
strcpy(buf1,bufchar);
}

### lecture04-缓冲区溢出漏洞.pdf / Page 100

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];
char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);
diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

if(argc<2){
printf("请输入要写入文件%s的字符串:\n",buf2);
gets(bufchar);
strcpy(buf1,bufchar);
}

else

strcpy(buf1,argv[1]);

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);

### lecture04-缓冲区溢出漏洞.pdf / Page 101

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];

else

char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);

fd=fopen(buf2,"a");
if(fd==NULL) {
fprintf(stderr,"%s 打开错误\n",buf2);
if(diff<=strlen(bufchar))
printf("提示:buf1内存溢出!\n");
getchar();
exit(1);
}
fprintf(fd,"%s\n\n",buf1);
fclose(fd);
if(diff<=strlen(bufchar)){
printf("提示:buf1已溢出，溢出部分覆盖buf2中的
myoutfile\n");
getchar();
return 0;
}

diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

if(argc<2){
printf("请输入要写入文件%s的字符串:\n",buf2);
gets(bufchar);
strcpy(buf1,bufchar);
}

strcpy(buf1,argv[1]);

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);

### lecture04-缓冲区溢出漏洞.pdf / Page 102

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];

else

char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);

fd=fopen(buf2,"a");
if(fd==NULL) {
fprintf(stderr,"%s 打开错误\n",buf2);
if(diff<=strlen(bufchar))
printf("提示:buf1内存溢出!\n");
getchar();
exit(1);
}
fprintf(fd,"%s\n\n",buf1);
fclose(fd);
if(diff<=strlen(bufchar)){
printf("提示:buf1已溢出，溢出部分覆盖buf2中的
myoutfile\n");
getchar();
return 0;
}

diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

if(argc<2){
printf("请输入要写入文件%s的字符串:\n",buf2);
gets(bufchar);
strcpy(buf1,bufchar);
}

strcpy(buf1,argv[1]);

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);

### lecture04-缓冲区溢出漏洞.pdf / Page 103

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];
char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);

else

strcpy(buf1,argv[1]);

buf1复制无有
边界检查

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);
20字节堆空间

diff = (long)buf2-(long)buf1;
strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

if(argc<2){
printf("请输入要写入文件%s的字符串
:\n",buf2);
buf1复制无边
gets(bufchar);
界检查
strcpy(buf1,bufchar);
}

fd=fopen(buf2,"a");
if(fd==NULL) {
fprintf(stderr,"%s 打开错误\n",buf2);
if(diff<=strlen(bufchar))
printf("提示:buf1内存溢出!\n");
getchar();
exit(1);
}
fprintf(fd,"%s\n\n",buf1);
fclose(fd);
if(diff<=strlen(bufchar)){
printf("提示:buf1已溢出，溢出部分覆盖buf2中的
myoutfile\n");
getchar();
return 0;
}

### lecture04-缓冲区溢出漏洞.pdf / Page 105

1. 为什么有间隔？
2. 间隔距离如何得知？

### lecture04-缓冲区溢出漏洞.pdf / Page 106

1. 为什么有间隔？
2. 间隔距离如何得知？

1. 堆地址不连续
2. 程序在线打印

### lecture04-缓冲区溢出漏洞.pdf / Page 107

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];

else

char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);

fd=fopen(buf2,"a");
if(fd==NULL) {
fprintf(stderr,"%s 打开错误\n",buf2);
if(diff<=strlen(bufchar))
printf("提示:buf1内存溢出!\n");
getchar();
exit(1);
}
fprintf(fd,"%s\n\n",buf1);
fclose(fd);
if(diff<=strlen(bufchar)){
printf("提示:buf1已溢出，溢出部分覆盖buf2中的
myoutfile\n");
getchar();
return 0;
}

diff = (long)buf2-(long)buf1;

strcpy(buf1,argv[1]);

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);

1

strcpy(buf2,FILENAME);
printf("buf1存储地址:%p\n",buf1);
printf("buf2存储地址:%p,存储内容为文件
名:%s\n",buf2,buf2);
printf("两个地址之间的距离:%d个字节 \n",diff);

2

if(argc<2){
printf("请输入要写入文件%s的字符串:\n",buf2);
gets(bufchar);
strcpy(buf1,bufchar);
}

### lecture04-缓冲区溢出漏洞.pdf / Page 109

文件名更改了！

### lecture04-缓冲区溢出漏洞.pdf / Page 110

#define FILENAME "myoutfile"
int main(int argc,char *argv[])
{
FILE *fd;
long diff;
char bufchar[100];
char* buf1 = (char*)malloc(20);
char* buf2 = (char*)malloc(20);

else

strcpy(buf1,argv[1]);

printf("buf1存储内容:%s \n",buf1);
printf("buf2存储内容:%s \n",buf2);
printf("将%s\n写入文件 %s中\n\n",buf1,buf2);

fd=fopen(buf2,"a");
if(fd==NULL) {
fprintf(stderr,"%s 打开错误\n",buf2);
堆溢出为目标文件
diff = (long)buf2-(long)buf1;
if(diff<=strlen(bufchar))
（“hostility”）写入内容
printf("提示:buf1内存溢出!\n");
strcpy(buf2,FILENAME);
getchar();
exit(1);
printf("buf1存储地址:%p\n",buf1);
}
printf("buf2存储地址:%p,存储内容为文件
fprintf(fd,"%s\n\n",buf1);
名:%s\n",buf2,buf2);
fclose(fd);
printf("两个地址之间的距离:%d个字节 \n",diff);
if(diff<=strlen(bufchar)){
if(argc<2){
printf("提示:buf1已溢出，溢出部分覆盖buf2中的
printf("请输入要写入文件%s的字符串:\n",buf2); myoutfile\n");
getchar();
gets(bufchar);
return 0;
strcpy(buf1,bufchar);
}
}

### lecture04-缓冲区溢出漏洞.pdf / Page 111

只是改个文件名，为什么不安全？

### lecture04-缓冲区溢出漏洞.pdf / Page 112

只是改个文件名，为什么不安全？

• 覆盖关键系统文件
• 写入配置文件
• 日志污染
• 路径穿越

### lecture04-缓冲区溢出漏洞.pdf / Page 113

只是改个文件名，为什么不安全？

• 覆盖关键系统文件
• 写入配置文件
• 日志污染
• 路径穿越

### lecture04-缓冲区溢出漏洞.pdf / Page 114

只是改个文件名，为什么不安全？

• 覆盖关键系统文件
• 写入配置文件
• 日志污染
• 路径穿越

写入恶意代码
实现定时执行

### lecture04-缓冲区溢出漏洞.pdf / Page 115

只是改个文件名，为什么不安全？

• 覆盖关键系统文件

写入恶意代码

• 写入配置文件

实现定时执行

• 日志污染
• 路径穿越

伪造日志
影响审计，掩盖攻击

### lecture04-缓冲区溢出漏洞.pdf / Page 116

只是改个文件名，为什么不安全？

• 覆盖关键系统文件

写入恶意代码

• 写入配置文件

实现定时执行

• 日志污染
• 路径穿越

伪造日志
影响审计，掩盖攻击

### lecture04-缓冲区溢出漏洞.pdf / Page 117

堆溢出的挑战

buf1与buf2的地址差可以提前知道吗？

### lecture04-缓冲区溢出漏洞.pdf / Page 118

堆溢出的挑战

buf1与buf2的地址差可以提前知道吗？

不能

### lecture04-缓冲区溢出漏洞.pdf / Page 119

堆溢出的挑战

buf1与buf2的地址差可以提前知道吗？

不能
1. 离线分析：拿到程序本地运行预测diff
2. 经验推测+喷射（尝试：64/72/80等）

### lecture04-缓冲区溢出漏洞.pdf / Page 120

堆溢出的挑战

buf1与buf2的地址差可以提前知道吗？

不能
1. 离线分析：拿到程序本地运行预测diff
2. 经验推测+喷射（尝试：64/72/80等）
堆chunk对齐机制；“固定”分配模式

### lecture04-缓冲区溢出漏洞.pdf / Page 121

2. 堆溢出漏洞利用

相比于栈溢出，堆溢出的实现难度更大，而且往往要求进程在
内存中具备特定的组织结构。然而，堆溢出攻击也已经成为缓
冲区溢出攻击的主要方式之一。堆溢出带来的威胁远远不止上
面示例演示的那样，结合堆管理结构，
堆溢出漏洞可以在任意位置写入任意数据。
回顾一下第二章介绍的堆管理结构：在Windows系统中，占有态的堆块被使用它
的程序索引，而堆表只索引所有空闲态的堆块。其中，最重要的堆表有两种：空
闲双向链表freelist（简称空表）和快速单向链表lookaside（简称快表）。

### lecture04-缓冲区溢出漏洞.pdf / Page 122

堆块三类操作：堆块分配、堆块释放和堆块合并，归根结底是对空表链的修改。
这些修改无外乎要向链表里链入和卸下堆块。根据对链表操作的常识，我们可
以知道，从链表上卸载（unlink）一个节点的时候会发生如下操作：

node—>blink—>flink = node—>flink ;
node—>flink—>blink = node—>blink ;

blink

Node-blink

flink

blink node

flink

blink

Node-flink

flink

### lecture04-缓冲区溢出漏洞.pdf / Page 123

具体的，在Windows堆内存分配时会调用函数RtlAllocHeap，该函数从空闲
堆链上摘下一空闲堆块，完成双向链表里相关节点的前后指针的变更操作，它
会执行如下操作：

mov dword ptr [edi], ecx ;
mov dword ptr [ecx+4], edi ;
其中ecx为空闲可分配的堆区块的前向指针，edi为该堆区块的后向指针。这
两条汇编语句恰好对应了上述两个链表卸载节点对应的前后向指针变化的操作。
空闲堆块的前向指针（数值）写入到
空闲堆块的后向指针（地址）里去

### lecture04-缓冲区溢出漏洞.pdf / Page 124

Dword Shoot攻击
如果我们通过堆溢出覆写了一个空闲堆块的块首的前向指针flink和后向指针blink，
我们可以精心构造一个地址和一个数据，当这个空闲堆块从链表里卸下的时候，
就获得一次向内存构造的任意地址写入一个任意数据的机会。这种能够向内存任
意位置写入任意数据的机会称为“Arbitrary Dword Reset”（又称Dword
Shoot）。具体如下图所示。

!"#$B&
'(

!"#$%&

F*

!"#$%'

+,-./&0
1234#
$56

!"#$%(

7,-./&0
1F*#$
489

基于Dword Shoot攻击，攻击者甚
至可以劫持进程，运行植入的恶意
代码。比如，当构造的地址为重要
函数调用地址、栈帧中函数返回地
址、栈帧中SEH的句柄等时，写入的
任意数据可能就是恶意代码的入口
地址。

### lecture04-缓冲区溢出漏洞.pdf / Page 125

堆溢出漏洞示例：以下列程序为例，演示堆块分配过程中潜在的Dword
Shoot攻击。
实验环境：VC6.0、Windows XP SP3、Debug模式。
在讲这个实验之前，先介绍一下Windows的堆使用。
在Windows里，可以使用Windows缺省堆，也可以用户自己创建新堆：
p获取缺省堆可以通过GetProcessHeap函数（无参数）得到句柄；
p创建新堆可以用HeapCreat函数。
p除了malloc、new等函数外，C/C++也提供了HeapAlloc、HeapFree
等函数用于堆的分配和释放。

### lecture04-缓冲区溢出漏洞.pdf / Page 126

#include <windows.h>
main()
{
HLOCAL h1, h2,h3,h4,h5,h6;
HANDLE hp;
hp = HeapCreate(0,0x1000,0x10000); //创建自主管理的堆
h1 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);//从堆里申请空间
h2 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h3 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h4 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h5 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h6 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);

}

整个流程解析：
ü 程序首先创建了一个大小为 0x1000 的堆
区，并从其中连续申请了6个块身大小为
8 字节的堆块，加上块首实际上是6个16
字节的堆块。
ü 释放奇数次申请的堆块是为了防止堆块合
并的发生。
ü 三次释放结束后，会形成三个16个字节的
空闲堆块放入空表。因为是16个字节，所
以会被依次放入freelist[2]所标识的空表，

_asm int 3 //手动增加int3中断指令，会让调试器在此处中断
//依次释放奇数堆块，避免堆块合并
HeapFree(hp,0,h1); //释放堆块
HeapFree(hp,0,h3);
HeapFree(hp,0,h5); //现在freelist[2]有3个元素

ü 再次申请8字节的堆区内存，加上块首是

h1 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);

ü 如果我们手动修改h1块首中的前后向指针，

return 0;

它们依次是h1、h3、h5。
16个字节，因此会从freelist[2]所标识的
空表中摘取第一个空闲堆块出来，即h1。
能够观察到 DWORD SHOOT 的发生。

### lecture04-缓冲区溢出漏洞.pdf / Page 127

#include <windows.h>
main()
{
HLOCAL h1, h2,h3,h4,h5,h6;
HANDLE hp;
hp = HeapCreate(0,0x1000,0x10000); //创建自主管理的堆
h1 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);//从堆里申请空间
h2 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h3 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h4 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h5 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
h6 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
_asm int 3 //手动增加int3中断指令，会让调试器在此处中断
//依次释放奇数堆块，避免堆块合并
HeapFree(hp,0,h1); //释放堆块
HeapFree(hp,0,h3);
HeapFree(hp,0,h5); //现在freelist[2]有3个元素
h1 = HeapAlloc(hp,HEAP_ZERO_MEMORY,8);
}

return 0;

hp为0x003a0000, h1为0x003a0688

h1将被链入到freelist[2]空表中，而此时Flink和Blink
的值都是0x003a0198，也是freelist[2]的地址。
转到0x003a0198处，观察内存为：

freelist[2]链表状态为：freelist[2]<=>h1<=>h3<=>h5
摘走h1之后，内存变为：
p freelist[2]（地址为0x003a0198）的前4个字节
变为0x003a06c8
p h3（地址为0x003a06c8）的Blink变为h1->Blink，
即0x003a0198
假设在执行该语句之前，h1的Flink和Blink被改写为特
定地址和特定数值，那么就完成一次Dword Shoot攻击

### lecture04-缓冲区溢出漏洞.pdf / Page 128

单选题

1分

DWORD SHOOT攻击是指

A

能够读取任意位置内存数据的攻击

B

能够向任意位置内存写入任意数据的攻击

C

能够向任意位置内存写入任意数据、并能读取任意位置内存
数据的攻击

D

以上都不对
提交

### lecture04-缓冲区溢出漏洞.pdf / Page 129

知识点四：其它溢出漏洞

### lecture04-缓冲区溢出漏洞.pdf / Page 130

1

SEH结构溢出

异常处理结构体SEH
Windows异常处理机制的重要数据结构

### lecture04-缓冲区溢出漏洞.pdf / Page 131

1

SEH结构溢出

异常处理结构体SEH
Windows异常处理机制的重要数据结构

链式结构

线程环境块
（Thread Environment Block，TEB）

异常处理部分

### lecture04-缓冲区溢出漏洞.pdf / Page 132

1

SEH结构溢出

异常处理结构体SEH
Windows异常处理机制的重要数据结构

链式结构

线程环境块
（Thread Environment Block，TEB）

异常处理部分

### lecture04-缓冲区溢出漏洞.pdf / Page 133

1

SEH结构溢出

SEH：Structured Exception Handler
SEH结构
struct SEH {

_try {

SEH* next; //nSEH

//可能存在异常的代码

handler_func //SEH

} _except(…) {
//异常处理
}

SEH

}

### lecture04-缓冲区溢出漏洞.pdf / Page 134

1

SEH：Structured Exception Handler

SEH结构溢出

SEH结构
struct SEH {

_try {

SEH* next; //nSEH

//可能存在异常的代码

handler_func //SEH

} _except(…) {
//异常处理

SEH

}

}
_try { // SEH1

多个SEH?
可能会

_try { // SEH2
crash();
} _except { ... }
} _except { ... }

### lecture04-缓冲区溢出漏洞.pdf / Page 135

1

SEH结构溢出

### lecture04-缓冲区溢出漏洞.pdf / Page 136

1

SEH结构溢出
恶意代码

shellcode
buffer
溢出覆盖

### lecture04-缓冲区溢出漏洞.pdf / Page 137

1

SEH结构溢出
恶意代码

shellcode
buffer
溢出覆盖

### lecture04-缓冲区溢出漏洞.pdf / Page 138

1

SEH结构溢出

### lecture04-缓冲区溢出漏洞.pdf / Page 139

1

SEH结构溢出
恶意代码

shellcode
buffer
溢出覆盖

操控ESP，调整栈
跳到nSEH

### lecture04-缓冲区溢出漏洞.pdf / Page 140

1

SEH结构溢出
恶意代码

shellcode

ESP
ESP

两次pop
1

例如，pop edi; pop ebp

### lecture04-缓冲区溢出漏洞.pdf / Page 141

1

SEH结构溢出

2

一次ret

恶意代码

取址执行shellcode
shellcode

ESP
ESP

两次pop
1

例如，pop edi; pop ebp

### lecture04-缓冲区溢出漏洞.pdf / Page 142

1

SEH结构溢出

2

一次ret

恶意代码

取址执行shellcode
shellcode

ESP
ESP

两次pop

例如，pop edi; pop ebp

1

成功了执行恶意代码！

### lecture04-缓冲区溢出漏洞.pdf / Page 143

1

SEH结构溢出

2

一次ret

恶意代码

取址执行shellcode
shellcode

ESP
ESP

两次pop

例如，pop edi; pop ebp

1

“pop pop ret”从哪来？

### lecture04-缓冲区溢出漏洞.pdf / Page 144

1

SEH结构溢出

2

一次ret

恶意代码

取址执行shellcode
shellcode

ESP
ESP

两次pop

例如，pop edi; pop ebp

1

“pop pop ret”从哪来？
来自程序已有指令

Code Reuse
攻击思想！

### lecture04-缓冲区溢出漏洞.pdf / Page 145

2

单字节溢出

单字节溢出：缓冲区仅溢出一个字节
void single_func(char *src){
char buf[256];
int i;
for(i = 0;i <= 256;i++)
buf[i] = src[i];
}

### lecture04-缓冲区溢出漏洞.pdf / Page 146

2

单字节溢出

单字节溢出：缓冲区仅溢出一个字节
void single_func(char *src){
char buf[256];
int i;
for(i = 0;i <= 256;i++)
buf[i] = src[i];
}

此例中溢出覆盖了什么？

### lecture04-缓冲区溢出漏洞.pdf / Page 147

2

单字节溢出

覆盖EBP（栈基址）

### lecture04-缓冲区溢出漏洞.pdf / Page 148

2

单字节溢出

覆盖EBP（栈基址）

为什么这样也危险？

### lecture04-缓冲区溢出漏洞.pdf / Page 149

2

单字节溢出

esp被修改
1

函数返回时
2

控制流被劫持！

### lecture04-缓冲区溢出漏洞.pdf / Page 150

单选题

SEH结构存在于

A

堆区

B

栈区

C

寄存器

D

代码区
提交

### lecture04-缓冲区溢出漏洞.pdf / Page 151

单选题

SEH结构存在于

A

堆区

B

栈区

C

寄存器

D

代码区
提交

