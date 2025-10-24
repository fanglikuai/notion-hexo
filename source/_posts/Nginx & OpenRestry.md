---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CSWSQ42%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCYJfJ9MI8FNZRTga0NPceJ9%2FQcGfbxR10N9KIziPhBaAIhAOadWUhUS0u0dbtfIhjd2c2jVFQwRbwCghXV84Ob%2BlSoKv8DCFYQABoMNjM3NDIzMTgzODA1Igz3l3SglkZU2k%2F3Wn4q3ANcYAAT1rgZeKbJ3moeYYdwb43wzRJFug1f9lEg8gvwUVz%2Fx8SUPV6B1bBs%2Fz4NVvFHbfDFLCXuB475KA0fs0%2FtVntsle331Qp4iUgDEXVjwxPyBkNJ3%2FTQsYiV7qDiQ15Dd%2FhKP6G8dneTWDfEtStYqTks7nRgsWjkPI5L9%2FjiH5Po7TbOAbXrGKesL%2BwX0oLUIbAg4QcB%2BKTfgvl4%2Frvjn7AIOMyxwTFlXx2EQ7nqdcqf4ygkNun5KhSLuHc%2FtqUto2M21cfuQOb3JxfT8JTSTviD92AK2Hkmkh%2FYZGUkPnFuMRtSnuwzTD4hwEa9M058vVfQqDQuTZ%2FThEPvQk%2BE0z7jOiW0CW%2FH4D6r3%2FdsIRzCojtVUsdCLKuMnvU%2Bt11yy%2FNEGo9TSnObHq5LhXFGVQURu311Wn%2BZkYTr8OslLRz4VU%2FaJwbEFTP4v7Ns3EY57HXLSQyjGl84dauwo7fWDGvKRROWzDvuLKPCN97C%2FEM9LxQibB9ERXouk9gPBHEnzackII67I6XJcz43gvL5FBNTCed7X%2BH2J9pcE60NfD6ewCOMptfLGFXzLP9RjbtORqxAoXUlrl2zHcdcExtZ0WrWgPc2NlBtqw4L6N17K2tX6DlUgRzFq8zDJzCajOzHBjqkAUvYROswabIwla%2F6T6SyuHebcR2f3eO8GHDSKuNkvqbmUb%2F%2B9aBm09R30LohjHaHl0PfB0mVyghJbw%2F0SwWTyafan6FwPsnrBtoG4F7I42U0L7Lhc40I78y9epEJts1Av1yno5tTfC6EIBhHSWDa1TbkS9KPw49OwRj6DcVSKw1ICpFFoIaeVB2LJG6v00AyduCdg4%2FkTDqp9xrDY5%2FcFsLpwm7v&X-Amz-Signature=91d37bfb69dfaf990451bd8946c66ab585fc9743f37126e9135676fcf28e8ac7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/681caddd167c86081c93eb4da2dc581a.png
banner_img: /images/681caddd167c86081c93eb4da2dc581a.png
---

# 基本概念


**Nginx (engine x)** 是一款轻量级的 Web 服务器 、反向代理服务器及电子邮件（IMAP/POP3）代理服务器。


**反向代理与正向代理的区别：**


正向代理：在用户这一端，vpn


反向代理：在服务器端，nginx

> 拓展：
>
> 堡垒机：统一的运维入门，带权限认证
>
>

基本使用：


```bash
nginx -s stop
#快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。nginx -s quit
#平稳关闭Nginx，保存相关信息，有安排的结束web服务。nginx -s reload
#因改变了Nginx相关配置，需要重新加载配置而重载。nginx -s reopen
#重新打开日志文件。nginx -c filename
#为 Nginx 指定一个配置文件，来代替缺省的。nginx -t
#不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。nginx -v
#显示 nginx 的版本。nginx -V
#显示 nginx 的版本，编译器版本和配置参数。
```


# 实战


反向代理域名的tomcat


```plain text
upstream zp_server1{
  server 127.0.0.1:8080;
  # 写要代理的地方
}
server {
  listen       80;
  server_name  www.helloworld.com; #从哪里来的域名

  #charset koi8-r;

  #access_log  logs/host.access.log  main;

  location / {
    #  root   html;
    # index  index.html index.htm;
    proxy_pass http://zp_server1;
    #进行代理
  }
```


## 跨域问题

1. 在 Nginx 的`server` 或`location`块中添加以下头部：

```plain text
location / {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
  add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';

  # 处理预检请求 OPTIONS
  if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
    add_header 'Access-Control-Max-Age' 1728000;
    add_header 'Content-Type' 'text/plain; charset=utf-8';
    add_header 'Content-Length' 0;
    return 204;
  }

  # 其他请求正常处理
  ...
}
```

1. 指定的域名可以跨域
