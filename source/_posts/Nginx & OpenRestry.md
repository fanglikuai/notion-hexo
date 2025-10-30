---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z52N2COV%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIAKAUjAmPzbtgwsylJsPKzSwG%2BBEd6Sv3Mjnk1wheW0BAiEAyXQOrnwWWVd282PNfR7%2FUi3CdmBuvYe4ejvUn%2BXkCTQqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAysy5gjNIKpZW%2BRcyrcA5F5199uJn%2Fre8jy7Frsdb32SVBaiHgm4G3ADayfe2AP0YdzZmSMtBwEbLPYo16jgVCTDgpM4qtsjEtu8RVkdtOmuIOh%2Fu0bGRuYPDHwc4lsb0Bl52%2F4OnE43UTeEHT39XcfiAwtcAjnnoXMJyj7wEw6H6LamvM1Ox57uX5s8QwZlfDyktYzEOKVq54Zxjlzc%2BSgVoExSanR25cTE%2BNcHFhiw4RRHWvw70Rmfto1VtZ5XquBSj85z6jzTyR2ZPF9Qh6QvXSSN7bBwDNgFZoUv%2BbLa4nM190%2BmgBgZTVD35xAR%2BThomH0wwHPt01mTwtOarVRJoG1iAU6wKNrhkfBQd03frjqKiU1O6lFpQ2ynERCdsnsuzZJLEExndM8sypIue74c42FlSZEWLFftmtisSc9r9mLt0rJV7Hm7stCWFrdRc0k68YYOKDEfgiITezSkJXy7Y6EJja2ix3FhJvu2hG%2FctHjl84XeijiHHrk7UhOM0U%2BuH7HOuvPry1MNnuh323MKcDmwaHQ%2FYsAsszXUX2UogjMt1J7%2FfChJpZ%2BZ80TJNkJG0UQRenlg1y4KkOPorzB95lKGLH2ObLK5ZC%2FZ8soJ3aCJiBrmieKMecKduoieVGew6Mf%2BL%2FvbgR0MInRi8gGOqUBIUuZTl7g7MZ9JWURWq4dYNbyGr6ff4VqxSM9KIS0tNN3SUtoHuwfwX4fXayXK70RGzwY3hbs%2BjtBlrRDf2vEv4YtmSWnDXxh1ie8wcDUwynA1YNygRNyM5e0GVK%2BIVcw61Nb%2FlEm6Prbx0D3sytRj4Y6KoVaJkNnBrZQvItm%2BALJhN1OPoI2q15Zd6%2FiicPIHnqa94LiiGtt93Noj9IbdX2qbxSa&X-Amz-Signature=83c589f89e11950a675aa8c65848363b64ff7a400bf3287a1722c7c763923559&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
