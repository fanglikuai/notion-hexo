---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y57GIUZN%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIQDj6YwHCADhEw2BidYSrvSsO6kdI2R3DQnZHz4laGfBGAIgLLX%2Fkgs3DRDXWcMt7A8azL6x63rAAS8S7YcvEfkrOtcqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD0woJp81FQDAp7LeyrcA%2BxOrpXWaMb2rQSYpUQTcKQVx79%2BO9MWeOsu9yG%2FLto%2BQp9vNqw4qVl4HZm8ZJZiRKLNkz9%2FcmttbwK9GO3DZ7hytbYyI%2FOZvY2daHrsC5fCb820OKDrf7EFN%2BuwciuQ9vB99n1WCaHvT9UJUHqi7CNzOZyYIBTuAiRgq3hES7cVShpm3hHBOjid2VpWDhbvhDEanrUVPeA9S1QZ8ld0vUb%2BcnvqLDksuj5Vvc3%2FEji%2F1jM226ONtedwFAta6beTTFDzcRkC4t5B5jQGyCurkMkWbunz%2FqGvguVUWtCAC5Op4qsgYzuqD1Nh%2BqzWrg69eLj1WPYfkdYp3%2FHmpLgZ7v89abn9Ra6sWyiLSeGFwUjBxfnswTPbWauNxVdt%2FiFx%2BhrCvsucqm9MP3KxZBDWFotqDw%2FHdbRSXQKsFGFXnv8%2FszlaTN3bToeRhb5axLD5tGdpsbSRmEQKQRTG7%2FjiWX%2BuTMmC5E7w%2B5QToXGng8cKeWTDBtNcoDyJy9UePeTfRUHcpV%2FmXpI13Ox4bmAFbaHGJqWqQqOn0r1E%2BWPoTAgxgBR0Et2X1BXuXp%2B0uGzy2bwMlVZ7ODkeYro6K%2F3TXPAQzjTDIBddI%2FOKWNLuJQYPvznyWnfUpk31iohlMKyj88gGOqUBpBYlMKfPkLh0I8jsEGFWP%2BDYhB0n4uxJnohNv6u9Y8uypDPK2vMmbWTV%2BRk%2Bj5H0xsWpSg86mcJaMFGW4huzpzwKL2TEIbdBuVfyrLItMksHOJ2qizN3Eaec9arlbM14p7ezJYdPLl6QVVlQuHv0tupyqML2rOgbxXqN7ytdYze0ii9MahXs8U1VysPqyTsfFvaS6cJ4FNGbdnwOE4RKAmPqxHSp&X-Amz-Signature=da7d194263383e4615281cea6012c4fc6714be302bf20e07be99bab2f4f9f555&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
