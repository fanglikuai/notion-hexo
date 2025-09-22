---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWGV3OFJ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSyF4ZtSH2%2FQuRDOtNiygXtQCagiYPQGuCTjIYhkDkcgIgSbhYLI%2F2AOl%2B32c%2FOP4RHGLZ3JP2YHzsGE4%2Fo98OOEEq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDPtYD4E5b5Zkm0Yr%2FircAxoXlB5COHrbZr%2F073gmXVYtWYz8ckqusx1xpa43VbXnB9jc4MwaTVoeEn%2B%2F6J2%2BDLE7iEqAUSoxrzVFqlUStEW3qb%2FxkisiRSq908qzbiM4ycihIc5nxUvrtjGOZ1l0Q28gbxfesT5YP5Z4wRPX%2FTKPssq9KWQIhLGWlgSokeM3uCU9H%2BAz2OD%2FJfbSD6rIeSjpDfaprzw%2BOvrI0K3uDrd0PiMx4i088uTZl8xUb0311VdZQnqaXJ19pZf9uPgCPjnat4UclVl%2FtHaU%2BrBzh9lM6Yyvvf1KyOxR2tyTVE0x5YowifpnpCKsKjHZDNALHuNguAHcouHE6zADM%2BIesJq%2BwENE3JTuZa8RZwotcNr7Ph7YTRnSdifWZ9a2BSYIvSfeEN0yolStHlLOoIsqmiEGMKCxFeuWrmHBEQgtxsEWjw2gYXvLMI%2B4R2UDiJYf9T5ZpzBpgVSNnuJzQ%2FcXPjRrcHAJq%2BhWF1bwyBuWWoYr6GxO0GmC68phtqbZUqG8I926L8k2HIFYYqxBD4G5v%2F6wMiwhWF3hGUbN6Q0kQrNEdqqjkFRvuUL%2Fow1hQ%2F3tYJZkwTqDU3UtMFnSTooF5dOg3BEzO1%2BWHd%2F89GzIH7gkrh5NRB6VnZPV6O24MK%2BwwsYGOqUBLCQZ9Wa5rEQq4qHxgP5cHpeQx3Wy3so6hFCEEKWyPvQXCEBPi74%2BAOa2gPJVglG5xWd3Kbxgc3SudH1phC3o23cok9QBtQUJl%2Fd5xvTRVYWtVZBi4%2BkiRfjtY3E0C5iIgkfpC8ULShEmhNMJqIB%2B6d9hbR95XnE5ecIwxggQs8elZN1%2BdHlzlwJZ8NiXwEm7PfhN%2F5ef5T2esSw7ZWZfHcyA0USW&X-Amz-Signature=21125e617ed771e12a27c7e3c0ca2e99bb2cda84b67ef43d165a0061f9c5106b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
