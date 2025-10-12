---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OYLRZZ2%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T110058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH%2F1TAQL59HzyGUYMRUlheqHEkCfM5SCYImBbhQd2KP6AiEA%2FaQtyoApy479SN0sjJcVH8kBOaxGAcD1%2BtKcwZqOPSIq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA4QwTdZybk7A12aVyrcA3TVeWUYa1OBDb%2FHb3y8c9DGqmOC8f0i3zisQDBZz%2FQ4dhBGd3Phmi%2ByLJXIAZBcfBQQxt0TopLs%2FH2Jvw%2Fnb6pO9HotKXjczTPFQuYFghEKjSsFVpN5xVlCrvWsTb%2Bu4dmVJMvKAWHqVyT8XPYoKWnqb2D%2Fc69ZEw3z7ffDBqpb8zP8xj5ynbUbNb25zb86UT4jHzswAOVlmKgyfQ0LTML5xEA3c3vVXVpR2E8HmNA8s92vqnxoqU%2BKB4J9e7%2BunQ3bGGjnXy3eZamp5SpDp%2BX%2FsRREpMEaEziMnLc%2BV%2BDLFxatbMIjR17gwtG%2BK1k9XgNlN%2FxpYGy3%2BFfCMl4qTBsQPzvKgdc8S1yyBw2w07sHYoLKCLny16mIDTUO2yJ8vL1sJykRJ3X6cS1kJ64C5zoHMitg4KzzZCjcJ2UlOqHvhaUD1NpHcFLWnBzqxvkCO6KbWdXa%2Bkg1dZUSEgiUWqR%2FnZ0wgZia%2FLdnAGDPZMXSdn4HTRJcuTii2jpFvG3IABhOeJwVEoVpqWxdqiNYV6Nn%2F3c1ojlagg2p%2BLBGR2tCnPdVEQ6tlsATqkhcVCO9eibTgGts2hExztnmQslkJXR5Zr6zg9FZnqm0pBInVQ5%2B%2By%2F1zOBXjW2kdrTKMI%2FsrccGOqUBkCMbpyshdUEXhsxH6OOngsJP0u97774q8eFCSBPvrNTBmVGYdGJ%2FZmihWXOBi4tVPMunDADDodBJLTTxIK6CLv2GsvXIE3VCA0dnOM%2BtalyGhlIkVUMl7%2BvFWeaPNLgVoR9P1fPSOyd2H9uR33kmZDr51lfiKrxIm%2F%2F8znsbG2lPWd4mlaX4O2eMnPvInbBjPriHSYL3MhKYCNQancKwIKcqqFiq&X-Amz-Signature=37db7ecf4da48a4880fb9655285aaccb38cad41d2ef854ab6f47851dcde2dd43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
