---
title: git配置
date: 2017-03-10 16:50:18
categories: 云主机
tags:
 - git
 - Linux
---
当你安装Git后首先要做的事情是设置你的用户名称和e-mail地址。这是非常重要的，因为每次Git提交都会使用该信息。它被永远的嵌入到了你的提交中：提交代码的log里面会显示提交者的信息
 <!-- more -->
 ``` shell
 git config --global user.name [username]
 git config --global user.email [email]
 ```
重申一遍，你只需要做一次这个设置。如果你传递了 --global 选项，因为Git将总是会使用该信息来处理你在系统中所做的一切操作。如果你希望在一个特定的项目中使用不同的名称或e-mail地址，你可以在该项目中运行该命令而不要--global选项。
eg.
``` shell
jjs@jjs-thinkpad-w520:~/.ssh$ git config --global user.name 857012499@qq.com
jjs@jjs-thinkpad-w520:~/.ssh$ git config --global user.email 857012499@qq.com
jjs@jjs-thinkpad-w520:~/.ssh$ git config --list
user.name=857012499@qq.com
user.email=857012499@qq.com
```

