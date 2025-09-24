---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWPNB4J%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUt9UNkmlyNKeC3evLyJdtd9LKVhgaRPW255ni%2B%2B6xEwIgLPJzQ8sLzp0cKdWaIECl%2F%2BdBTN%2Fvr1v%2BxWYL0jSk8%2B4q%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDII8K5N%2FQmcDqyJ%2F%2BSrcA59GxKsMVisWpNUJmnTa2fyWa3PbY5JVeZ%2B%2B9w3FUCCV0rZCyCGJ0ea9KAm8agpAiQCOkHwWKoKWtPf8h7mfgMZEagQUdkWUzTVrO06QdAsi6lOwO%2FSXO19eQn%2BpWI95V8cHx%2B1i3%2Fyb%2F4CXH0lyH8QaJiS404EUEGyaFHU82d%2FLquc1pCan414SBWV0tpBQqt7CKfxR%2Bh5db91ejXs%2B7kckifSuUHmmSm4qQy%2FiwMHc3nxK3dZCRGLHJYdLWjg1QqJMlts080m3RCxFsjGQ9MX97vlAOjVZhsRgS%2FsdcXycy3qr44bfyofasi5j7ztnGgjHfpWd6NxPhytBXqSrmPXxfsl06myNB%2FpwqxYKEpAsi5tQX5NFyndT%2F0%2BZaiwoGil1OoempS%2F%2FalFjEJ73Wm6R8hvOr384D%2F8Ps9SmYlSkDCv2N7TE2DxPj5epIrDESUAHOSdUK8kiGbqSXfBBWLQ%2BtkHwdcu0cpfApG0XMfJIgQSRUXc9vBKpA5eA5oUhLbPxsI9U5%2BPD2H9BFfN9RUJgThTw39SVHIj8vrq%2B7x7FCiGF06Eo2bW968BJg7iWayUzeDhoyUUtkxZP2zk3aMfxOv8O73EBoKXvKD6MgjpNM6SgqZWTNA%2FeYKzrMP%2Bd0cYGOqUBMoyQ%2B4Y6YUKZfPGmw4wD9cDjggFchB%2F5SEWtVidSC7moo4me8jUt4FUS%2BJKTBY8nfsSU0ph9r5t0RONh7KuMoVPlIz7ojQkaOfg%2FA%2FttbUH0jW2FbJ2Q6Se42jy8AZqfYvPvCofkkTXwJpY55MKGZgIU%2F2mghfFmXdX%2FFDybuVvisDh5A%2B4t7xAQPvEg49Y6RsDiXWPGAk66o8uH26e0f%2FZRDS91&X-Amz-Signature=b18424155c537667dd510ca5b0fd8334a25772c545aed3b9d345a3d04bf6442b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
