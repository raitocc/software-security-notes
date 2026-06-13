# Lecture06 漏洞利用基础 - 复习手册

> 本文件基于课程 PDF 整理，用于考前复习和人工复核。

### 本讲主线

Lecture06 讲 exploit、payload、shellcode 的关系，以及覆盖相邻变量、代码植入、shellcode 编写和编码。它把“漏洞存在”推进到“漏洞如何被利用”。

### 漏洞、利用、破坏

有漏洞不一定有 exploit，但有 exploit 一定对应某种漏洞。

| 概念 | 含义 |
|---|---|
| Vulnerability | 可被利用的弱点 |
| Exploit | 触发漏洞并达成攻击目标的技术方案或动作 |
| Payload | 攻击载荷，完成攻击效果的整体内容 |
| Shellcode | payload 中可能包含的一段机器码，负责具体功能 |

触发漏洞、将控制权转移到目标程序/目标代码的是 exploit。

这几个词可以放到同一条攻击链里理解：程序中先有 Vulnerability；攻击者写出或使用 Exploit 去触发它；触发成功后执行 Payload；在二进制漏洞里，Payload 里常包含 Shellcode。不要把 exploit 和 shellcode 混成一个东西。SQL 注入、反序列化、逻辑越权也可以有 exploit，但它们不一定有传统 shellcode。

### Shellcode

Shellcode 原意与“获得 shell”有关，但现代语境中不一定真的开 shell。任何以注入方式获得非法权限或执行特定攻击功能的机器码，都可称为 shellcode。

漏洞利用的核心是控制流劫持：利用程序漏洞改变程序执行路径，让程序执行攻击者希望执行的代码或代码片段。

Shellcode 之所以难写，是因为它不像普通程序那样拥有完整运行环境。普通程序可以靠导入表、链接器和运行库找到 API；shellcode 常常只是一串被塞进缓冲区的机器码，必须尽量短、位置无关，还要避开会截断字符串或被过滤的坏字符。

### Exploit 的结构

Exploit 不只是 shellcode。它通常还要负责：

- 构造能触发漏洞的输入。
- 覆盖关键控制数据。
- 计算或猜测跳转地址。
- 绕过坏字符、长度限制、ASLR、DEP 等限制。
- 把控制流转移到 payload 或 shellcode。

导弹类比：

- exploit 像攻击方案。
- payload 像有效载荷。
- shellcode 像载荷里真正执行功能的一段机器码。

一个典型早期栈溢出 exploit 输入可以拆成：

```text
[填充数据] [覆盖旧 EBP] [覆盖返回地址] [NOP sled] [shellcode]
```

填充数据用于到达目标偏移；覆盖返回地址用于让 `ret` 跳到可控区域；NOP sled 用来提高跳转容错率；shellcode 才负责真正执行 payload。现代系统启用 DEP/ASLR/Canary 后，这个结构会变复杂，例如把 shellcode 换成 ROP chain，或先通过信息泄漏计算地址。

### 覆盖相邻变量

不是所有利用一开始都要覆盖返回地址。若缓冲区旁边有权限标志、认证结果、函数指针、路径变量等关键数据，溢出也可以通过覆盖相邻变量改变程序逻辑。

理解重点：内存越界写可以先破坏“数据流”，再进一步破坏“控制流”。

覆盖相邻变量的题通常不需要你想 shellcode，而是要你算偏移。例如 `char buffer[8]` 后面紧跟 `int authenticated`，那么写满 8 字节后，下一个字节就可能开始影响 `authenticated`。如果输入函数会自动写入字符串结束符 `\0`，这个结束符也算一次写入，课后题经常借它考“至少多少字节开始覆盖相邻变量”。

#### 注册机验证示例

PPT 中用一个注册机验证过程说明“覆盖相邻变量”：

```c
#define REGCODE "12345678"

int verify(char *code)
{
    int flag;
    char buffer[44];
    flag = strcmp(code, REGCODE);
    strcpy(buffer, code);
    return flag;
}
```

主程序大致逻辑是从 `reg.txt` 读入注册码，调用 `verify(regcode)`，如果返回值 `vFlag` 为 0 就显示 `passed!`，否则显示 `wrong regcode!`。正常情况下，`strcmp` 返回 0 表示字符串相等；非 0 表示不相等。

这段代码的问题是：`buffer` 只有 44 字节，但 `strcpy(buffer, code)` 不检查输入长度。如果栈布局中 `buffer` 后面紧邻 `flag`，攻击者就可能通过超长输入覆盖 `flag`。这样即使 `strcmp` 判断失败，只要最后 `flag` 被改成 0，`verify` 仍会返回 0，主程序就会认为验证通过。

这个例子有几个很容易考的细节：

1. 覆盖目标不是返回地址，而是 `flag` 这个认证结果变量。
2. 目标值是数值 0，不是字符 `'0'`。
3. 字符 `'0'` 的 ASCII 是 `0x30`，四个字符 `'0'` 会形成 `0x30303030`，不是整数 0。
4. `strcmp` 失败时返回值可能是正数或负数，不一定恰好是 1。
5. 想通过字符串写入四个 `\0` 也有问题，因为 `strcpy` 遇到第一个 `\0` 就停止拷贝。

PPT 里提到的字符串：

```text
"12341234123412341234123412341234123412341234"
```

它刚好用 44 字节写满 `buffer`。随后字符串结束符 `\0` 会额外写入下一个字节，可能覆盖 `flag` 的低字节。如果 `flag` 原本是 1，低字节被写成 0 后可能变成 0；但如果 `strcmp` 返回值不是 1，这种“只覆盖低字节”的方式就未必能让整个 `flag` 变成 0。这就是该例子的思考点：利用不仅要知道“能覆盖”，还要知道“覆盖成什么值、覆盖多少字节、字符串函数会不会提前停止”。

### 代码植入

早期典型思路：

1. 把 shellcode 放入缓冲区或可控内存。
2. 覆盖返回地址或控制数据。
3. 让程序跳转到 shellcode。

现代系统中，这条路会遇到 ASLR、NX/DEP、栈保护等防护。

代码植入要同时满足三个条件：攻击者能把代码写进进程内存，能让控制流跳过去，并且那块内存可执行。DEP/NX 正是卡住第三点：栈和堆即使能写入 shellcode，也不能直接执行。于是攻击者会转向 ret2libc、ROP，或者先调用系统 API 修改内存权限。

#### 覆盖返回地址的偏移

在注册机示例的栈布局里，若想从覆盖 `flag` 进一步变成代码植入，目标就从“改变量”变成“改返回地址”。PPT 给出的推理是：

```text
buffer 44 字节
flag   4 字节
前 EBP 4 字节
返回地址 4 字节
```

如果要覆盖返回地址，前面要先写过 `buffer`、`flag` 和前一个栈帧的 `EBP`。因此覆盖返回地址对应的 4 个字节大致出现在第 53 到 56 字节。这个计算体现了代码植入题的基本方法：先按布局计算偏移，再把返回地址替换成 shellcode 所在地址。

注意：写入缓冲区里的内容不是普通文本。它可以是机器指令字节。程序正常执行时把它当数据；一旦返回地址被改成缓冲区地址，CPU 就会把这些字节当指令取出执行。

#### MessageBoxA 调用示例

PPT 用 `MessageBoxA` 弹窗说明 shellcode 如何调用 Windows API。`MessageBoxA` 位于 `user32.dll` 中，不属于当前程序自己的代码。机器码不能像 C 语言那样直接写：

```c
MessageBoxA(NULL, "hello", "title", 0);
```

它必须完成几件事：

1. 确认 `user32.dll` 已加载，或自己调用 `LoadLibrary` 加载。
2. 获得 `MessageBoxA` 在当前进程中的入口地址。
3. 在栈上构造字符串，例如 `"westwest\0"`。
4. 获取字符串首地址。
5. 按调用约定从右向左压入 4 个参数。
6. 跳转或调用 `MessageBoxA` 的地址。

如果 `MessageBoxA(hWnd, lpText, lpCaption, uType)` 要显示文本和标题都为 `"westwest"`，参数压栈顺序是：

```text
uType -> lpCaption -> lpText -> hWnd
```

课堂汇编里常见这样的结构：

```asm
xor ebx, ebx        ; EBX = 0
push ebx            ; 字符串结尾 0
push 74736577h      ; "west"
push 74736577h      ; "west"
mov eax, esp        ; EAX 保存字符串首地址
push ebx            ; uType = 0
push eax            ; lpCaption = "westwest"
push eax            ; lpText = "westwest"
push ebx            ; hWnd = NULL
mov eax, 77D507EAh  ; MessageBoxA 地址示例
call eax
```

这里有一个容易误解的点：前面 `push 74736577h` 是把字符串内容压到栈里；后面 `push eax` 是把字符串地址作为函数参数压栈。函数调用时并不是去找“刚才压过哪些字符”，而是根据调用约定从栈上取参数，参数里要放字符串首地址。

`MessageBoxA` 地址可由 DLL 加载基址加函数在 DLL 中的偏移得到。例如 PPT 中给过类似计算：

```text
user32.dll 基址 0x77D10000
MessageBoxA 偏移 0x000407EA
入口地址      0x77D507EA
```

也可以在程序中用 `LoadLibrary("user32")` 和 `GetProcAddress(handle, "MessageBoxA")` 动态查询。Windows XP 时代静态地址可能较稳定；后续系统引入 ASLR，不同系统版本和补丁版本也会影响地址，所以 shellcode 不宜长期依赖写死 API 地址。

### Shellcode 编写难点

- 需要汇编和机器码知识。
- 代码长度受限制。
- 不能包含坏字符，例如字符串结束符 `0x00`。
- 地址可能随机化。
- 栈或堆可能不可执行。
- 安全软件可能检测特征。

Shellcode 编码的常见原因：

- 规避字符集限制。
- 绕过坏字符。
- 绕过安全检测。

“绕过返回地址”不是 shellcode 编码的主要原因。

课程中的 `MessageBoxA` 示例常用于说明 shellcode 如何调用系统 API。Windows 32 位常见调用约定下，函数参数通常从右向左压栈。`MessageBoxA(hWnd, lpText, lpCaption, uType)` 的压栈顺序是：

```text
uType -> lpCaption -> lpText -> hWnd
```

如果题目问“按压栈顺序写参数”，不要按函数声明从左到右写。这个点非常适合填空题。

API 地址也是 shellcode 的难点。ASLR 或系统版本差异会让 DLL 加载地址变化，shellcode 不能总是假设函数在固定地址。所谓 API 函数自搜索，就是 shellcode 自己遍历进程中的模块信息，定位 `kernel32.dll` 等模块，再从导出表中找到目标函数地址。

#### 简单 Shellcode 编写流程

PPT 给出了一种从高级代码到 shellcode 的入门路线：

1. 先用 C 语言写出要完成的功能，例如调用 `MessageBox(NULL, NULL, NULL, 0)`。
2. 在调试器中观察对应汇编，或手写等价汇编。
3. 对汇编进行加工，避开坏字符。例如 `push 0` 可能产生 `0x00` 字节，可改成 `xor ebx, ebx; push ebx`。
4. 在内存窗口中提取机器码字节。
5. 把机器码作为字节串放进 exploit 输入，并把返回地址改到 shellcode 地址。

例如下面这段逻辑中，`ret=(int*)&ret+2; (*ret)=(int)ourshellcode;` 的教学目的，是人为修改当前函数返回地址，让函数返回后跳到 `ourshellcode`：

```c
char ourshellcode[] = "\x33\xDB\x53\x53\x53\x53\xB8\xEA\x07\xD5\x77\xFF\xD0";

void main()
{
    LoadLibrary("user32.dll");
    int *ret;
    ret = (int*)&ret + 2;
    (*ret) = (int)ourshellcode;
    return;
}
```

这不是日常编程技巧，而是为了演示“返回地址决定函数返回后去哪里执行”。

构造字符串时还要注意字节序和入栈顺序。例如 `"hello world"` 的 ASCII 字节需要按 4 字节分组压栈；x86 小端存储会让立即数书写顺序和内存中字节顺序看起来相反。PPT 示例中用 `push 6C6C6568h`、`push 6F77206Fh`、`push 20646C72h` 来构造 `"hello world "` 一类字符串，本质就是把文本字节倒序按块压入栈。

#### Shellcode 编码方法

Shellcode 编码的必要性来自三类限制：

- 字符集差异：目标输入字段可能不是任意二进制。
- 坏字符：例如 `0x00`、`0x0D`、`0x0A`、`0x20` 可能被截断、换行、过滤或改写。
- 检测绕过：简单特征检测可能识别常见 exploit 字节序列。

网页 shellcode 常可用 Base64 表示二进制数据。二进制机器码常用自定义编码，例如异或编码、加法编码、简单加解密等。编码后的 payload 通常分成：

```text
解码器 stub + 编码后的 shellcode 主体
```

执行时先运行解码器，在内存中还原真正 shellcode，再跳过去执行。异或编码很常见，但选 key 时要避免把某些字节编码成坏字符。例如某个原始字节与 key 相同，异或结果就是 0，这在 `strcpy` 场景中可能导致字符串提前结束。

### 易考点

- exploit 负责触发漏洞并转移控制权。
- shellcode 不一定开 shell。
- payload 包含攻击效果，shellcode 可能是 payload 的一部分。
- shellcode 编码常为绕过坏字符、字符集和检测。
- `MessageBoxA(hWnd, lpText, lpCaption, uType)` 的压栈顺序是 `uType`、`lpCaption`、`lpText`、`hWnd`。
- NOP sled 用于提高跳转容错率。
- 传统代码植入需要可控内存、可控跳转和可执行权限。
- DEP/NX 阻止栈/堆 shellcode 直接执行，但不阻止复用已有代码。
- API 函数自搜索用于动态定位系统 DLL 和 API 地址。
- 覆盖相邻变量时，字符 `'0'` 不等于数值 0，四个 `'0'` 是 `0x30303030`。
- `strcpy` 遇到第一个 `\0` 会停止，不能简单认为能用字符串写入多个连续 NUL。
- `MessageBoxA` 地址可以由 DLL 基址加函数偏移得到，也可以通过 `GetProcAddress` 动态获取。
- 静态 API 地址会受 ASLR、Windows 版本和补丁版本影响。

### 从漏洞到利用的完整链条

一个漏洞只是“系统中存在可被利用的弱点”，并不自动等于攻击成功。漏洞利用要把弱点转化成可控效果。这个过程通常包含三个问题：

1. 如何触发漏洞？
2. 触发后能控制什么？
3. 控制后如何达成目标？

Exploit 负责回答前两个问题：构造输入、触发错误、覆盖关键数据、改变控制流。Payload 负责回答第三个问题：拿到控制流后要做什么。Shellcode 是 payload 中常见的一段机器码，用来执行具体动作。

可以把漏洞利用理解成一次“程序状态改造”：攻击者先把程序带到一个异常状态，再让这个异常状态变成可控状态，最后让可控状态产生目标效果。只让程序崩溃通常不够；能控制返回地址、函数指针、内存写入位置或解释器语义，才是利用的关键。

### Exploit、Payload、Shellcode 的关系

| 概念 | 关注点 | 例子 |
|---|---|---|
| Exploit | 怎么触发漏洞并转移控制权 | 构造超长输入覆盖返回地址 |
| Payload | 控制权到手后要实现什么效果 | 弹窗、建立会话、读取文件 |
| Shellcode | payload 中的机器码片段 | 调用系统 API 完成特定动作 |

一个 exploit 可以携带不同 payload，同一个 payload 也可能被不同 exploit 使用。Shellcode 不一定真的打开 shell，它只是历史名称。只要是注入并执行的短机器码，都可以被称为 shellcode。

### 覆盖相邻变量

早期学习漏洞利用时容易以为必须覆盖返回地址，其实不一定。程序里很多安全判断依赖普通变量，例如：

- `is_admin` 是否为真。
- `authenticated` 是否为真。
- `price` 或 `quota` 是否被修改。
- 指针是否指向安全对象。

如果溢出能覆盖这些相邻变量，攻击者可能不需要劫持控制流，也能改变程序逻辑。这类利用说明：安全影响不只来自“执行恶意代码”，也可能来自“改变关键数据”。

### 代码植入的基本思想

代码植入型利用的经典思路是：

1. 把 shellcode 放进可控输入或可控内存。
2. 通过漏洞覆盖返回地址、函数指针或其他控制数据。
3. 让程序跳到 shellcode 所在地址。
4. shellcode 调用系统函数完成 payload。

这条路线在早期系统上更直接，因为栈或堆上的数据可能可执行。现代系统启用 DEP/NX 后，栈和堆通常不可执行，直接跳到栈上 shellcode 会被阻止。于是攻击者转向 ROP 等代码复用技术，先修改内存权限或直接复用已有代码。

### Shellcode 编写为什么难

Shellcode 通常要短、小、位置无关，并且避开坏字符。它不像普通程序那样可以依赖完整运行环境和链接器。

常见限制：

- 不能包含 `0x00`，因为字符串函数会把它当作结尾。
- 输入字段长度有限。
- 地址随机化导致绝对地址不可直接使用。
- API 地址需要动态寻找或由 exploit 提供。
- 字符集过滤可能破坏机器码。
- 安全软件可能识别常见字节序列。

因此 shellcode 常写成位置无关代码，尽量通过相对寻址、栈操作和动态解析 API 来工作。课程中的 MessageBox 示例主要是帮助理解“机器码如何被当作数据写入，又如何被转成执行流”。

### Shellcode 编码

编码不是加密课程意义上的保密，而是把 shellcode 变成一种更适合通过输入过滤和检测的形式。运行时再由解码器还原。

编码常用于：

- 消除坏字符。
- 适配字符集限制。
- 绕过简单特征检测。
- 让 payload 能通过某些协议字段或文本字段。

编码后的 payload 通常由两部分组成：解码器 stub 和被编码的主体。程序跳到解码器后，解码器先还原真正 shellcode，再转去执行。

### 现代防护对利用链的影响

| 防护 | 对利用链的影响 |
|---|---|
| Canary | 覆盖返回地址前可能被检测 |
| ASLR | 难以预测 shellcode、库函数、gadget 地址 |
| DEP/NX | 栈/堆上的 shellcode 无法直接执行 |
| SafeSEH/SEHOP | 覆盖异常处理链更难 |
| CFI | 限制间接跳转目标 |

所以现代漏洞利用常常不是单一技巧，而是组合链：信息泄漏绕过 ASLR，内存破坏获得写能力，ROP 绕过 DEP，最后执行 payload。

### 复习自查

1. 为什么“有漏洞”不等于“一定有 exploit”？
2. exploit 和 payload 的边界在哪里？
3. 为什么覆盖相邻变量也可能造成严重安全问题？
4. DEP/NX 为什么会打断传统代码植入？
5. Shellcode 编码解决的主要问题是什么？

---

