title: 密码管理器V1.0.0(Beta)
tags:
  - Aem
  - 密码
  - 管理器
  - 软件
id: 538
categories:
  - 作品
date: 2013-01-26 19:50:55
---

是否还在为记住各个网站的密码而头痛？

是否还在为忘记密码纠结？

试试这款软件吧!

在这里，记录你的密码，不用担心密码被被人查看。因为数据已经加密过了。

你可以将密码记录文件 *.usr 和 *.usrpas 上传至网盘，这样你可以在任何地方查看你的密码（只需要下载本软件即可，本软件非常小，因此具有便携性）。

[toggle title="点击展开使用说明"]

使用手册
1.-----------------------------第一次使用程序
2.-----------------------------通用指令
3.-----------------------------管理员指令
4.-----------------------------超级管理员指令
5.-----------------------------普通用户指令
6.-----------------------------其他说明

1.第一次使用程序

第一次使用请使用超级管理员账户登录
用户名为 root
默认密码为 systemroot
登录后，请立即修改超级管理员密码
如有需要可以使用超级管理员指令添加其他用户

2.通用指令

mode
查看可用的数据加密模式
格式: mode
样例:
输入 mode
输出 unencrypted
simple
setpassword
修改当前用户密码
格式: setpassword &lt;参数&gt;
样例:
输入 setpassword 123456

3.管理员指令

adduser
添加一个普通用户
格式1: adduser
样例:
输入 adduser
输出 请输入用户名:
输入 one_user
输出 请输入密码:
输入 123456
输出 请输入加密模式:
输入 simple
格式2: adduser &lt;参数1&gt; &lt;参数2&gt; &lt;参数3&gt;
样例:
输入 adduser one_user 123456 simple
user
查看当前系统下的普通用户
格式: user
样例:
输入 user
输出 root
deluser
删除一个普通用户
格式: deluser &lt;参数&gt;
样例:
输入 deluser one_user

4.超级管理员指令

所有的管理员指令
额外的
addadmin
添加一个管理员
格式1: adduser
样例:
输入 addadmin
输出 请输入用户名:
输入 one_admin
输出 请输入密码:
输入 123456
格式2: addadmin &lt;参数1&gt; &lt;参数2&gt; &lt;参数3&gt;
样例:
输入 addadmin one_admin 123456 simple
deladmin
删除一个管理员
格式: deladmin &lt;参数&gt;
样例:
输入 deladmin one_admin

5.普通用户指令

dir
查看该用户的所有目录，或者某个目录的信息
格式1: dir
样例:
输入 dir
输出 menu1
menu2
格式2: dir &lt;参数&gt; &lt;参数&gt; ...
样例:
输入 dir menu1 menu2 ...
输出 menu1:
password1
password2
menu2:
password1
...

seach
查找一个密码所在的分组
格式: seach &lt;参数&gt;
样例:
输入 seach password1
输出 password1:
menu1
menu2

password
查看一个密码
格式: password &lt;参数&gt; &lt;参数&gt;
样例:
输入 password menu1 password1
输出 用户名: 123456789
密码: 123456789
安全码: 123456

adddir
添加一个目录
格式1: adddir
样例:
输入 adddir menux
格式2: adddir
样例:
输入 adddir
输出 请输入分组名
输入 menux

deldir
删除一个目录
格式: deldir &lt;参数&gt;
样例:
输入 deldir menux

addpassword
添加一个密码
格式1: addpassword &lt;参数1&gt; &lt;参数2&gt; &lt;参数3&gt; &lt;参数4&gt; &lt;参数5&gt;... 至少3个参数
样例：
输入 addpassword menu1 123456789 123456789 安全码 123456
格式2: addpassword
样例:
输入 addpassword
输出 请输入要添加到的分组名
输入 menu1
输出 请输入账号
输入 123456789
输出 请输入密码
输入 123456789
输出 是否还有其他信息要添加&lt;Y\N&gt;
输入 Y
输出 请输入栏目名称
输入 安全码
输出 请输入内容
输入 123456
输出 是否还有其他信息要添加&lt;Y\N&gt;
输入 N

delpassword
删除一个密码
格式: delpassword &lt;参数1&gt; &lt;参数2&gt;
样例:
输入 delpassword menu1 password1
[/toggle]

[note]
<span style="color: #ff6600;">**如果软件使用中发现任何问题，可以在这里留言。**</span>

<span style="color: #ff6600;">**如果是威胁到用户安全的BUG，请发送邮件至我的邮箱，而不是在这里公布。**</span>
[/note]

<big>下载</big>

&nbsp;
[warning]
作者:**Aem**
本文版权归作者和aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]