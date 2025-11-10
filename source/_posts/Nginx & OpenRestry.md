---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665N6WAIDN%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIHHrD2%2Bu0R13SBuDbZVzkHK0k2i%2B4FQpxxzzA8K2QDGUAiEAji4wxnD4dc4ZfY1OyXgI7mRSJUKeNXtS7yPc0gdo19Iq%2FwMIDhAAGgw2Mzc0MjMxODM4MDUiDB1P1JrZ%2B7FZ68vK4ircAyRNH7yhGedpotG8hSqqJkAT%2Bm1nEDsSDUr3eVA0dw05NHNqFHC%2FV%2B2M3AmXjYwuLisoD9w4YhVW0ZE8BJXyvd%2FL3H1DMcLL3QoLfM2eto4EF3Zjdj9Sirw2WLMPQtELIl1kDBJJrufDB%2BNp3y%2FoLKfdT%2FgqYdezJ5E1iiL28HHodUWXMgo5n72iY0jYrMi4Z0tXsws1yXcacvAjsk6KfiIXRmaiDhpv2sVY4HnXuNI1S2wqRR6hOptr9YHEEFdaI4W5JHElhhqmJthaoYrwMU95IiXjeUlwtKRCFMGsYq5N8NB0Nxvw3J4uKPrd8gihE9Zg%2BkGXUnoAhNTb1MJBYML7Ii9SXtIjpke1GKwFfoOHkyxZ01TBtIZFCiigPtRfdxS5bYEen0Cfm4u2AdMMvVhhIHtxGybg2Y3nOdAsG24tcJUlSKjK1stsmczN%2FWIOKC1prSMUDG5sXZH3t85rlK4C2JxyLa9A5xXpgvvzHRb8GOJJpCl8zgiHT7zTYuQT2MV%2BXubOHyKhIyKzwsd92E9TFKD3uV8x82xPOecrsRJnlmAaXqE%2B2OvtNn67mGRZZQVHGd43TJGKJM3K256YnOfx5BaQWIW5HWAagyvysP%2BDM%2BMfjKxgvBPQNnAYMLmpycgGOqUBYQNLTo8Kgc4gWagBJuma7IjqDUZ9y%2FYSf8R1o7hFH6223VrlU80rR9ayR%2B0TwmYshYol5RYol0vzk0y%2Ftt4EWvjXapEni%2FwFkl%2FClpVvtRfsPn%2Fk%2Fz3SUrtoViGh%2F%2FO9CvCExPTCBKnMuzYwRL52WQyBwE5XMhWz8jEZgziowpwK961MdcXVh7%2F10wxNIveAzzs0vceI1a7QwUqKayNHTMlBUZNF&X-Amz-Signature=4286e73f3333ffb38e996db881147810dd1282bc04f04df96e8b0c8951ddb2f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
