---
title: linux彻底卸载nginx
date: 2019-01-27 16:01:19
tags:
---

# 1.删除nginx，–purge包括配置文件
```
sudo apt-get --purge remove nginx
```

# 2.自动清除全部不使用的软件包
```
sudo apt-get autoremove
```
# 3.查找与nginx相关的软件
```
dpkg --get-selections|grep nginx
```

# 4.删除第三步查询出来的结果
```
sudo apt-get --purge remove <软件的名称>
```
# 5.至此，关于nginx的文件及配置文件已经完全卸载了

你以为这就结束了吗？（too young ，too naive）

请继续往下看👇👇👇👇👇👇












# 6.查看nginx正在运行的进程
```
ps -ef |grep nginx
```
一般执行完此命令后，nginx还是启动着的

>  $ ps -ef |grep nginx
>  root      7875  2317  0 17:05 ?        00:00:00 nginx: master process /usr/sbin/nginx
>  www-data  7876  7875  0 17:05 ?        00:00:00 nginx: worker process
>  xxxxxxx   8321  3510  0 17:40 pts/0    00:00:00 grep --color=auto nginx

# 7.kill nginx进程
```
sudo kill  -9  7875 7876 
```

# 8.全局查找与nginx相关的文件
```
sudo  find  /  -name  nginx*
```

# 9.删除第8步列出的所有文件
```
sudo rm -rf file
```

# 10.恭喜你！！！

这次nginx被你彻底删除了