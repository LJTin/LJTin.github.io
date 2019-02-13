---
title: PM2
date: 2019-02-13 23:00:38
tags: 
---
本文只记录PM2的运行时，需要其他版本的可点击以下链接👇👇👇
参考官方文档：https://pm2.io/doc/

# 概念：
PM2 Runtime是具有内置Load Balancer的Node.js应用程序的生产过程管理器。它允许您永久保持应用程序的活动，无需停机即可重新加载它们，并促进常见的Devops任务。

# 安装
请自行学习，使用搜索引擎或点击链接👆👆👆

# 命令行大聚集：(以下.js可省略)

## 常用的命令行
启动应用程序
>` pm2 start app.js`

文件更改时启动并自动重新启动链接
>`pm2 start app.js --watch [--ignore-watch /*/]`

停止应用程序
>`pm2 stop app.js`

停止并启动程序 
>`pm2 restart app.js` 

启动时启动PM2
>` pm2 startup`

以集群模式启动，使用-i选项 
>`pm2 start app.js -i 4` (4为集群数量)
>`pm2 start app.js -i max`（max可以自动检测可用的CPU数量）

展示pm2的列表 
>`pm2 ls`

展示日志
>`pm2 logs`

监控
>`pm2 monit`

从列表中停止并删除一个进程 
>`pm2 detele app`

## 不常用

使用重新加载而不是重新启动0秒停机时间重新加载
>`pm2 reload app`

重置重启计数器
>`pm2 reset all`


未完待续······