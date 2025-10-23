---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WCQJW7NA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBKDXahPa2hzvGonSdwAxPWSnCjYIC6Vw2IeC4qkweCYAiBH1TXuHVtiPS8p%2B2oUg7tRfBdtOWyQlVGduUfNs0YnISr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIMYaOF7yRB8ELLqeWbKtwDzxPxUB1i2z0sDGl88taK4Sm%2BRyq0ZNjKGWcpNT3s5VOpxhXV1q91TjzC307UK6kb%2BFE%2FzxcCEEvI%2F3CKGh76%2BukJjdCRdfV95P91QW9P22D4cdlyZLWwWHjDIayTIoBYBt8ePsrfTscoTyBNP4n6VElumeKuCFJjPfZhITfBV9DlcgWro7iJvCd1pSteW0DwCaiP6YTpn1pqif%2FUxxFpnGNBkTwdHIvdVSI7GlRMNyttCvpQ1vAxyqWpm0LMWqR9uyYhWPsKdDXRejo9kFG0inHfwYz0B%2B%2BlI25asLukdYucqaRO%2Bea5dZ4jCvRssOGzjHAg0F3sB%2BsAnFHF9ZOByTq%2FC%2BgNCjojAF3pHf2WzejrKGi%2BElSdf73Dg%2FPc%2BlIM%2BL0VqFASZ9PgN6xU1aDuT6kdu9Hjyb6R8VYUehBYbw83o%2Fm4MPHfwnTFDQeVCVbhCVT8YzdIXIasTC1JNRftGBRN5Cu70NpLoCZ8dz9CCW5yNN1Vyj0YWUzNHuRNFuK22ArS%2FjYLQakiZp1K311FCRpeo%2BfJvQcyjBbuvxK9oK%2BUbc7ap3t5zOUc5ycRw4rQbGaVXWGoTVSy9SP42c9wXL3WlZqvUzRcKWR1hgiq4BFU4fQP7Kpp8Y1Hwlcw66XqxwY6pgHIHFeVMcBmsHu6gTAK9YMEM6bzFkuYRedIiueNwCXx8f4co2o7CQ0e%2FOzIXYv0Ca3gmvANBk1pSL%2BeyQ6JV3H%2FeXBPooHL3EKtONrGbaLSF4SZ%2BAgeSnDcQxTVkLOt4sc7nUs5E%2BQeHv0Zffppj25k2f7mwmrC8EPYOKTSWmyPQBIoRxHYhrmJnMAbXcLX6rtGn3c%2FPba0oAf8JRXEIQ9gtCwW976L&X-Amz-Signature=010c06a486c38626320504ba576f0749819847be2a0118a3da14c693b63a66ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
