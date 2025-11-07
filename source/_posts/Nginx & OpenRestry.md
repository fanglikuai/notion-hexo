---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBOZHJOB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7MlPTL1VSXykEF5ZsYzsfhkEOtL2j61dFOBKDZZ6DMwIgEY3yqOq9yhuHlKigQU9phP39GI1Ky2N9eunVoFXhYXYqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL956DbQOKCu3P5RuCrcAwT5oU3ik9h7tzfx5kYwsQia0w543a30lDVle46YOqGeSVDULGTP3rdmpqqSE9YoJYWhdlcK0RKgQO%2F7YXvF%2FCCkhJCPMP9Cihg2NsDzcu%2B05ZFYKwZqCraRM2DOWUWzMULz3laToAZpDNNAHgkR8M304yqX9PflveeSn9nffGBpd0mO%2Fr9tPJQ4oGPSW2rlFSZJbRTbduLUMKjjZ%2BsibNn%2BkB2VHbCqB9oMMBGnqgOrHdox%2FboJLfzzIl1GP2iG7UNqjSApiePoJnhMNPUMIMUrNT6J9qqwI06SZhUBq7fvf5EjcDVBNfIqkx%2BEqjzprfaf0FqTBFfasmyLJp23z%2BGamoi8lABpgWGVxCRUPbo1P1u9qKVzyGNMsmTaIAy22FK8cwZDl9SSFxpmjGQtBQPsuJB9cH2NtSwBcuO9AHQ39X1eEDFVxPaN%2BtjbVK9wXYBPr2hMrQooTgvruAUmZzOn8HYbZ7ymC04WLlcIiG%2FVwmQ2sOjmSDv9iaS32L6wjOgoWHPF%2Fd8OWcQqeztE3Q2emjZ1547D2VF%2B8C3PpMlT1MN9wswBgMyCCIU1k3WWTWvtVj23JqDjUDFkkGspwyzgQz9zaA85tyinWJB80CWyQqpr8aq52%2B3E4rSAMJbtt8gGOqUBnJljqTg%2F0JtwbKNgrQdf1XukdFT3Fei5K9NS58%2FBt8CvjES87eCKwPIkSnjqwWAyzNgSoHkSY9zcocyggoQEDhrJ7OLk4zJNmrrGHFspbYyKUfD7MkyRtR2C3tFK8UnKjKmKwsKt4uwgTXu1Ja65Gyckk6ATVbDntg8fnHsj0giWa%2FiAuxGvCLXBiNTdY2Hn0xulFDpmPD9YDkoflP5gxRnlH64Q&X-Amz-Signature=d986e58c6d1c1db42aa1ce2352e5c7f61da009222aa1331689c3ba51a6c8cfdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
