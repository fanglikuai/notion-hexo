---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636RHJIT4%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIBcLFJiMC4fiH%2FOKCgXc8Bm8Hycm2a%2FjS3AaOmMHr6JAAiEAngnrrbPJoVglCNPZnSwFi1vO0tgLIsEbhSnj59x2KzMqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHDRiiEc8GwWJ9RCJyrcA%2BnsK1PhLwNNfz3JXBUi1Evj7pe%2BgfBRCQexEyjOlm08MixPAuJXiGks3HqgUiGkEIvCTZhpfVkG9yaXoE7qmUXJZNTlnwh%2Ffy%2BnCi1plIJ38NDa4maLub5Oqw3YEgSmuc1oJFGqFjvH45%2FacGify2AjRdEhFWTzBwq%2Bsc3tfo3cu0K4NQk0UpgQfDC4BHBmt10OI2fqxo%2FRmtzwg0P5ngJbZkOe5bo%2FoJxotO70zuumDYD7U6iXES0bBLQsWMU6%2Fav%2BBTVxmulZegmk5QMwR6d%2BnCC1LK4jplr58RcPwk2v4AxeBUUL90zbJ4nAKajRZa3tsActH9zMpO%2BEa6oaOO%2F2%2FBMd%2FclW4D4SoZ9c%2BYp2QxdGHQbq5JPlFq3VIHjV9%2BqIIVQ69i3cvSnZWs%2B8q6u4JToElr5pny4leywGjDqJ%2BU3o6ZeLvRgReC4FD4gwYENbX5%2BtqzJ%2BO6KJ90zb99KC%2BNYy2%2FL5E8Dvh%2FnQb%2BtYqeRntLaLR4hngUawDTUHe28t%2FubrJFfBHAVSSBpm%2Fzn3yNKQHIQ10p84EgaxUlDI%2BdBzZpxjyEw%2F6Y%2FRmc%2F2oq5NDwCHKsNIIPEEyTWTXXzvcEYmfN%2BkKtCJqYb93G3m3PYxIjMmyKGtW3iAMIrqmMcGOqUBYdgmvHgXRL7LFUUX2bAPrAwL51C%2FxFbID1%2B16UYYiDD2xcxWeAzRwI6z07C%2FouhDgItXABVzrBkAvENgNVQNmnDWtuEh6f9PLLMazhCSXfHy%2FqCluim%2BXkDupZ1Fk4m8hG3eWBQfUrLDBgRmVHvzVXDIWwYEADJAl7%2B4lDXyFPckEgFnq7cHjPUEBYqwlxRKVyVM9Oja3%2FNDmRyR0g7F3x88pR5%2F&X-Amz-Signature=368e364fdca445293be83e87bdac96736cd6b780cf8124ea51a8841d9159fba2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
