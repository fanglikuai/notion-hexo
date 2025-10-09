---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JDGUL3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T180159Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIB54f2Vd5oKIXXENN3SqTmnZxAeGzcMcfSTVCiuK%2FHYWAiEA7zXCaOoWnr4oYJN6eKx3qOkykz5N6JW7eAsabfj9ivwqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIGapU3aO1nSBqMF5CrcA1WAk0mVn6DkvEu%2B4RjvRq0AUR3hOJQZOnZ12xqr1ZFt2QLWRrHmktHPDZ1OqLcU%2BKTBOaMmWSN1yNLz3fTmGH2ztPiBr2LTxwoC7WtdPih3gHEcY%2FkuX3fhKn6ZK%2FC07%2FBsNWXws5fdflZW9l7WXZnf3L3Ecs7LpgVD6VLJ%2FB6oybm0vf20xaD%2FaAmsg5yU90RC5HhkhCD0DnS%2BfaPscYzC2KQ6jI0owIRzEpDc9GMQSZaxxKGe359cZq4h2yd%2Fq0WoSMdqpyO7K10B6CWtxicbw5WkAItll%2B1fs6lgFBuAdICadzt99miWtdEjBWBRmplvp1%2FwOexCF0%2F7J3OXE%2BZnzA%2FsqpEtF%2FPEXCVFm9m83EwEYTHCPJ1dH4VQ5qsUzEfuDLl%2FFlnw1CJkQbwLCkH1FC30pqkYM30lGMUHNNZOppg8kxCxsman6xAVsnbYqtSv1VH%2FP9RkLuu24U9Zf2266vLlC%2BO%2BbFoTRnWedaGLW4BFrCUUJe4x6w5hMnDEJwaS2vtqIWtrnczjEW%2FkNVhiafvJQnRcFNHy5cr4B1ygXJoZYDRe%2FLy4jnGYqDBoKfnTOjOd0KevDJvgWadWvWqNSux33K0qdhIbSoK2wE8%2Frp8fMg01qM6pEAxiMI3in8cGOqUB7h2kR8Sk4x8d4GiAKPN0hqv2uyJB0Kbb05ks2STuUjNIW1Z8BLcPoxFHgw735BVQLmmcWFL3wIibe%2Fh2ze8lEe%2FkBskFAx4LjwKum0hXtd0WMqBJoA%2BOKLOk6n95JRvULbFyJ1rrQDVOHYyxXxzEBqZVqeqrWcaYb4Bkm5wURJxujocoPA4epp%2B7RgEyPq9YhNlVnyB2ct31whZU4cIk46VCZPgQ&X-Amz-Signature=b8eef4631b03b770c6e74b6cd3d8070a7385c4c7ac55c86815b0b00d046406d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
