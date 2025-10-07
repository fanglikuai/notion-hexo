---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPGDM3AM%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIDyPoZOcni%2FjoCtyoSi6ijPCJcWKmb9cFptYJg3AOcZ6AiB%2FC6HV1ANl64Z7UZ%2FaEfpL1CKzoC%2FYg7rIAl4GBocMeSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcDFdZNDUIF3bveb3KtwDi%2BwKzsgQSKCTb6pSbynevqJ1H%2Bk7P5irY5baQTddaxGIvF05lmkWKrKBUm34%2FFwR5789huHZ04SkyHdUofZUUnWf0H64BvTURYbLoA1i1KBSjhGmB0YxPbHY23AoJsZ8PbjwUhClZMrHb68GRzgAU7hQMpfD5rTU0g6gPH%2FXEQrdMdcau8Az%2B%2FpsHHRbuuAjNYvo7RXzAk%2FlTFlVwmZijoMTfg0r3jIh5wO%2FOnIcWFOVl84K8LzfprwKfaG%2BVdnN6PU25ZTYjBl6vgvFNOGB39XtMYtgKX7dI5Rq2ifsplM8UbAT7m1iO3czcSzaV0U95EppvwLR7OCYGiXOSmJDcrUnxN07xsrr1BsESFiPNYsmgSpFq13PzwCCPKQaOD2VLdBxcp8YJiU2ke38Uk7LOcmP%2FWK7fn3Vg8zBMdJ33gbidn1kgQhfun6zNOaNXDbZYwdRbsYOv2P4LGmYk5FIBzn0hPDbR%2BaikGgUonbEGOKlCm9N2J5SFZZOnLGTGQSMO2DM70wQKosFB6ZCQOQAoSCLuBP2n9%2FdMZeH6JHhuY913icZjpkumxrlwSlaZOM5oC1Ay%2BYLAbcrxQnKddlnIzXw7ZxzwBXLM1a5eZrLoLFCixNiiaY8S2eYB5oww9WRxwY6pgHftiEz2tUOuAD97rrDxuz5UPOyUF3W83f9GeAgVmaFm3wqQtGt73ielpPH%2F5VXEhUb1psC7RatAiSyyMHka4eVjDReko4NnIAgs14znDrXlYcvGwWbYgqMWXIMHEHwlJnwum310mkH1KcR5j2Jdo%2FDZedca3S%2FoKB81jevS%2FhCazUZt7uUv6I%2FH%2Bq6kDVr7nOnSA0dMu8sQ%2F0LauvMT%2B85l6r5J33c&X-Amz-Signature=939205f7fb315cc7361b71e4d8b191e7e8807c599bbf0a0b700fed6b9affb0a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
