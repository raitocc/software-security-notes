# Lecture08 Web 安全 - 习题

> GitHub: https://github.com/raitocc/software-security-notes

> 配套答案：[Lecture08-Web 安全-习题答案.md](Lecture08-Web 安全-习题答案.md)

## A. 基础查表题

这一部分主要考 HTTP 会话、文件上传、XSS、SQL 注入、文件包含和反序列化的基本概念。

### 选择题

1. HTTP 被称为无状态协议，更接近下列哪种含义？

A. 服务端默认不具备跨请求身份记忆，需要额外机制维护登录态  
B. 浏览器提交 POST 请求时，请求体会自动进行端到端加密  
C. Web 服务器处理静态页面时，不需要返回状态码和响应头  
D. 数据库执行 SQL 查询后，会自动保存用户的浏览器会话

2. 关于 Cookie 与 Session 的关系，下列说法更合理的是：

A. Cookie 常存放在客户端，Session 数据通常存放在服务端，客户端携带 SessionID 标识会话  
B. Cookie 主要用于保存数据库表结构，Session 主要用于保存网页 HTML 标签  
C. Cookie 通常由数据库创建，SessionID 通常由浏览器随机生成并提交给服务器  
D. Cookie 与 Session 都用于替代 HTTPS，因此配置后可省略传输层保护

3. OWASP 的全称更接近：

A. Open Web Application Security Project  
B. Online Windows Attack Signature Platform  
C. Open Wireless Authentication Service Protocol  
D. Object Web Access Storage Policy

4. WebShell 在课程语境中更接近：

A. 以 Web 脚本形式存在、可通过网页请求控制服务器执行操作的后门环境  
B. 浏览器用于渲染 HTML 与 CSS 的安全沙箱组件  
C. 数据库用于隔离不同用户 SQL 查询结果的事务日志  
D. Web 服务器用于压缩图片与静态资源的缓存目录

5. XSS 与 SQL 注入的主要区别更接近：

A. XSS 让脚本在用户浏览器中执行，SQL 注入改变后端数据库查询语义  
B. XSS 主要攻击数据库索引结构，SQL 注入主要修改浏览器 DOM 节点  
C. XSS 依赖上传目录脚本执行，SQL 注入依赖 Cookie 设置 HttpOnly  
D. XSS 与 SQL 注入都围绕 PHP 对象恢复过程中的魔术方法触发

6. `sqlmap -u url --tables -D "testdb"` 的作用更接近：

A. 对指定 URL 测试并列举 `testdb` 数据库中的表  
B. 导出 `testdb` 中每个字段的完整数据内容  
C. 查看当前 Web 服务器的 Apache 访问日志路径  
D. 设置 Cookie 的 domain、path 和 secure 参数

### 判断题

7. POST 请求的数据放在请求体中，但这不等于加密，也不等于不能被伪造。

8. HttpOnly 可以降低脚本直接读取 Cookie 的风险，但脚本仍可能借当前登录态发起请求。

9. 远程文件包含在 PHP 中通常与 `allow_url_fopen` 和 `allow_url_include` 的配置有关。

10. PHP 反序列化对象时，`__wakeup()` 属于可能被自动触发的魔术方法。

### 填空题

11. CSRF 的全称是 `__________`。

12. LFI 的全称是 `__________`。

13. IDOR 的全称是 `__________`。

14. POP Chain 中 POP 的全称是 `__________`。

## B. 理解应用题

这一部分要求你把机制放回代码、配置和攻击链中理解。

### 选择题

15. 关于 `setcookie(name, value, expire, path, domain, secure)`，下列理解更合理的是：

A. `expire` 影响 Cookie 生命周期，`path` 和 `domain` 影响 Cookie 的发送范围  
B. `secure` 用于指定 Cookie 的数据库字段类型，`path` 用于指定 SQL 表名  
C. `domain` 表示 Session 数据在服务端保存的位置，`expire` 表示数据库连接超时  
D. `name` 和 `value` 由浏览器固定生成，服务端主要控制 `secure` 参数

16. 下列哪项更接近“会话保持攻击”？

A. 攻击者拿到有效 Session 后周期性访问，使服务器延长该会话的可用时间  
B. 攻击者把评论内容保存到数据库，使其他用户打开页面时执行脚本  
C. 攻击者通过单引号破坏 SQL 语法，再观察数据库错误信息  
D. 攻击者把压缩包内文件通过 `zip://` 包含进 PHP 解释流程

17. 下面的上传处理方式中，风险更突出的点是：

```php
$name = $upfile["name"];
$tmp_name = $upfile["tmp_name"];
move_uploaded_file($tmp_name, "up/".$name);
```

A. 使用客户端文件名拼接保存路径，且未展示真实类型校验和执行权限隔离  
B. 读取了上传文件的临时路径，因此浏览器会自动启用 HTTPS 传输  
C. 调用了 `move_uploaded_file` 后，PHP 会把图片重新解码并消除脚本片段  
D. 把文件放入 `up/` 目录后，数据库会拒绝后续 SQL 查询请求

18. `htmlspecialchars($_GET['name'])` 防御 XSS 的关键思路更接近：

A. 把输入中的 HTML 特殊字符编码，降低其被浏览器当作标签或脚本解析的机会  
B. 把用户输入保存到 Session 中，使 SQL 查询结构与参数彻底分离  
C. 把 Cookie 设置为持久 Cookie，降低跨站请求伪造的触发概率  
D. 把上传文件后缀改成随机字符串，使 Web 服务器无法读取页面内容

19. 关于布尔盲注中 `1' and 1=1 #` 与 `1' and 1=2 #` 的比较，下列说法更合理的是：

A. 通过真假条件导致的页面差异，判断输入是否改变了 SQL 查询逻辑  
B. 通过两个请求的图片尺寸差异，判断上传目录是否可执行脚本  
C. 通过 Cookie 过期时间差异，判断 Session 是否存放在服务端  
D. 通过两个 URL 的路径长度差异，判断 PHP 伪协议是否可访问压缩包

20. 关于 `php://filter` 与 `php://input`，下列理解更合理的是：

A. `php://filter` 常用于读取并编码源码，`php://input` 常用于读取原始 POST 请求体  
B. `php://filter` 用于设置 Cookie 安全属性，`php://input` 用于枚举数据库表  
C. `php://filter` 主要触发 `__wakeup()`，`php://input` 主要触发 `__toString()`  
D. 二者都用于浏览器本地缓存控制，与服务端文件包含场景关系较弱

### 简答题

21. 为什么说 GET 和 POST 是请求语义差异，而不是安全等级差异？

22. 为什么存储式 XSS 的风险通常高于反射式 XSS？

23. 参数化查询为什么比黑名单过滤更可靠？

24. 如何区分正常文件包含和文件包含漏洞？

## C. 综合融会题

这一部分更接近考试中的推理题，需要把多个 Web 漏洞机制串起来。

### 综合题 1：会话与传输

某网站登录后通过 Cookie 保存 `PHPSESSID`，登录页和业务页仍使用 HTTP。Cookie 未设置 `Secure`、`HttpOnly`、`SameSite`。管理员长时间不退出后台。

25. 这个场景中与传输层保护不足相关的风险是什么？
26. 如果攻击者拿到 `PHPSESSID` 并直接访问后台，这属于哪类会话攻击？
27. 如果攻击者周期性刷新页面让该会话持续有效，更接近哪类攻击？
28. `Secure`、`HttpOnly`、`SameSite` 分别主要缓解什么问题？

### 综合题 2：文件上传攻击链

某头像上传功能仅检查浏览器提交的 MIME 类型是否为 `image/jpeg`，保存时沿用用户文件名，并把文件放在 Web 可访问目录。另有页面通过 `include $_GET['page'];` 显示内容。

29. MIME 类型检查为什么不足以判断文件安全？
30. 攻击者如何把上传漏洞和本地文件包含串成代码执行链？
31. 为什么 `shell.php.jpg` 这类文件名在某些环境中仍需警惕？
32. 请给出至少三项服务端防御措施。

### 综合题 3：XSS 与 SQL 注入

某页面有两段代码：

```php
echo "Hello ".$_GET["name"];
$sql = "select * from user where name='".$_GET["name"]."'";
```

33. 第一行更容易引出哪类漏洞？根因是什么？
34. 第二行更容易引出哪类漏洞？根因是什么？
35. 对第一行应优先考虑哪类输出处理？
36. 对第二行应优先考虑哪类查询构造方式？

### 综合题 4：反序列化链路

某应用从 `$_GET["config"]` 读取字符串，执行 `unserialize(base64_decode($_GET["config"]))`。应用中已有若干类，其中一些类包含 `__wakeup()`、`__toString()`、`__destruct()`，并在方法中进行文件操作或模板渲染。

37. 该入口为什么满足反序列化漏洞的关键条件之一？
38. 攻击者为什么还需要分析目标应用已有类的源码？
39. POP Chain 的思路是什么？
40. 从设计角度看，应如何降低这类风险？
