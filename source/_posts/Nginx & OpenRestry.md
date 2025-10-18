---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHA3PO6T%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQC28xhc7CJkGALh%2FTzxuJ4tdZwsZ745huGl%2BNOMALq9WwIgSLo8PZzddqSUZpnaFzm4sUrVx2cgKGP6hYToULytY0IqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPv8YkaovwdYUdkpESrcA1%2BxH5iMSLfT8aIDeW8sWBPqRlP5QM7afUw8NrIcCRi9lDQfkI1vtu1wxS4veZsWC6OFGzDjHhbT%2B%2BlprctsAo3QLAgSB0YXiUCSLBGsCnBbbD0puPlUyE4uZf9iHRLdT8SN9XxBRiqH2cBoxuKrfwtZbplObe0VRyvzICXXYZqhQ%2BC980HLQB662TwxaacD76gmyNSg2rVyaC0N4OvCAGsaeG88m6UaxbJ0ClfBTSTIH%2FzvKirYSQ3gppeYAhaeEAeWfNuF9ixE2DdEvRiOzF%2Br2VowecnUQocSj3zCg3ZGhkICOuM2NAxvEp2aInMYNS5v1TrCgmAbDtoaTP5ognq6wOvWwK6P5olR50LY7wUQ0MwrgH63JQtLX0D%2FvFsYtJh8yTeS9QCI7jgoQcCGClizj4W4K2bSN3YtvkjUHbMVI7qLTe1ifkAfo57O1eQvJspMC5Kyyrl8QtSHj2%2BhUwqRXe7mVDVW4kxmHWU3cz0kjl30CvhTIjq3h3B%2BuWE4538gkCSe5Q0Whi%2FMHEKOZkinNol3Kh7wp9TUV%2BGmyJ%2FIfoc%2BJy%2BYLY6NO2erBjqqijUlRKz26SujgpA0h%2BJ4bJc9H8YQY%2BTwJtX3JzgMdfC3XhaJnY6AlL%2BBc9%2B3MILJz8cGOqUBcnirbiJqQRB8qk%2FfJnMp5Mdy5Abpz7YMzYK9Vs%2BrQoDtjXpM3NcLLPQPMIcuiNu8NC3LuACrtbrciqRWVua5ywbcHqJKPy7yYp8a6X5Lq48WJ2XTSg8elEQ5u0nMHkY2BTwQW7VjyG4LusvFp5MOjATkv69SDTxdUH%2FARdueUWpZRRnHscH40LPgG6%2BMLFrYvcvjOnsaOOb6kXDGFlnevE8MWnQi&X-Amz-Signature=beb04c2a2cfb894ff990af3c292fc183fc53278b3dee3182febc8690e99f374e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
