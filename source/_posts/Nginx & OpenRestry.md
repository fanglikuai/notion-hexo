---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMDJ27RA%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD991i8FfC03nmPADTRRyM1H55wyWVeo1G9dJ%2F2IFgUXAIgVaaN4SywtJ5bVrM%2FKxTsTDyDSgERBeooP1FKK6hip70q%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDF9g3J57ndoc8z43QircA33wxkgF2lEn4PhG0wSNc%2BFd6Wsq%2FG7FR6IaWrVp0o%2BrZopLkP52FFdhahicLmCvdk3H83VVPOZY5f44ZdRL9v%2BZ%2FmiLQrbC3it58M2DGNDj4QLBsGbQCIKnfnAWM9Q57ePai6QHwis6CwOPQL2kjuiiUxvxnqcrTscb8BgP3tX2TjtgArqtYYzeGomcI2XNQrABcD%2BuItvKjRAa%2FqIqKH8oOHlv3gTmKk8iSyE2qxLv8hhUuatEDzpUdTW5BinMk2DM4qVSJBj1b5Df73371ejpp%2F3WgU7bTpzVtav69Ud4JwJbM583ePCRVT9zgYph1h3i2vC1Vi9rIBh4ippM1SDcs0YtHRUcO0EkBeuqmN86qt2kDN0h9INPaL4nQECqXDY5OgXGkAKvvXysiSGwq3zkBJhTXgH9OOYK4L0OlawK1dIokVMkxm7UxJHy8oZtV5JHQLbc0xKKDpmHxtoDEpEjXUFjfVVMy9PG3Uxb4oZ1RDcIDPwimEjsg5sOawSQHWNcj9%2F6ZBFDuYq3PWv7ejKuWBuyGi4yNE%2BvytI3NHdZyI7jU2ebXmeZzIMpjI8zgXMjL7GDTMVXs2by01%2BaBAzy2Vh1hQbsp70sr0JLr0e4UOLa0gkTvYWkdm8uMJzEy8YGOqUBpjrpOHVXF7uV0ev3HiYpn3AJOSCdazIA2FSm%2FVopZN8k%2ByYlkww%2B0JmR60%2F10cceE7jY0gX8V%2Bffby6d1WcOQSwPAz8Q8f9h7vt3v%2BX2gq8%2BlquJatFlUw21qK0zx%2BGNK433DeYPWlZ%2BwphfNBVGjUOSX%2FVaXUyimIJp2T0nw%2FDzW%2BajZItPpIpzI1bQ7iEKPc6cZoeUEdHr8xbq%2Bdv2h3Ef0nmN&X-Amz-Signature=549d9f1b555d157b4bb88b7ebae79fa3e824503665ddf7c10de24de9aa8dc715&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
