---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652BJAMLC%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIDmuPn1sM3SOAb%2FQLPBH9CSC09hBl9xIUQRHt3GzGlMwAiEAijIOXxA1RI1cEkMCxPSW7Pf0x7bOJ%2FMRsNj2pee8cO4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETe%2Fvzr62RsIk2YzSrcAyUegv3ujh67lborUQDEXYOCKyUGASCAaa50wTnWgIachFSFQD3nnL2Mf4jwzPlVe6NugyhGUwfesrZGqUV90U%2BYANSzjKiggq2Bg14Lrdhhz8hvXulBg3hMqSQBRnh3KvK5iwS51nzEeEU3Gr2p7FKWAtqwUzpLeK%2FD8SLTKxm%2FK10R0hmrDvZENMutzLvntqr8LLMS6NyxxsGl8HscI1pC9EE4kQaxAu14My3xKs9kofWLJ8NjiqQPWgAxOAXaaf649URZM1n%2BkSZx3%2FafECo4N%2Bzutg2Y38YgmX0h0a4s%2FDgQjDGxsk7FqdTQlC855i1QEZiA7dOm7L3hB4%2FN6FOG6A1vwqH6oAfhMrnZRAIjgql6%2Bl3htm5KOHa2VnWgb6PqDt7Mws8ul0PkU85MtJLcgQEcbLaI0AdO427pN0KcOiAIk9G9qB2BTfQpwtpFyxvl1HQLkrkm%2BYwrFqcCil%2FYQZLx4x%2FHnf%2BWxQlW6ZWxk%2BpIjOM2lyMmFkzB19NtVaGZ3xZPKK6EXzeKNP7LTdNzZImHbONQ%2Bbg2o8ALR9w916SlJbny5R7YGpq3b1G1uM96fKf1RdOLFFbzRxh5y1gIKrzuUK2pwQKdfsnEdEUf5ypAu19HXXOhS83kMOXc3cYGOqUBpilb0eoCflWTbxwtq4Rbx4DylMPfA5T14GgGg1x7CkXcpJY5UZsJfOn2KKjMcvcFeCbRU9aIB7JaPuw7Kzf0SXISJLV8DUYblVc5M%2BG%2Ffr8wp%2BgqjPVFuoRJJM5WJYKyh8R3535FdpaGLS61JCFbyg28T2gxJ%2BtMuqgfB44Y7ucJY2FbHKJ2ihMwdsPTiuYQoPshSoFyfed6AxdLGgsNXD5ZMoDk&X-Amz-Signature=da532e4120b5910ad94b913510f38bcfd94a33af2eab3e967fef3841da9c248f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
