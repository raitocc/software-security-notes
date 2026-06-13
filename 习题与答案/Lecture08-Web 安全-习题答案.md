# Lecture08 Web 安全 - 习题答案

> GitHub: https://github.com/raitocc/software-security-notes

> 对应习题文件：[Lecture08-Web 安全-习题.md](Lecture08-Web 安全-习题.md)

## A. 基础查表题答案

1. C。HTTP 默认不保存跨请求身份状态，登录态需要 Cookie、Session、Token 等机制维护。A HTTP 不一定断开 TCP 连接（有 Keep-Alive）；B HTTP 默认不加密；D HTTP 可以传输各种资源。

2. D。Cookie 常在客户端保存，Session 数据通常在服务端，SessionID 用于关联二者。A Cookie 可设置过期时间是正确的，但这不是区别；B SessionID 通过 Cookie 传递是正确的；C Session 关闭浏览器后可能失效（取决于配置）。

3. B。OWASP 是 `Open Web Application Security Project`。A、C、D 都不是 OWASP 的全称。

4. C。WebShell 是网页脚本形式的后门，可把 HTTP 请求转换成服务器端操作入口。A 浏览器沙箱是安全机制；B 数据库事务日志是存储机制；D 缓存目录是性能优化。

5. D。XSS 的脚本在浏览器执行，SQL 注入改变数据库解释的查询语义。A XSS 不攻击数据库索引；B XSS 不依赖上传目录；C 反序列化与 XSS/SQL 注入无关。

6. B。该命令用于枚举指定数据库中的表。A 导出数据需要 `--dump`；C 查看日志需要其他工具；D 设置 Cookie 需要其他参数。

7. 对。POST 不显示在 URL 中，但不提供加密，也不能替代鉴权和 CSRF 防护。

8. 对。HttpOnly 主要限制脚本读取 Cookie，不能阻止脚本以当前页面身份发请求。

9. 对。课件中强调 RFI 通常要求 `allow_url_fopen` 与 `allow_url_include` 配合开启。

10. 对。`__wakeup()` 在 `unserialize()` 过程中可能自动触发。

11. `Cross-Site Request Forgery`。

12. `Local File Inclusion`。

13. `Insecure Direct Object Reference`。

14. `Property-Oriented Programming`。

## B. 理解应用题答案

15. A。`expire` 控制生命周期，`path` 和 `domain` 控制 Cookie 发送范围。B `secure` 与数据库无关；C `domain` 与 Session 存储位置无关；D `name` 和 `value` 由服务端设置。

16. C。会话保持攻击的关键是延长已窃取会话的可用时间。A 存储式 XSS 是脚本注入；B SQL 注入是查询篡改；D 文件包含是路径控制。

17. D。客户端文件名不可信，直接拼路径保存容易带来后缀伪装、路径控制和执行风险。A HTTPS 与文件名无关；B PHP 不会自动消除脚本片段；C 数据库与文件保存无关。

18. B。`htmlspecialchars()` 把特殊字符转义，降低输入被浏览器解释为 HTML/JS 的机会。A 这是 Session 的职责；C 这是 Cookie 的职责；D 这与文件上传无关。

19. C。布尔盲注通过真假条件造成的响应差异推断 SQL 逻辑是否被用户输入影响。A 图片尺寸与 SQL 注入无关；B Cookie 过期与 SQL 注入无关；D URL 路径与 SQL 注入无关。

20. D。`php://filter` 常用于读取源码并编码，`php://input` 读取原始 POST 请求体。A 这些与 Cookie 设置无关；B 魔术方法触发与伪协议无关；C 这些与浏览器缓存无关。

21. GET 常用于查询，参数在 URL 中；POST 常用于提交，数据在请求体中。二者都可被构造和发送，是否保密依赖 HTTPS，是否授权依赖服务端鉴权，是否防 CSRF 依赖 Token、SameSite 等机制。

22. 存储式 XSS 会把恶意脚本保存到服务器端，用户访问正常业务页面时触发；它不依赖受害者点击明显异常 URL，也更容易埋在留言、邮件、个人资料等业务数据中。

23. 参数化查询把 SQL 结构和用户输入分离，数据库按参数处理输入；黑名单过滤依赖枚举危险形式，容易被编码、注释、大小写、数据库函数和语法差异绕过。

24. 正常文件包含是开发者固定包含公共代码，例如数据库连接文件或模板；文件包含漏洞是用户可控参数决定包含路径，导致读取预期外文件或执行可控内容。

## C. 综合融会题答案

25. HTTP 明文传输可能暴露用户名、密码或 SessionID，攻击者可通过嗅探或中间人位置获取会话凭据。

26. 属于会话劫持。如果 SessionID 来自 Cookie，也可称为 Cookie 劫持。

27. 更接近会话保持攻击。攻击者通过持续活动让服务端不销毁该 Session。

28. `Secure` 限制 Cookie 通过 HTTPS 发送；`HttpOnly` 降低 JavaScript 读取 Cookie 的风险；`SameSite` 限制跨站请求携带 Cookie，降低 CSRF 风险。

29. MIME 类型来自请求声明，攻击者可以伪造；它不能证明文件真实内容、后缀解析方式和服务端执行权限安全。

30. 攻击者可上传含 PHP 代码的伪装图片，再让 `page` 参数指向该上传文件。PHP `include` 处理文件内容时，可能执行其中的 PHP 代码。

31. 双后缀、服务器解析差异、大小写差异和错误配置可能让看似图片的文件按脚本处理；即使不能直接执行，也可能结合 LFI 被解释执行。

32. 可采用白名单后缀和真实类型检查、图片重新解码再编码、随机重命名、存储到 Web 根目录外、上传目录禁脚本执行、限制大小和权限隔离等措施。

33. 第一行更容易引出 XSS。根因是用户输入未经输出编码就进入 HTML 响应，浏览器可能把它当标签或脚本解析。

34. 第二行更容易引出 SQL 注入。根因是用户输入直接拼接进 SQL 字符串，数据库可能把输入内容当 SQL 语法解析。

35. 应按输出上下文做编码。这里是 HTML 文本上下文，可用类似 `htmlspecialchars()` 的 HTML 编码方式。

36. 应使用参数化查询或预编译语句，把 SQL 结构和参数数据分离。

37. 用户可控数据经过 base64 解码后进入 `unserialize()`，满足"不可信数据参与反序列化"的关键条件。

38. PHP 反序列化对象依赖运行时已有类；利用通常需要找到魔术方法、可控属性和危险操作之间的调用关系，所以要分析源码。

39. POP Chain 是通过构造对象属性，让目标应用已有类中的方法依次触发，最终把可控数据传到文件操作、命令执行、模板渲染等危险点。

40. 设计上应避免对不可信数据使用原生对象反序列化；可改用 JSON 等简单格式，增加签名校验，限制可反序列化类，并减少魔术方法中的敏感副作用。
