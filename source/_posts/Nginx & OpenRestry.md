---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYQRJLH%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQD3QIhCuuXPidDDXI6OWPA%2BfaoQ9D18PruPJRukvESVoQIgOoXrxEZ8S6vQvRqWP1Jn1N6boGKP0m4SvbTYObadmbUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJpRo0L%2B5SCzESGqyrcAxDXSOdjvSc1s7yruYKS5Q11d0E6Ubi7Aj0qGabJ9D2EpvM0pWzwcVlTx18O1mD4Q1PE66imADVG52tsB%2B4JqLOl5xt1vihTYPll2CfZQIKY%2BQcz6L4ScNrCLfL2F%2Fbq0TdKRBdZQPoeJ0GCoztyGoK07oXo5e4B9OBfaBwPM93EaJEATeSKBMrd29Q7mY2UxDLr8Br%2FLq5nS1e%2FARCasdJ1J7tNPpJhgFkmYS6TeKAlsFj%2BeXI9XcjSDlEzRAjBua1kTaKmS%2BvzZjNgR0Y6%2B4zHjcidHVxfwDv81clkx5Wzzlk%2F2D7X%2FLP7kzVaXo%2B1pLxRE6tKvkL1WX30x%2B9Rl0HSzGWndiAYT%2F7%2BfKmpbrd4xUzyIIbx2Lnyi1znInZZEyIcQ6x8L7fsqLIu3q4jpsaRsiODnb63pFOmtF3Lzcs9wuECvIQ5095sY1UxpcOaZBx%2Fj1GD4s68NUaRkoyBxLsRHf5IrPSUv%2B4Q9ph%2F467ykNwE9gEUbZD7GCA9l3sTJufWvK34HKFstmEK0BYx4c5vYbGxf%2FcEjwFCdifY9GYeWgsukxbCPY7%2BZsAGqJs%2BvoLVlvUGYUW0xsrjZWkF3lWd%2FVhrDOJ%2FTPY8J7YQaqBO6R5pq1FJ3MdmtMAhMN%2BcjsgGOqUBld0%2BwNLw17Tc1BYFrnOV9BRrUkxuxK%2FQQDAVY2MAjqRRhOyRehO7rsGuuAPCMGdiwn8kxc46ZAqOt3z1w97mWRFX4P%2BmgSslrz6y0%2F97U%2BxOvrXIzyeEiZMuEYNvys760mFhJoiU6rL7RgFLDAYUlOWyuTE6bOxJs8WEE6w6cEzVtVlZ1AshBgNvobHjnvw%2FnYY7gQy1w793Fo4jzZWMN%2BJMLyP7&X-Amz-Signature=f5f6df84f0a79b17c1d510db650bdc166ded874ce3b3465273518c860d134394&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
