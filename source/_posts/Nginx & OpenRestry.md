---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IEY2PKH%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFqP2L42%2BT9COB%2BT0b8pY%2BvEJsEkYTccOdO4rVWTqIg0AiAaTzyolR6r%2BBHQ3qZTV7N%2BTMFZPCg1djhs%2BYqBmw%2BPhCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMyG6pDGX4a2hVh7KkKtwDGGzRDa98dJS7C%2B3AwOlfPF0iSsCjgbDi9m1KZHd2GSiBCGpkn0LPPwEs9QtO6nDz75Iv0%2B%2FzHjSkml%2FDphBJObTT8vBSNkZ1CY5qppvp74EBQJpLOFeGfPQ1j9KYVoQLbHYRW2uBnJwmYWDczn4QC1AQKsKUx9IqHoyMKS7Pl65JtjhGCUPXjzgQnplFrhTVbwG6sTnqwFE5plYZew%2FmJ4LTN7M7%2F4oYs8N2SaiiuX%2FeyijTplye7YozklZlBePew6wyaXC4ep%2FgJBvzMKp9sBcm2gHimXplcj%2FFWyINXiOsowO6Dg4lIvPIabaDdkaEUwmMnTt1J73D5lal%2FtcXn0Mh7Ps5SwydAmi0dz3txPGo4HypPDJceBucqUdcRPmvrwEAacEoRIktwzya6hbuIClsjyF9A3IHxCmBXbDnRD6tSYcSOXGo4g0qgvx0irg8BIYCQA3nNRuChBCtRcINMMAqGhB4oCOuKtizYY9Ig5naImLuYAMiMFBRpi4t2zRMAz1EmRzLPgAkqJ%2BzAzThM0oYNq5eAwx6WFJDS7i0ahpWIdzoWsQUNFdjel4E3OU7glu6KsdGB59z%2BqyYN0Rks%2Bpf3JAvYo8jEyalDreDdAbB8f5xjv9kPUQTlSswpNPeyAY6pgGggkYAxJx56QsD1SawE2JrRjENHDVbd9gBbJb2wS5N8%2F11PdlBaiRcNPU1OrP8E%2FMatges6AsMLYhDOZjqu7ccCUgtFkZuR3T6D%2FNnrRfEDkLLWae4p%2BhlOdzuqTxjrqn47KQehkSOjpD8f8OXcshJmbTFTjCR363opmYbFC3p7%2FdIBHQQrNq8r6z1P9dwyJ23o93bpFPsL7aPXiiqSNiBb3wh3CBs&X-Amz-Signature=6beccce4ebc38d04cb6f648d10d02a0229ba098ef03ea93731b846183bd84de5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
