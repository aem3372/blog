title: 更新Win8.1后浏览器无法打开网页解决办法
id: 1218
categories:
  - 应用开发
date: 2014-01-30 00:27:59
tags:
---

<span style="color: #333333;">**最近有朋友反映在更新至Win8.1后出现了，浏览器无法打开网页但是可以登陆QQ的症状。**</span>

以下便是当时的**解决办法**：

1.以管理员身份进入命令行模式。

[![以管理员身份进入命令行模式](http://www.aemiot.com/wp-content/uploads/2014/01/20140130001927.jpg)](http://www.aemiot.com/wp-content/uploads/2014/01/20140130001927.jpg)

按下WIN（徽标键）+R，出现如上图界面，勾选以管理员权限创建任务，然后输入CMD，点击确定，弹出一个命令行窗口。

2.输入netsh winsock reset，按照提示重启后即可。

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]