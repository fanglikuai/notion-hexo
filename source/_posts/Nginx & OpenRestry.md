---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN4AN4IC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD7c3IWjRG0LjPmea7RZ7UsSTIGuiwPy%2FvESwSSQIL8vgIgM46sEsiDd%2FbSzWk7s1j8LIZ3e4LDjBbCPpaPXTXNfhsq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDN09j%2FbjXod7SLELircAxSlLR%2FO2717GMYRWnc53UP7DmF0ItL88W%2FJG6dfR5xfUe6%2FMOPNiBN6gkE7e7hQfmCnRsGILH6%2FsAE1QDbwu0Fiit2bNumEULgVZWpndmkDWR4ElTb76WS%2FOKla9EFkk3XXGqeNQrQqQk3NK4z10tWMM9cmI634UISMHaFrKSUO%2BFW%2FDNVM1yFbup1uTS3Kre1qWR%2FLtatB5srh5NoozEgLlVRtEBkJzIDXSjOU5mU12S8GZUz9xzGojeRnPC%2BRy8CuMXwYsa%2BfNvD5xZ1ptUlf%2Bm4DmlP4ZJ5MmgqIi8qB4dV%2BnMAGPL%2FmJLPn%2FP9hwIxIQKx0WiWD9T4HiyZ%2FFbHvqP9mxsrvBmA68OUvbqCO9sMNB3SJ9vlcIoJ57GWcKGeIlf56VpeWdqFIQruqhtaTcjWM%2FFLBKYq4FzYP7J9h8hToiojft6KNoZa73KKrsgSuu12OeJC8gP%2BYAcnZq%2BbaAXFPpodTpzWCyYG75WzsM%2BbWWHjLGFyjeV7QAE4UExWWgpgheqrKw%2F%2BSMhSzhNRwqP77tLcq9G0f%2BbTVz9IdICbgdH%2BhKCL2aNsmGygB5T7n5ibmJFx7blxvYnIW2onngas6dQ3gOF%2B7sQ9UkaXp5B7pRIVvOd7QKNLOMMmPiskGOqUBRDHgvkfEaMXhdafXjs%2BYgF238y4OK5yZzsea%2FhTfqOoW63dyBT0ALWVpJy421ReupxVy4Nf9IO%2Bk%2FRw0X%2FSq6V9ISczTIvNEcQZv7kNCOjUJ%2BBqoS6Ex1oLWJwxJ5bPXtH8WlIq3Nek3R0mP7KjWhpbPV0w7vRmy4XRSb5tlqDYKpbHPWcaJOQyz1IvVezdxOtOLyub5xcu4CsnOVpi5pEQ5Sjf9&X-Amz-Signature=9625eb8b51d732398dc3f215399c30f800723e363a220bdf5ac08c99cc955e5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
