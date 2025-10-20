---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U36DDQW6%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIH9pxv%2FVqRkKWNL7%2B7CjUBNsLAdnILHePfo4gsKJuK9pAiEAoTpZpRG4SNw4h9cDQiw5aThfRS6%2FnJ7ICm0JeOzvK2UqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfQQhTp4YhzNfNwaircA8BPtPggnuM8o%2FNgQgMYKmJd8zE1%2BH49%2FOy%2Fv%2BF65YyWP29bEoK5wDxnSCwYP3scln29uWU4RByrimX5vDoUB2Hc3puUThXYHEPthcC8CspSmBx7uK0bZAobCCIV3j0ZgBm5C1FdXzjovqZ%2BIoow2FJpf%2FO40eZrEa84zzvLvElS89OwOsaANounzPbLRTNQwHiBSsri0Ze3E3bRRv9TQgArSX5wEK%2FXVR6XiXrNNdGDh3Upk4XWhIdYNCwDsmjVmPdaopUHKAfJaT0O64QEoZQ24I5kvlNpdyR2SpBDz0MtZlgD6iityyu0ArXublZpSM87gMOq9ETuK1NiyO1iKFO69jY5PSBT2OsZi5rfVlNwTDZrAsT1uTK%2BjHgwcyjd8qFWIuDLbQY1K%2Fd4kGS8VS9UP5FYZlSyuhJvjS5xrkIsLK1RtAMyQkxn1HBM0cUYb32hFB4ujpcGbA3L1uVD6AbK1OqggHb2aZUQ9U5Uv2kNORjE04DqNRMSQXzdXdEoM9hBQ%2FpJF7eaPWJ7rVqjSgCi4nvNW9Ob38cxmAoSu5ysP7G1a4sQqTs%2BSNB5oGwjCAcALMFTUcGdTMewyYfLRF5I56VKWX9YdX8o6VH%2FZMyFYtsur86FtwlEkMKoMMC22ccGOqUB%2FVWNmUuJ4NyuRqgLw%2FtiugYif6eVG%2BYv53MNxnRbOrofkeYDIVgef0lyjHdDL7GiHPnQ7bJTVBA2QbKn7iQEZ1QqluzyXSv51dYyhv6GCvr1ojHVIGPukOWMw%2BL%2B71a%2FdQMhzxD2oQBVUXY35Nk46lrqLJX5DdI5TZmGRq4jOqbAEFeQjKalz%2BENoGFT4scJYvarq1n4WoIfyuNvKlGzaiOzcIPJ&X-Amz-Signature=2943dedbe3dd845b0abadedcac2b955b6da4eedbf4941398640027adebeaf9ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
