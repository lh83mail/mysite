---
title: Linux 常用工具使用
p: linux/linux-command
date: 2018-03-31 09:53:14
tags: 
    - Linux
    - 工具
mathjax: true
---

# 快速参考

## SSH

### 参数
-C  数据压缩后再传输
-N  不执行远端传过来的命令
-f  后台运行ssh
-R  反向代理
-L  正向代理
### ssh 正向代理

```,bash

```

### ssh 反向代理

作用：远端服务器通过远端端口把数据转发到被代理服务器的目标端口
场景： 
1. 通过外网服务器访问内网服务器

格式：
 ssh -CNfR <远端端口>:<被代理服务器ip>:<目标端口> <远端用户名>@<远端IP>

```,bash
    ssh -CNfR 8233:127.0.0.1:58203 root@120.79.64.181
```

$ b = a + c  $


$L_2Loss=\sum_{(x,y)\in D} (y - prediction(x))^2$


$$ n! = \begin{cases}
1 & \text{if n = 0},\\
(n-1)! \ast n & \text{if n > 0}
 \end{cases} $$
