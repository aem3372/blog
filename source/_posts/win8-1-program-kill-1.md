title: 更新Win8.1后浏览器无法打开网页解决办法
id: 1218
categories:
  - 其他
date: 2014-01-30 00:27:59
tags:
---

**最近有朋友反映在更新至Win8.1后出现了，浏览器无法打开网页但是可以登陆QQ的症状。**

以下便是当时的**解决办法**：

[![以管理员身份进入命令行模式](http://www.aemiot.com/wp-content/uploads/2014/01/20140130001927.jpg)](http://www.aemiot.com/wp-content/uploads/2014/01/20140130001927.jpg)

1. 以管理员身份进入命令行模式。(按下WIN（徽标键）+R，出现如上图界面，勾选以管理员权限创建任务，然后输入CMD，点击确定，弹出一个命令行窗口。)
2. 输入`netsh winsock reset`，按照提示重启后即可。
