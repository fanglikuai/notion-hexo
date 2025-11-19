---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEJGWMH6%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQC4u7gnK%2FF3%2FjS6pqFV2SL%2F76GlNo7F6oexMwI3FEjTVgIgNebFh4COiSFZhorftJ0PYS1sbYhzUuh8HFBrV9pC8NAqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBsnE4FMjWnQKC3%2BOyrcAx1sNWwbKg2m%2BqwT8gVI8euQWP2o4fsQ5hhDJH6XdZ4P9BHmQOmcHnwm0lAobg6qcMz0wk8Vws%2BcdlQ9Lbxk5ODY6dl16wyZSCyasl28wMwDyMXpKrdmxIz3XQdPdRde%2FmuWLo4iCIVpk3xZVa6yssHm%2BpXzraUyPwj1tDUl2lLC1S1sPwHM7NfaFSHOUBP61pcM6BxVQqn27vELdSkHlTnJckI54ig9tva1Z1KMvqiRuUrBakkZXSvTC6Qa0JTBI4AlMQ022Z35mHj44cDXLYETMBENRK3uz2TNRhTu1MluJggK1I9WakMupdP3QypP%2BZiKC13gO1GzmLewTwW%2F%2Be2kgibPrAiFWqIxAN263EjVVgaOXhh4Nruy9HcTSDafxxyS309jR7QtuJdz%2BmiGPRrq6eE7UEDPYrZgoLx%2FPUBAWc3TW3eUrIv39R4OKsoT316u9kwn6MrKkN9Lqer%2Bz5tK1niqxFR1Rbbj2jLqVtuMT0W%2FchuGo1w8s9mE7Xr0Z4fPN96BSxZMIAvfMRBDqWF%2B5KA8tdtu%2BUWSeV7s6sNo6CrziRHtEP9WUoDDo0qn5H1m5VbZzM2gmPZkpFTJAis%2Fon9IBsL5MwfDcgp2219RHcYVhcq43WdnMqDGMOK09sgGOqUBhXjC7W5wMoF43CG72buiMob7Vbbs3bDAkx%2F52wSWOBOB5HTnJIOTR2WUJHmq4cGXFyrMhmpkyPa0tKioQ05H2mA7XS1e%2BFS89Wa1n%2FksmbrOzr3ih%2FGbxW8XekJgvka1nLardoSKGnlBymsBdyzQeHTOSmRi7Ar9SNjHVPYb7eHmTSTOdhffboEJBSrBiGG9HEK5LfhPLYgp0JM0ShBbY%2FRqEAJV&X-Amz-Signature=5a03dbaf8baab80e30d32552366de734e538afcbb93e2007fb39d55dc38494bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
