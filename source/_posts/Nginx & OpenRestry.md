---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAA75627%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAV%2BZBycphITmq4HHF4izO0TeDY1YsRR5fyvQnJalYcAiA0ri%2FEoumm1YDGNP5WzbCGkuSSCSBgiTHrevMHRSW38Sr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMdsA0nXR4l3JvOVssKtwD766QjXh5zLmKmkxB%2FfRTq6xynmNlf9WC5nR7UrN7Ih2Vh4QGBVwmeot%2BAdC9coQMQfkEFdkCYTkxvG8AeaoeJShJTSaEOnZjHMklgACnDts2sG%2BtoOEX0OEnqNa%2BLBu9KFOvCSRiydUEEuKuKxxEUe%2FqmiYLJSk1Zhzw2srmE%2FcjoUb0iy6XtXGBe%2FYoCEft3rrvqtPVwh%2BrBNzeDK%2BPUfRb3ZFF5KCaX05Tyo004mWfPHoYqSMtpXLEF7XSc85900XRemqHV9howqtVrP4jDIIlCuuCi3g%2Bbo8hp9Aznp54ZVhUofd2NsQv8wAdIfuHJp2RRyGrpmE1Ljd7deptbFCBxuqTGi2KsLvC7CtjcwUE3bABfetPZAr1b%2FHKRNmFymFeBySQj%2Bdm7aRKOIvFKdgzbsY9SVOpph6Bv66gyYNockUk4w3QTznuXMdPkq5Ybme5p20xjk4%2B2c8xW1vqhi6%2BkS8R2MxZbog4%2FVgxo0puxCpHFmn9TwLXk2aIBrqvsbTcJmgANkwGumaE5172BBvttlfytn0HBmBT%2F7ptEzSY3ovX4kF8pa%2FGJzrEfrv9a%2BSQbQkDWgyRLxUNO4un3bmAFsiWsTnHrPBu1HgRshGoFqcOfcBIgYlMxv0w2P3QxgY6pgG1NHCgwqoVYhwvh2A2BVwO%2BlGFrX5GjXbrrNcZ3G0GXmLUjWUaRiX6Ce74WrAMNeQ2MsqKbOgdTwI0%2FeB0kDnou1QLyXZfCz5W8iLswa%2Fv3yzIdLM3M1Kt0qKowQQVhUm%2BAlsQ7v%2FL8msZ85M%2F%2Ff9ptGu%2F806z1PkfdA%2BmyyWhAj5XuByWnQ7pcw%2BoXnNeW2KSC%2FgjuulR2PEZfVltg8lauvQ8YW5o&X-Amz-Signature=da0657bc82fcdd968e62c8669f5b4e0b5197b55166b63ee64c31d79d6b4a8bb0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
