# Lecture03 软件调试基础 - 习题

> GitHub: https://github.com/raitocc/software-security-notes

> 配套答案：[Lecture03-软件调试基础-习题答案.md](Lecture03-软件调试基础-习题答案.md)

## A. 基础查表题

这一部分主要考“看过课件和手册就应当知道”的概念、缩写、工具和字段。

### 选择题

1. PE 文件格式主要对应的操作系统平台是：

A. Windows 可执行程序  
B. Unix/Linux 可执行程序  
C. Android 字节码程序  
D. WebAssembly 模块

2. 下列哪一项通常用于记录 PE 文件依赖的外部 DLL 和函数信息？

A. `.text`  
B. `.idata`  
C. `.rsrc`  
D. `.reloc`

3. Optional Header 中，和程序入口点位置关系最密切的字段是：

A. `AddressOfEntryPoint`  
B. `ImageBase`  
C. `SectionAlignment`  
D. `NumberOfSections`

4. IAT 的全称是：

A. Internal Access Table  
B. Import Address Table  
C. Import Alignment Table  
D. Image Address Table

5. 下列工具中，更偏向 32 位程序动态调试的是：

A. OllyDbg  
B. x64dbg  
C. IDA Pro  
D. GDB

6. 在 IDA 中，用于查看二进制文件中字符串并跳转到引用位置的窗口是：

A. Strings Window  
B. Names Window  
C. Functions Window  
D. Imports Window

7. OllyDbg 中，单步步入和单步步过对应的快捷键分别是：

A. `F7`、`F8`  
B. `F8`、`F7`  
C. `F7`、`F9`  
D. `F8`、`Ctrl+F9`

8. 下列关于 `MessageBoxA` 的说法，和课件注入示例最贴近的是：

A. 它是实验中注入代码调用的 Windows API  
B. 它是注入后用于恢复原入口点的跳转地址  
C. 它是 Loader 解析导入表时写入的节名  
D. 它是 IDA 中定位字符串引用的窗口名称

### 判断题

9. PE 文件中可以包含图标、菜单、位图等资源信息。

10. 用户态调试器中看到的程序地址通常是虚拟地址。

11. IDA 的 Graph View 会把函数拆成基本块并显示控制流关系。

12. 在 PE 注入示例中，写入注入代码后，还需要修改入口点或跳转逻辑，让程序控制流到达这段代码。

### 填空题

13. PE 的全称是 `__________`。

14. ELF 的全称是 `__________`。

15. 公式：`VA = __________ + RVA`。

16. IDA 中按 `__________` 可以尝试生成 C/C++ 风格伪代码。

17. `XOR EAX, EAX` 执行后，`EAX = __________`。

## B. 理解应用题

这一部分要求你不仅记住名词，还要知道它们在加载、调试和分析时怎么互相配合。

### 选择题

18. Loader 解析 PE 文件时，较合理的流程是：

A. 读取 PE 头和节表，映射 section，解析导入表，最后跳转入口点  
B. 读取入口点字节，按文件偏移映射 section，再根据导入表修正 ImageBase  
C. 先解析资源节和字符串表，再根据资源目录决定程序入口点  
D. 根据节表权限直接执行 `.text` 文件偏移 0 处的指令，再补充解析导入表

19. IAT 之所以有必要，主要是因为：

A. 外部 DLL 函数的真实地址通常需要运行时解析  
B. PE 文件需要把资源、节表和调试符号统一映射到虚拟地址  
C. 用户态调试器通过 IAT 把虚拟地址转换为物理地址  
D. `.text` 节中的内部函数调用需要 IAT 保存返回地址

20. 关于加壳程序，下列说法更合理的是：

A. 壳代码通常会先于原始程序逻辑执行，并在内存中还原或准备原程序  
B. 壳代码通常替换导入表内容，让 Loader 不再解析外部 DLL  
C. 加壳通常把程序资源节移到文件开头，以便调试器优先显示  
D. 加壳主要调整节表对齐方式，不改变入口点附近执行逻辑

21. 文件偏移和 RVA 容易不同，主要原因是：

A. 磁盘文件和内存映射采用的对齐粒度可能不同  
B. Loader 会把文件偏移直接加上 ImageBase，并按导入表重新排序节区  
C. PE 文件中的资源节按虚拟地址保存，而代码节按物理地址保存  
D. 调试器显示的是物理页偏移，而文件中保存的是虚拟页号

22. 在 IDA 文本视图中，若某条跳转以虚线箭头显示，较可能表示：

A. 条件跳转  
B. 函数调用返回  
C. 导入函数跳转  
D. 交叉引用入口

23. OllyDbg 中使用“查找所有引用的字符串”定位 `wrong password`，主要是为了：

A. 从可见输出反推关键分支附近的代码位置  
B. 从导入函数地址反推 Loader 写入 IAT 的过程  
C. 从节表偏移反推出 `.rsrc` 与 `.text` 的映射关系  
D. 从入口点地址反推出程序是否经过加壳处理

### 判断题

24. IAT 中填入的导入函数地址，和 DLL 实际加载位置有关。

25. 文件中的 section 较紧凑，而内存中的 section 往往要按页或较大粒度对齐。

26. ASLR 改变的是地址布局，程序逻辑本身仍由原来的指令决定。

27. IDA 的反编译伪代码有助于理解逻辑，但分析关键分支时仍应结合汇编。

### 填空题

28. 已知 `ImageBase = 0x00400000`，`RVA = 0x00001234`，则 `VA = __________`。

29. LordPE 中，`VOffset` 通常表示 `__________`，`ROffset` 通常表示 `__________`。

30. 在 OllyDbg 中，设置断点的快捷键是 `__________`，运行程序的快捷键是 `__________`。

31. `CMP EAX, EAX; SETE AL` 执行后，`AL = __________`。

### 简答题

32. 简述 PE 文件为什么不能被理解成“一串纯机器码”。

33. 简述 IAT 在动态链接中的作用。

34. 为什么用户态调试器看到的通常是虚拟地址，而不是物理地址？

## C. 综合融会题

这一部分更接近考试中的代码、流程、地址换算和分析题，需要把多个知识点串起来。

### 综合题 1：地址换算

某 PE 文件信息如下：

- `ImageBase = 0x00400000`
- `.text` 节 `VOffset = 0x00011000`
- `.text` 节 `ROffset = 0x00001000`

请回答：

35. `.text` 节加载到内存后的起始 VA 是多少？
36. `VOffset` 和 `ROffset` 分别表示什么？
37. 为什么这两个值通常不相等？

### 综合题 2：PE 注入流程

某程序原入口点：

- `ImageBase = 0x01000000`
- 原入口点 `RVA = 0x00003E21`

你在空白代码区写入如下逻辑：

```asm
push 0
push title_addr
push text_addr
push 0
call MessageBoxA
jmp 0x01003E21
```

注入代码起始地址为 `VA = 0x01004ABA`。

请回答：

38. 原入口点 VA 是多少？
39. 修改 PE 入口点时应填写的新 RVA 是多少？
40. `jmp 0x01003E21` 的作用是什么？
41. 为什么 `call MessageBoxA` 能被解析到正确函数？

### 综合题 3：破解逻辑

某验证函数逻辑如下：

```c
flag = strcmp(password, input);
return flag == 0;
```

反汇编中出现：

```asm
xor eax, eax
cmp dword ptr [ebp-8], 0
sete al
```

请回答：

42. `xor eax, eax` 的作用是什么？
43. `cmp dword ptr [ebp-8], 0` 比较的是什么？
44. `sete al` 在什么条件下把 `AL` 置为 1？
45. 如果想让函数总是返回 true，可以从“修改返回值”的角度如何改？

### 综合题 4：分析路线

你拿到一个不知道源码的 Windows 程序，运行后输入错误密码会显示 `wrong password`。你想定位验证逻辑。

请回答：

46. 如果使用 OllyDbg，可以怎样利用这个字符串定位关键分支？
47. 定位到分支附近后，为什么要重点观察 `cmp/test` 和 `jz/jnz/je/jne`？
48. 如果改条件跳转和改函数返回值都能让程序通过验证，它们的区别是什么？
