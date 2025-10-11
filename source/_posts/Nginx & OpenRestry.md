---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX65JKQO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDdxRbnQtkoaZcvfcHh9eAkvPoIpMT520z3eiURTa16wwIhANeacX2S%2BHfX5rA9XHy9X9jlojUj%2FurRIjV4MVFzB6g3Kv8DCBYQABoMNjM3NDIzMTgzODA1Igx%2B%2BJI9ZhrvRspRKCQq3AMUenTY5SjHgXwQywvxZqUZwNKo3iBTZZSAOPHBdlrQ4tzJoo8QgpFTSg5%2BdCXV9sp1VlAT0o1UhMlhwhzENWadzilpqKSunb6MYaPaZNWXVxzd%2BsoMKMu%2FtLc%2BPvfRhlNFMJHJgHHF5S%2BZ3OKb1SPiaLAyMSwhQepDdaVh94HrNiUy41pqp4hk%2BjBtfOSI%2BF0024cTMNiezWpJppItVWMpolJoabB2Ckn%2FYyS%2F6to2vDRuJR%2Fr9hEDRa%2FpyY3pfkwkycZWdDLGf5hH8Rs5GSyqXEJAqFZPE4SNzCReZ29LLsezMjD3myFM%2FdGXEvGG9D5JhmWbS5yOLOG47BzGtXuDd38J9u6N4WmtjgWbhOAg4EWF0LDKRvjOesaFYOwhZJXPiIyLZvi2VRB6jWzLCDMpnQlutKT1WSzzVuHs2MycufLyJSFpeH6msWpskm3Xw7g8zF0XbSDipCZgsLXD8vyjX0eX846HhRKxPx1Wi49dOGtP8p4mVHdCyHlP1TMWtt34IlSLJlnoz1eSIv9w9RoS843FyaLLQHhi0cpbKu5ZQadoHdhbiPIM5tqd0PIJ46nNX1YPDiSsvs%2BS0IU7mfBanwCikpeL0tZP0qZG5LVvduCtYGIJGsTXqBqkMTD%2BpKnHBjqkASch8kNwPaRPiTZ1tVxQD8ED120brzs5GkKAGkuapYXJZ700GJaR6YxsrMycqh950kygg428cFn8WP8DtRM6cQKrrl%2F6KeUY4HpFAeigbIrilwua3VsE5E6CK24rzZDVvPnXkZcAW6kmBv9iolZp55KtM3MCA%2FQlVsn1IjUtUKMTON02uAhSISxwulh%2BIxlmd8jcoDuiO3AHAYhQgcvRFjaHpg5T&X-Amz-Signature=4dd42c5ad50407be24117afcc366817cea8ed4561d13eccda1f336c557b994b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
