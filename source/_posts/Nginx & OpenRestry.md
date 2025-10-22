---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YH3NQLYG%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQDdyNXOnL7ZmnA%2Fo2wXKl%2Fz1QLJFzmSMVAFdYVT0PTQiwIhAKg08bQg7yxt39EglLoA1m7YSlw0pWnCkaQzoVhGuKilKv8DCDYQABoMNjM3NDIzMTgzODA1Igyf8PBmDu%2BNjhnRAvMq3AMpnQYgAwDcJZ0W9NiZ8UPnawlJsZa49fS7bMJcAgQSna%2FxgynaK%2FopQcjy7a4Ng24hbaaoTj4a5YUUAPmsyNfMKC3l0LHrtbyMdFWbh5%2FgJF%2BSVE4U0NxBq9UfoJBoPVt4KCrp3Ndf9EfNqhlCn%2BBO7Stv3YeWZWplwJw7NsyMDRlzszB%2BDctFXjU7pif75uRn%2BbEiZ0B2om3Cxld1V0Pe2jIlS70RhhbK5S1ePCYrN7456NI33fqhBnHJL2M1dPNhLRcUmX25NUb3Qmh4eKc7m5kVQc2j1ZFWzoxtEanTNNyD%2BJ4Bk1oFpzBbK4pcznT4kDKKfKxSg8%2Fisyv0tsZfsI%2BrWqjBDyUx6W4i3YODS7OwxSfL7kyy0nw14GR7St298BNDNbBcxY47FXDZhgwR3KOetJv6iagjfpr0u05%2Ba62BszmlIss2Kbxd9ZEpPAu4DpGZ17cWIehnRoeZs%2B1tykEs8YB3t0gRP3UFGTjl9BD16QDwiX%2Fzmkr%2FAvEuKFuLz%2F2l8iwcfRRnvNHLxdmfGWOkeDUEnzMuhhUUrsYf8IZzXecNJYFSy5RA8rQMn4dUW21tq9GroNihGnnjFhcU33Mcl11MmTKnjhW8ohqCaH%2Bo1rK6PJk4fR8hyTC%2F%2B%2BTHBjqkAYYueqriDFIkrZEO0eIG2UYv3z473ziEtJpR0fYHWTMdJrdUBkhtpaD3pib6YD6vde8Q%2FZonuZD9b61tztlHQI63yDNCGCDgmdGdtRwnCmTB8fuF02akSr66G9jvsP07T9se18m5Rdmy%2Bv8SpfFoBEiHYvpxploIzezZ%2BgB7JeuIBmhzuVa5T%2FfsR8%2FjUHyRdV3gb3kqsBqm6P7CyI%2BRI8f1WXuA&X-Amz-Signature=ba9bda9ba08b185f2aca5e2051b40d8460b0738802ec47ad71a224f9faeed76f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
