---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7HH6LI2%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQC%2Fh%2F9d2ElSR2Duhg1JbxzzAdEqJ04A8WHf97m68a5EaAIgKwiOLIx8h8hery8bM%2FteY2NPpQ1PCdlx9IpjrGBV8aUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN1cqOGa3MofZyLDWSrcAwiqWcnZWQB%2FnssKhb0cHQUBVrp0a4I4mIHHzTZDbbUQrRnc7%2FRodQfixEv68s5qssFYdJOU7K8DyjsoESrqkWnCMdz2fYgAMxQJVscsGBbqOuUxm%2FhxAbHESJ1JLDrFuwtS%2FEns%2FdthObmtvh2RWzVSZuSzYcx0cSqf%2BjeWdZJFf4eBoD8ndhhbMHyJBlGPAlaxlkrrepcy4wgnOxb%2Bwllt3laMZsd3Z%2BuZUZZ%2FXNSKXIe8zLUN6guvgasxmrprMcax2KDjiKGxSVu5Y9f%2FO3tS6MzYKzwBMWMLchcMNIPAaiuVC3yenyZTD7vVb7AzOuwOiAB9jQORJSYWoPq%2FamGvArsYIiDJfP%2B9utreZEzZXXWG24UylqjbrpV5NuAPCDk8P0uarzhinIU7sl%2Fx0U%2FLS%2BHBsl7AsNbbNaJIaa59VR1dCnv2QOFFdes0KeJSslMqvIRF6QJohorgm%2FZ%2F3VYIftHo0qOAoI6V%2B1z%2B5N2C49t7KVTE5gPzFeuKvRzEzhBeo%2FqsHF0l6VStuYVgjLkE5tCXwcRUFfI85YeYeA1qnQx6rliaPP2KAQfggAHosNkDXVXIpkcM8KQBktVzwfe9JLACnkm%2FW5BR0NKJFuiY6kabZpSWp3qU6O8fMLSbjsgGOqUBsiQbUxwpZ1G7qSv%2BkN49cDcjNWdEQFQwK2LPfYT%2FvjK9lSOWPCFJ9Ps4444LTdewXtYfyx6J4X9nidOdDxn8rLaxNPf01LMc5HRSSvw4AW82UFmYAUBBtIaE8rooRXKmHhcub85mQmqTSqqiBz%2ByWmkmPIIW3fTr7Z9d0t1DopoGuFzb8eNiNjmXv9IXeltWrWf8UVwvloKtZU%2FT9x8iFsLKYaJk&X-Amz-Signature=859796d4eefa8dc138d8c3195f0ccc345798e9a1df3295bff53c02ea93aee034&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
