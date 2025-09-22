---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOCHLPRX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfZK%2FfD%2BVT3cLvUOmeHFccVv5Gm3WjWn1LGtNKaShOPgIgLVxiOOzaKO9GvRjlx4npWLlzlC0vn%2BIWa660hcBtzZ4q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDIYQevxYwZX0y9G%2F6ircAw7VfwKSpc8ssmc9tWcmGsXbjsK65bHDp4cgrGyir8x2C1u5rQpJMqj6K3X64fiYcBrnvAOL5ESfSK7jgaZp4sM0R%2BWhJfyyfgdXSao24kPbUDKTanLxuYlb7BJjyoejqY%2BKjm1qOYHGJayZKSXLz2BxpdkdWlNVa1%2BpYoTpXXpWLEjLgb5ft40TEMb6vdp2KZfkYdfggWE18KG5kShcmCKl2ON6Wr6Rdf%2Bflni%2BvHuf9GWgem9H7qvm%2FYPKJMvioUXdng6zEssKc66QCHf%2FvgLign6NRbBJHAPdcGLsci7YOacGcnSnJiQxcWSIZVrMFxJ50HZ9jX3RO%2FrKXUGkqvPDYbETuTcDwJ4kLuIcPUtsZpabNxlquzq4NHSzIIyUGq%2BlkFraa4plJhpaU9bAA67brOlnobmi%2FTQqK53YFL1VV8%2FcG25p0zsg4V7G6ZJUIDUobi5kwLSvsL5CAFemykO%2FfyLsPBATJd6lLKfT%2BreGvjbdgR9zE95vvNcs4r5OqVJIDiYRmrONNT5609ZtVVm%2B2u3tngA9%2FOWPEAeMhJW4PBwpoVOKLaLb3PIol7GB15lvlWY%2FaUdlnS9QmV7z16ymPV8ewZrmwFjVnSuQAECWtuR6RYf7cvaVoQLBMPLfwcYGOqUBWAvUc8YaD%2Bd8%2FFFiJICAbyOV18lbNiB0PU8ylHtwQIvG7zdyTdX7epHnphd6Dad%2BLcMcYWCsIdS8BdooT2m75GFYWmHauU6Y5sRB%2FrNuchmtb2nbDIGc6bNvBMdDRhpP3Pu66i%2FVSpNYpg%2BGDlKpXM4SFqn3nbGU5d1F6BLMFlHv8U136tzTHIgwTOf6dz3CUAyKIsnSGlTgWwLrEf5mNQEl3cxS&X-Amz-Signature=0868ab0e210d94ef956b4eb4016b269bddc23fd78b4208ecfba72c582bc41802&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
