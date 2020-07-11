---
title: linux 用法点滴
date: 2017-04-12 10:54:03
categories:
- linux
---

### 修改前缀显示
没有文件先创建一下
```
cd ~
touch .bash_profile
```
当前提示符显示方式
``` bash
echo $PS1
\h:\W \u\$
cd ~
open -e .bash_profile
export PS1=&quot;\\u \\w$&quot;
```
