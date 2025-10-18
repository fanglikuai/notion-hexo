---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WA5YRE5A%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIDOtNVtRE5MsqNjr0zYFZUqAahyPvY2fszxZxLd4YVC2AiEAovJArzN3BvtIizu1tVUHdARcGun5Jpe1QKzTx2p1%2BmcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtOHz%2BkVjjfFZrjZircA%2FEqEgatkJZnYGZ%2FT7rOtKD%2F578M%2F1%2FQVULUC26gP1fUciBHuV3RL39%2F4diiFOewpv4FfIU%2F9%2B3TTVrFgUJY%2B%2BFhjoLF8hDdCsLvgPJzzGKtkdNjaMHS53hY8P4z5TGk8Fm1yt8AGyZQ6ebqX55I9r9FvMzY2NbRqSpAEGS8JgZHub8FOy9AWRNOCZd05BGcELXDNlMz7EjLReZ4esiUxhwXI%2Bx4c5EcrCL577OALCxNUMCsI1AWPCEltYbUxo09d9J%2BC0bkUSOnuMgWMKUKLzcecX47qo6cWIfkV32z%2BZpA93K4yHtuLWeJDhjKqGZVPGeDOC1n7J%2BgtpkhmSI6qlvq76q85EPc7ny4R8ePigp0aR6GESVWxUeLzhtEjP6UpmMj%2BaUCKn8q%2F%2FoVO73Cgu3%2FtqTMTP9SoUitADkGCjUy7HPRaNl7cqNxbvEuC8JZB0NDPaKMqA61%2BLEBfUXdO3Su3mNev1PLlTI9z6jM0MvR9QPhEeOymQgfAWPU3ZQa49eUahxHn9Q%2FqB68fmqBTgWP0LEHj%2FHMbbKELL0McU2f3qIJ8NS8aGYSQF3sqMHZW8P00qMHWSRp%2B0HcUfROwseSQ%2Fooi9f26jujpKuSgFugkcGsnSTxlrqS0IKeMMrlzMcGOqUBC%2F%2FlDAq1DezLmMfljhD4DE82lAp0HHykHqwFly1ZBI3Bwpx1ElO2UAzykd02%2FxsDcd9xrM0dKMlXxq%2Benf4ma1Pmn7xToicBlhyUCCUuV%2BWV9lv6KL0uclp9bSJQOtIDg2tMedNUwuw3RgDnmiD3GYfT4Y3t04yswzuw%2Bc9dk9v%2BOnq9Wn0IC037TA2967rAvh2O%2BH9Dt7LuvjOe4St76WBVhZMA&X-Amz-Signature=ebefe67b44cbed76e943fceeeec32123666f4a21782bd5873a9f98d6644f7e16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
