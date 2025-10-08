---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IB3C2LB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDyrrUHtt%2FkETswBD%2FF7OntmaAuAUKAumzZeaETTm5ClAIhAKUzn6Wq%2BXNPcytZ47aBLz%2FEWrYoXwh8sfsfTAb%2FrohiKogECL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0IGdQKfSMbCuRlDcq3AO8t%2BgSa4hheSTBFgenQ39rzGhVbVJJVsCpXHvX3zO95Wdg37c48LrmsdGdax0MGiZmai63fE0Gnwr3l7Ri5tDfff96qZIqTzCFhZ2EEf2XmlvM87q3VqqtqmG%2Bf0ICtbrJ7Q1HqRFZRqznGAQG1ydKv9QmsQ7ZOrYc0iSY6GU8lEG4CY5QG70xwLhpardt0Sc8b9xTBfAK%2F9gvjAPsBQIkDuhJKqrVWwEVC6khusgNaSCTvs6pWxU6crMmBAbJRx6IiAHR%2Bw3xSJUYhE0RucL2n3GqQeSL2PGY%2BkHIDhWbqJzSJHqqgL0BlU%2FbqOYlAc22Ihsva8LyYISANSqtPpzm8oV2eauEEBNx2PoEtPDcRb%2BdGeyBVwCPN2qVPNWAaIqSEXnkm5KCiFT2B8VafmNQwMPw65SrNDg4k6V05Nxw4N7Xidg7RcSSlJif0i9%2FjhfYTui2lhBcM6t5j9x35YY6OGn7IT5Bw5ZUMeNSG1eoLrRt01FBRK4u18PRKbnBIzdSyMTi9jurnBphdLpt5Cj9m%2BvXsdSEYWHvmBSGVnCviyBmtToWs%2BNRsWoXESvl0Xz%2B2SWARbKc7qV0YlSrSfOn97F42IQXM7taTOSk45DL94G553TcSUhfDK4nsTD42pnHBjqkAdnZmbt44vWujK5f7ZcQxFUm6sp%2FV535lTLOopWxzBmdUzZ1Y2bAODfZo59FhXGNOMRdaBprv9cQ9bESoIJS2ottl1fEBh1PFEqKBYe%2FumBZAt0BsuGx8LRfvSv0g7wIF49tRZKIHhQIQQKXIw8aBaejVRG3tANpbc7nwwEJ6nmN1tHSepQkrwTqEO2JNyM8QQrL4Ac6cLiIXEY9k3ROokZ9hXDI&X-Amz-Signature=1b39fc8866cb09c93df843148e6c1752fa63315b6d93df2d63f0718204077b1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
