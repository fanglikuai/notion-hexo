---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SC7FXUZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAW5NnIKBXo5Sqg33pEbBX60ZCIAPX%2BsWPHCScDJngIxAiEAt7hsX7Ou7cHz4QYuVlldIwahU4%2BMpzXImEWiNyelg9Uq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDBzmmSOL6KfN0Nu0pCrcA5KE1yf6NaR8bSH6%2FDdf4WeRnaispuGs%2BxJZbWNTA8iG%2FQTIcj%2FeYU0gwbZxcQfIPVKOwjoFwRQKQbhWpSFOMKLgh9OhNZ6Rj1MbVaMZ9%2FymoFmSTnL7nc92KSWQLiGiGkVYtWufKvvoWj%2FRm5ZvpoiFTT%2FN3X7Uxq6OS2oaZjqIA9jI2LGxrGNS8Kko6nZwbnuaJAanqoQVUhn1OyplBQEupyngnu5EawS%2B%2BWR4gWAo%2FxQi%2BcnHdzsa57MnCPlH6zRi6kDUgZPNd9NaHp9xJhlD3t%2BwujymQbeVoyeWb3hSLieTHrKT9NJpy74SJvgwtUtuE3tPzmXVM4OGUv7m%2F5s%2FtspUUqiW3KUla0%2B1XFGfqeHfB5CV6sAan%2BBdecCmDOwE1N2KlarNpSn8QJzKnITN3OQOUYtxh9ClQTIAIpW28DqWC1CSRptgo%2F9slv6f9pTuQPV4iLKqdbl36ovf7VVfkIMBfvcWpVt8ugUmMmxSwGXlc5osUj4PHCWZgBo7OOKLZuY5wCH1r0Ju4XFhH%2BTmb71PBQdiiU8f7b52SZkhNfCxcRv%2FesH%2FNOuE6fasZdyCtiTgvQtv45uKLJ6MnbnUKFfkXjmRqwLFQgUUYXTyGqFj3uOXlku7d5d0MP2kuMcGOqUBCXa5h4OQffKbA2EYfgcLLEBSXqe2BlVSMLRhCd7OAGsFXoq2XbRUjFbbx9BNXdVyDlHfk5spslP0Gm3Gxk1adVbQFlXj9a%2FhByX29uAYGwBQA6gMA5KqtiHryGo%2F6ohQ9BR1kjhIeMhzEhRzZAe%2FuaTm9vIKRsGV543Rn2evKg9hJPUqmp6VVFYdZqaxIus12q1JyzXXfuhXiAqT9RRIlzOiYc6Z&X-Amz-Signature=d9c316bf428f4edf0cd6a3891c52c8d63453ff78106654c062fe92669f653888&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
