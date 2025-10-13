---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637UPZEIP%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWLP6O3KOc%2FNlN0zLtZsaOUEODTyN6lnmU8m43n%2BWRfwIhAJ2kSq%2B1r87FD5bW6cGPik7r719iiN2KHhuaJkbtUhbWKv8DCEEQABoMNjM3NDIzMTgzODA1IgypUP9f6tnTBnvuFHQq3ANFMtck5jrTqnqHeprDwDUeTMReJJv2PQFxSdgKLHuZ7i5I8fwzBLzBp3viIpcOxCQ6GzY3KJ%2FyLAbjqmkHeAVhcQtor0Thk60shYvV7Q7OJkgF%2Bo465kz5IALzvqFwXvm8nKU9kBwCW%2FWiMbBUPepDq80gHx71nLasmHIq3tRG%2BbI0TfeuyCOb1GjM%2BeTH0NwuFpb%2F7t0zp61RjHrR9R%2F3BPV%2B2YPxfX6S9%2BXSokXinpJDe0nlRHtiBU4L9UFqAiT%2B7BlQbjGlmTke9wQvxDW5TZw1qnSzT9D158BSGOYTrUxmGrwpV99FVhOVp%2B%2F%2BvkTQSNnR4%2FBA2bZhYxzqTUfbuWTMxbR98s48PT6nLLPw8EHfu9DzjUTSuWqLz1htWZUaalRQredArsoWLtVlE8DQJ3%2F28hu3ajWDc4Qyg%2FBzvdhKcz6EhOICcO3bDN5RyOP3UjBuOsNLiqMF%2F6zyfMq5nOCbUqryz5AgRwUHnW0K1PMWqmuhbIgdZ25zLaSMnhzcue8d7N7KmB6XPSrLNXLGV%2Bu7Pgn4EG6x0RtmdBgd6m%2FwJNu6weN2mooCZB%2FowWOiNVIePSVEOy%2BQm5TB64jq2UvcTWwLwSZz8DsGCjiV88wq99VTPsegby2sTTCT4bLHBjqkAdW52geXZmLwwKX27L3mQlT8r%2B4SFmNzgTQzHcNFBbZF%2BECzTiBlwoSh0Kty%2Bny0AcyzeaKeRDMIzHLcMsndgk0tW1UiNp7jBfiQaspVQsWHvFqE6D5zuiYU6n3QnRAL3f0ix25IZmKO7vITLDxPQpaPc%2FignOlCEPM3IocyoKjIY6CK2go3ZdGnpko98RQAYPeDoYLOk43d%2BAATa80AzI0aIfQd&X-Amz-Signature=0eac17ba94fbd590e5ad1908b342dc9eb5539ceeeac5e2bf4cebad4dafdf1320&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
