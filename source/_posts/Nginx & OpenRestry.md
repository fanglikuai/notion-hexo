---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CBMTVW3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDv329Ddn6vaJQ%2Fxq4yti9v8WWlQ7L5t1H4HkwvMfl%2F4AiAcSmdPwGNdixSkWERaAtJDr3RfGEzVN5pJKYTHT20PhSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDZtsPwIwCt6nOOJSKtwDxNZJU0P3VUsjS8Oz0HBM5MyAFaFu87qoEFhm35WWXGWSiwlAnRV6fn5v8K225r5d1qObcmCfpt%2B3xMLv7Fu4pOvBlCh0qc7FFpW4f3JAXRi%2F81qnrjUQVRe8xRfBYA8NkqvU0FaIJgUK7CrG%2FF8aM%2BYURBOGxwcv6hJQLdSEVavU2cviJhSEVaDAimYLlLE1WoW3zPQ3ULJMyP9GTsXoJotxOVwDBjlxqYq69wo8Zz3VPa%2Fm5sCil9XWffSIPXNujjcb3syDjoU4SGV9cljZF9hYaYDTfUSASiDMnOnWTA2gx6iCZLQPH%2Fa0ND9QJwPfZjrJcN99w%2FMX9hHtuR2pw7jjnAG5lDOLnLfFFfhJ1IuM8GjMrOp0x%2F5Vsu5kOTpuT6F5HtKoHRKJx4vKLaxcqmswzxtmlLyufWW70wwbZlRrOy38wI0Y2Ae27octnZ8AOKhYPQPRvG9o%2FytiwPZoqlN5bqiAHhKIN%2BD10f1JSyqp0bRFaMQCjTrY%2BMbUUYRiIXS4ZZzmX94SAIq8bClfVshWvkk%2BwrRaXBM0csIYIOs7x7gBb6AUGs9vmni1WnvPKQwXABPicAxF0M5kl8ZmqfnrV2flxcHSFtvqLywp7FeWiIxvn8%2BurZcJCxEwpM%2FDxwY6pgEK9ym4pTETf2GQ3OqbnWeOE74Vky367SY3jZvm2zYDeWCm9DrFldlryNaVaMP3kZNWJ4mnt6%2B2L2HFRFlUtnaI6X1X8abawq1OUZhmeYhds95AW3mUU2LCFn6hYGelmV2JZl34vHRfFZ5CyHe3OmWgr4rx8O9qTUuRV0IfwcnXdkjCQHb6YVqEm4sz55Fa5EaCM1W7kEN3iC6XMiYrd%2B6Z1LH9M8fL&X-Amz-Signature=db5308452936202df7e09a73462cd26b283a103405eb486afc082782f3ca95e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
