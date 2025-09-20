---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBNXEJ5X%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIBMvnOBBV%2F2%2BvuP1H4QmJgx5hnj5x2y%2B%2FLY8D2tRSbYGAiEA0WJl6pWkWVByZUXuozzMv1RZPDyjI56uK9anOyBPfD0qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8Td6dRtR98x7sZESrcA5GpDgHwlsVwWX8hpbvm6jdYI%2Fj%2Bh81zptzXWAhIksKqjabZ5oWbnkAuHFxlo3tc%2FFVErzRIdZ9MNeaGFQkis8Tf%2BI498JVYdmGsWmLG0gAoqILHdtf82VwkO30j0CRAEfmrub%2FAdqMbLLu0uvSFYl8A51wuzQmpha15GL9wChYaSL%2FbLCjR4AonexkwIPvVszmmMhvkwQObC2kBhQGp2uchoyfQ0Ph3dd1QJc52F65DX9NEKWUstxlWX56TdX2ph1nwtbuZdNTBlV8sM3dJBf7I8TiXMStkgGy18U0wvw%2FvAZqXuiaEeOyuqJMq%2F7Bqyw0fAuJKiiuNNFl4ym9WFWybamuxu%2BD57efkMp%2F0blN5bxqG8K3C6FD5UgK5XxoGZxkEkdYtj%2BANDaFQC5415SenWjOgiK8ASLUuIRQj3JSl7Zb3%2FcSomk4JuG9N2mCq8r5dqiN9Dqlfcl%2BSvHZzljq%2BtdfJ0%2BIzlJxrhACwoZIrK4R3jOgRDqsI3RZ3TNBHzEB%2F%2BHgBupCuDP7B1v1770LLUMCnwAXyprMge7lbr8aKaQQtnfFrYyViYqHk6XnB6XKahMS9bTAx10PkYKe20fggjqjJhiP66gdBR00alDPqcDyZ3ex8U9VHB0ZSMJ%2FMusYGOqUBFuRy%2Fn%2FBvsnxxi2xPFPuVe3jlSP4APXQ66v8L6wyjHRQ89Osz3T5VU7iv6iZGcE7xbXijoA3SIFzv5y8and8p376bHzhG9ML84%2B27wncG4n2cugVcLRk62A4EpoadFT63UjEmvO41Xh6IyaczbLdymCln3fq8zlC7%2Fb2eVijEbs131OHd4OQm3%2BuD2Heqqm3RyveTMl%2BVCumIQ08EomxxeYAMFSE&X-Amz-Signature=359abd6c7e6df71b459b8fdd0eb83389911cc729d063a899d15d299d4c83e08f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
