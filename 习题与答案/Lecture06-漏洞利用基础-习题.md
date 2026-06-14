# Lecture06 漏洞利用基础 - 习题

> GitHub: https://github.com/raitocc/software-security-notes

> 配套答案：[Lecture06-漏洞利用基础-习题答案.md](Lecture06-漏洞利用基础-习题答案.md)

## A. 基础查表题

这一部分主要考 exploit、payload、shellcode、代码植入和 shellcode 编码的基础关系。

### 选择题

1. 在课程语境中，exploit 更接近下列哪种含义？C

A. payload 中用于执行功能的一段机器码  
B. 系统用于记录漏洞编号的数据库条目  
C. 触发漏洞并尝试达成攻击目标的利用方案  
D. 编译器插入的栈溢出检测代码

2. 关于 payload 和 shellcode 的关系，下列说法更合理的是：D

A. payload 主要用于描述漏洞编号和影响版本范围  
B. shellcode 主要指厂商发布补丁后的验证脚本  
C. payload 通常是操作系统用于加载 DLL 的结构体  
D. shellcode 可以是 payload 中完成具体功能的机器码片段

3. 下列哪项更接近 shellcode 编码的常见目的？B

A. 让栈帧中的返回地址恢复成原始地址  
B. 避开坏字符、字符集限制或简单特征检测  
C. 把 PE 文件中的资源节转换成导入表  
D. 自动修复程序中的越界写漏洞

4. `MessageBoxA(hWnd, lpText, lpCaption, uType)` 在 32 位常见调用约定下，参数压栈顺序更接近：C

A. `hWnd`、`lpText`、`lpCaption`、`uType`  
B. `lpCaption`、`lpText`、`hWnd`、`uType`  
C. `uType`、`lpCaption`、`lpText`、`hWnd`  
D. `lpText`、`lpCaption`、`uType`、`hWnd`

5. 在覆盖相邻变量示例中，字符 `'0'` 与数值 0 的关系更接近：D

A. 字符 `'0'` 写入内存后会自动转换成整数 0  
B. 四个字符 `'0'` 在内存中通常表示 `0x00000000`  
C. 字符 `'0'` 会让字符串结束符提前出现，停止后续拷贝  
D. 字符 `'0'` 的字节值是 `0x30`，不等于整数 0

6. 在 Windows shellcode 中，API 函数自搜索主要解决的问题是：B

A. shellcode 需要把字符串都转换成 Unicode 显示  
B. 不同系统或 ASLR 下 API 入口地址可能变化  
C. 函数调用时无法使用栈传递参数  
D. DEP 会删除 DLL 导出表中的函数名

### 判断题

7. 有漏洞不代表已经存在可用 exploit，但 exploit 通常依赖某种漏洞或弱点。T

8. Shellcode 这个名字来自开 shell 的历史场景，但现代语境中不一定真的打开命令行 shell。T

9. 传统代码植入要同时满足可写入代码、可劫持控制流和目标内存可执行等条件。T

10. `strcpy` 遇到第一个 `\0` 会停止拷贝，这会影响通过字符串写入多个 NUL 字节的思路。T

### 填空题

11. `MessageBoxA` 所在的 Windows GUI 库通常是 `__________`。

12. 动态获取 API 地址时，常见组合是 `LoadLibrary` 和 `__________`。

13. NOP sled 的作用是提高跳转到 shellcode 附近时的 `__________`。

14. Base64 更常用于网页或文本场景下表示 `__________` 数据。

## B. 理解应用题

这一部分要求你把 exploit 结构、相邻变量覆盖和 API 调用放回实际利用流程中理解。

### 选择题

15. 关于 exploit、payload、shellcode 的边界，下列说法更贴近课程内容：A

A. exploit 负责触发和转移控制，payload 负责效果，shellcode 可作为具体机器码实现  
B. exploit 主要保存 shellcode 编码前后的差异，payload 主要保存漏洞库编号  
C. exploit 与 payload 都是系统 DLL，shellcode 是导出函数名列表  
D. exploit 负责修复漏洞，payload 负责检测漏洞是否已经公开

16. 覆盖注册机示例中的 `flag`，为什么可能绕过验证？C

A. `flag` 保存的是 `MessageBoxA` 地址，覆盖后会自动加载 user32.dll  
B. `flag` 保存的是返回地址附近的旧 EBP，覆盖后会改变栈帧恢复过程  
C. 主程序把 `verify` 返回 0 理解为验证通过，覆盖 `flag` 可改变返回值  
D. `flag` 保存的是输入缓冲区长度，覆盖后会让 `strcpy` 停止拷贝

17. 为什么把 `"westwest"` 内容压栈后，后面还要 `push eax`？D

A. 前面压入的是返回地址，后面压入的是 DLL 导出表地址  
B. 前面压入的是 API 地址，后面压入的是 shellcode 编码 key  
C. 前面压入的是函数名，后面压入的是 PE 文件节表地址  
D. 前面压入的是字符串内容，后面压入的是字符串首地址作为参数

18. 下列哪项更接近“静态写死 API 地址”的风险？B

A. API 地址写死后，`strcpy` 会自动把地址截断成 1 字节  
B. ASLR、系统版本和补丁差异会改变 API 运行时地址  
C. 写死地址后，参数压栈顺序会从右向左变成从左向右  
D. Windows 会把硬编码地址自动转换成函数名称字符串

19. 关于异或编码 shellcode，下列说法更合理的是：C

A. 编码后 shellcode 就不再需要控制流跳转到它的位置  
B. 异或编码主要用于把 API 地址转换成 DLL 基址  
C. 编码后需要解码器 stub 在运行时还原真正 shellcode  
D. 选取异或 key 时不需要考虑编码结果中的坏字符

### 简答题

20. 为什么说“让程序崩溃”通常还不够构成稳定利用？

21. 覆盖相邻变量和覆盖返回地址在利用目标上有什么区别？

22. Shellcode 为什么常要求位置无关、短小并避开坏字符？

23. 为什么 `GetProcAddress` 比直接写死 `MessageBoxA` 地址更适合通用利用？

## C. 综合融会题

这一部分更接近考试中的代码推导和利用链分析。

### 综合题 1：覆盖相邻变量

某验证函数如下：

```c
int verify(char *code)
{
    int flag;
    char buffer[44];
    flag = strcmp(code, "12345678");
    strcpy(buffer, code);
    return flag;
}
```

24. `strcpy(buffer, code)` 的风险在哪里？
25. 为什么输入 44 字节后，字符串结束符也可能影响 `flag`？
26. 为什么输入四个字符 `'0'` 不能等同于把 `flag` 写成整数 0？
27. 为什么 `strcmp` 返回值不是 1 时，只覆盖低字节可能失效？

### 综合题 2：代码植入偏移

某栈布局近似如下：

```text
buffer[44]
flag[4]
old EBP[4]
return address[4]
```

28. 覆盖返回地址前，需要先写过哪些区域？
29. 返回地址对应的 4 字节大致位于输入的第几字节到第几字节？
30. 如果返回地址被改成 buffer 起始地址，函数返回时可能发生什么？

### 综合题 3：MessageBoxA shellcode

某 shellcode 片段意图显示 `"westwest"`：

```asm
xor ebx, ebx
push ebx
push 74736577h
push 74736577h
mov eax, esp
push ebx
push eax
push eax
push ebx
mov eax, MessageBoxA_addr
call eax
```

31. `xor ebx, ebx` 的作用是什么？
32. `mov eax, esp` 后，`EAX` 保存的是什么？
33. 四次压入参数分别对应 `MessageBoxA` 的哪些参数？
34. 为什么 `MessageBoxA_addr` 在不同系统中可能不同？
