---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRCF3OZ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICe0Q%2BvunMWMN9L19MZn9kAlFZzNKRzEZ9R3zAiZ5lsgAiBWyeA1zEeQcK%2B5BdYjCPBNJmRSU89nZxqc8S08WhogZCqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM9eawOO4jKwSxb2JQKtwD6dhK4IIdI7ODs08GPxlP04M3D9uBSysrZBc41ktpbHoAmAn0Pf490hd3MoO30Oc2JHl%2F8ZjU1HYp2LqCy2aPwXlUPWZN3bg90hn0xsTLBdLTc6UjETJl7cA25Dut1WT3H97%2B%2BuYHTrfKXYckLtvP%2FdyNpV5YjmLgfoto%2Bob8FvMaOMffG9AC5Ceefz11eJ4Kx%2F1TharPOYDmAV3Dt4TPV2hRvoDtLvd0WUE%2Fnaw95YdeB1aoDw8P7rd8Boa%2Bpua65MP1SYrsXRUd6eV8aKrxt4qN75S6kS3%2BPkMD6vh%2Bvrt6Vkw5clw8Zd5kIrPThxMx5DrQOyZe0DSmfXWm55rxjlx1NK4u6w8Fj3hOL4UomK3JEjbcegVLgOBuJBx58CD2uuRJsW0RgLBjFDScMLzuhcL6BegZ5hmWpFwQpYMiWnPCBEm2K%2BwKFpFgdWs6HuW3T6yRKOpspmeKvlHU15SW6hyUrkucietz%2FwwaeHvMx08QNStNXd17wRJzUZFgix6OmA7wEqQ%2Fna018BxcdCi3O4yqHLnqv8Q8qvi%2FbtlOpMMOjFz%2B%2F2oZaOoeXF341yetvvJNyDTUhOp58IkZeMvmSa693xlIA%2B9AE5CJ7C8tHr%2BUnMIzWCieJISPPo8w5JuJyAY6pgEh81cE6bhX1QgKaCQZljVm%2BHFZNjcvoM%2B2WQ%2BNUi1OesTN067MthV2R8OtpsjftC7LJV1q7dTkltzSvlvWEXfz%2FQvu9XGC3bCGTDvNBliXVIWUr0xqUsu6kIS%2FmYsbMr2G4ppRHPubHz7LduXiWBSwLC5l7fpCwo8TKkbktzuduSKHLnP3pm6%2FwQlI0SwixM0fE7hQUBFGZxwrB4wpgquYpF%2F5yBmt&X-Amz-Signature=9852f642e4023b0a3447a45101410ec887d169f0d8c115887fc2a8f4d7d68c93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
