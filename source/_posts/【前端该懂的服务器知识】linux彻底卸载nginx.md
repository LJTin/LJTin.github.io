---
title: 【前端该懂的服务器知识】linux卸载nginx
date: 2019-01-27 16:01:19
tags: 前端
---

# 前言：
  前段时间搞的AWS的服务器发现nginx的配置压根就没配对，所以趁着周末解决一下，然后网上找了一些资料，发现和自己原先弄的nginx路径差异很大，奈何我头铁，强撸一波（灰飞烟灭，唉～～～）
  
  最终结果：凉得很透！！！已经弄的回不了头了，只好全部打翻，重新来一次（毕竟还年轻嘛～～～～😆😆😆😆😆😆）
  
  既然出了问题，那就先从卸载删除开始吧，请各位看官往下看～～～👇👇👇
<br>
# 1.删除nginx，包括配置文件
```
sudo apt-get --purge remove nginx
```

# 2.自动清除全部不使用的软件包
```
sudo apt-get autoremove
```
# 3.查找与nginx相关的软件
```
dpkg --get-selections | grep nginx
```

# 4.删除第三步查询出来的结果
```
sudo apt-get --purge remove <软件的名称>
```
# 5.至此，关于nginx的文件及配置文件已经完全卸载了

你以为这就结束了吗？
<br/>
<h1>too young ，too simple</h1>
<br/>
请继续往下看👇👇👇👇👇👇

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

# 6.查看nginx正在运行的进程
```
ps -ef | grep nginx
```
一般执行完此命令后，nginx还是启动着的,比如下面这样：

| xxx | xxx | xxx | xxx | 
| :------: |  :------: | :------: | :------: |
| root      | 7875 | 2317 |       nginx: master process /usr/sbin/nginx |
|  www-data | 7876 | 7875 |     nginx: worker process |
|  xxxxxxx  | 8321 | 3510 |    grep --color=auto nginx |

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