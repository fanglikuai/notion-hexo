---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVGPT47%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaenjbkvNrVvol3ngA%2Ft4cgdb4dPZrQkbnTfckqsn6qAIgUM%2FKKalYTDwTGkPXX97FV%2Fb6bEAZ2Ltz%2BlT6y5Ou4rQqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIN2cCWoxAHM0BeVzSrcA99DkkFeF3%2BEk9LpAlMmzD8%2BcRCbJUSoBodPGMpLxLZIhVJFPY3ZKSq9NGVkxu0QQQ9A8pDLjF5JKK4O0ffIbWTKoHdka%2BJvvGrBKhQb16A2zk8UBIyU9%2FjpulU1bS8j18KqJw43vc0qg%2F2sV7fvRKqZdR4QlD3GVp2fcGL26IaRD9cA9xxlsuqj3p%2FNVumIjXKEDgJGcYOgpLUe%2FkXLlweilK39jECnEGLlNGBYdV3A%2B09v0wy9NnMHVtnQ2sRQpeEsSSuoQoGfxyQcPi8msTF5rJff5nyxa6Sd%2Fs3y8XCy2KcjyG0rxSLvhxzt6oEppKxlj8hBmODAAAtB04UA1%2Bs5mPuKetgF%2BixfFpmbgZls2CY5WSpwkw4S%2FrLx7mSok5TJdlwUuhxBU9De%2FF2uo7mEcRxO3%2FsMtbzSZw6oy%2FQO%2FyctPeVN4lqJwjdqTyaF%2BMz3ztdLFZeKtXknJnVkHkevVnETeULCeg5BXXdfmAPoQNeLc7eLTkKyhhAi9MOuOgTzZNhMnnu9XF7FwZeEpAXSFMTqQ1HxZ2XP0jh7FXeTEaeJqV4yFlzPLni44zbbI%2BOOGWM%2F2PN6qwtgiVUVYGYdPWmBtpn%2BFnzLsg9fJFNaI1UzR5nMOx9SaCGuMIbrmskGOqUB2BjBZC7g0djdgeNr0lD1RVwjVWxUhXqP5Ipl6PbCMlm1F38RJxpPFwestXOY2ICL%2FNV6rpjwyoaAAqVVShiY3MuWl34F0P7m4Ydq7Q1CpO%2BFay%2FN8KxEBkZyJrc0eA1NCjb7tmFRoRsrSS1slciQzAzoFGVLje89hXyqnvKwftOsh9IWqVThh2%2F2jz6%2FtH7%2FhT00OXqznrzRX8T15Y9V1er%2BGQMJ&X-Amz-Signature=d143111a0303ef20270ca68a713e8e6cd8c8ee2962d6a41488ad1886ed0f3c49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
