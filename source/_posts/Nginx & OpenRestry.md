---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UTA5CV7%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T210051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFM2o2LFXeg2b1mHl2h%2B8p3vWDNgePMOusTLAstD1UctAiEAnePFRL%2BOMSV3r54lI5KkaCPAllHYRI4BW6j1HIpJF6IqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGy0%2FTt2XK05ZrjCsyrcA1JMKTn8l9eCdqUJ8AADFNblQ3THRRmm3mPKNyKSmUIIxDxzaTGCG5Fr%2FjDyPS3C6CHZ5q%2B98smPaHqoUKjzPqo3sdcjM3CcHF892WlpMb1TL%2FEkexCpX%2FR1bVZ%2B0AuuPXow32F1qbG42FGLd1UaYxnnelZP%2Fvbuv3xxC3C%2By8AojC1jjZw0KiNb%2B%2BznaQB1v%2F3mdvUrCIzhv23CE8f1aYdJFHwIWvLP6OGKT3AOHfRM7J%2FLM1wNLbLOM%2FNQZ6H5VBeMMmyaUjhuCy9mcKfM9IgQ13BfstjYJ6qJOxWPvBQA2zPAPmvOHl2uIb3ZsI0miw%2BMzvkMGV23cFk3s9h9zMxkpiHoBWZZbO7xqJ9vHLNUltzfRqEAmBD3%2FXH%2BU0I%2B83YORX%2B%2FXnOGsiFV1YIufNfUhH6wrQO3CZ0vP%2FKQps90VGAp21zA94YFJDn9eWu2Z%2BdinPALjUou29ZkNRquCOGpIVYvy5PXppX6iiJrnp6jYja4vDur9HO%2FfzlLApCYYyrXnL5JC2fiE4RefCKZPRCG68r%2BtcMxKwtEbBGF36qoRzWypfqePB9TZ3xUCRTGOTEAeThwz7190qSZ7OneFQmruFvphtfI7c9oa6ykeaQWPRALhYV8uR2ahezQMLq4xccGOqUBvIKLb5RrsZJ%2BZGRNCvtrKUPbLo4nRHOITAEIWtNjlbQqkEp%2BsXPCkbhRnCbzEGzMTyYwV9f4%2FdeduXk%2Bg6GDcToP%2BjxNnmgaMHsrIK917oKuxqvjm99puEoNsCRecS%2FupHFFn%2FffTs4Mime3N3K0iT66ZYgLclFx%2Fun9lI5%2FtDdq2Pz64u%2BoEQ7c7oWJtUUkkLDIoaRmxw8%2FXoosDr9sNjSoS1iG&X-Amz-Signature=90d18f3baba66e2ad661495477123ce6a7e395d0151d6bfbca7cd93cde6700f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
