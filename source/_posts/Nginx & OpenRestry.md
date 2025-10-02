---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBF34QIM%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDuqQkNgyBoMA%2BxnZ2SVXwgwvsijwb31f8nfxN5JuVsgAIhAJBpERc0WCuOfcrnf%2Ffw7nq9nlFdXGEUUdAiUvksUnm6Kv8DCCgQABoMNjM3NDIzMTgzODA1IgxL3K2uWdhrX1jbTL4q3ANY33oLJvlBYgb3JCCxaRkyMFRBR4vTe1uooWK8HRAC2F%2FDJoOKXwJaOHLrLdwJ1d%2BZCuJeGBX%2FYpZxZfGZ9L9X1SzKem6ymoQPF4ND1ECU%2BMNEnaZRzxqfmPKAWXH13DtcLevCrlOBGi6tyC48QS1RhT%2BcRL99Uj9tnVXgIVKZu8tC%2Bw082ZnH9a66DtvPlutdQgCRxpcmd0vhlcEtsqEiNwRuzkLWLKoMwVLbWA3Dtx6n4Xu2FdS6ZAKI9jnBmIDcuSfWwRcPqJ9OPWSySLyDCJ23lUSjI9luxgDDZsyiyQwHgQKF5bCKC95zTNSg3Og%2BKnT6vGjM5PV%2FzCZ0IwaLFL%2BfUGhwe1Irnk4phIGx8Ty%2BVZgHs8%2BkEtDI9bKNwCfxtLDazqnf8HHScdPSIBUSMdY6hyk8OLLSHJQ4qkURfWM4gxTpQ4z9hGc58uWLLuFErUWNLkDWhQ53fgASYYI1nbuXxUwfPySoXZHPmCDqYzTYnfu4xazu7pcWGc%2FgqqiZ%2F9TL2jSZJywhQ6k7r%2FQcoHs%2Fwnbe%2FoxxPtYX6y%2BLuZDPGzetlc9TLjhrTButnZkXu0t5gsIC40G3AKY1kz0%2BSytCGq%2F4VN9W5JNHvgP5%2FfMVmRVPWudLUaFJKDCh0PjGBjqkAdArS12jJjOogULvu787x1apgamSNnryDJ0PSYpef0BKWqlEJjERtI2L5s%2Br9O753dXDeK10hw5hBGMP75ecAvYeKri%2FE9vB%2BoRjuC0Ks1SEqgPBcterzt0j114QezP35XVpNxbHTWKRyOKhc5zWbCYwluUAYCkCJi9cCAVdsjIqra5KYqQqBDGYPPtx7JQWUtT4QoXZWuCdQ8PfRmZjhj7c%2Blqf&X-Amz-Signature=a4469a6d3b419d8c5e295d19a6980ecb3e73137ea5673a3dfe634e7966f365a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
