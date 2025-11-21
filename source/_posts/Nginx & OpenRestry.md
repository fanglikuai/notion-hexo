---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2GHS3UM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQCyUu4juJ4lx4o3Il4SaoKatuAODWsvxKmNlGWbnMt6rwIgNg2z9Uy1WVs4YdXhyb9Ka9bYeJVpvJQtNuyV46YAKW4q%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDJjWS3vQF8poi3OCHCrcA1n%2BGsQiTBD3rfu1QXLysv%2BxicWb02f6T71sGjYFOz9WY7O2lhacsMHUzR323j%2BBVLCHeixyphaGQoQVUzjw97QGAd0rndrHBwP5mfapkBAmdO%2BlWR%2FOpUm4oI0ZjsnbElkplxNPwfEawkwLn%2B7Cl6%2F1t%2B4G3WsLJLbYZN8YtLNKhdkPVZTF76fKK%2FEjQVK8DNK2cm%2FDuj4Aq1RJv%2BcUuUVtyV1kQMz2DeGIRTIojbkOi1FGLgVrBethTH4PrRDbf8pArnwc0UoothFbZRKuLMUKKlztRhR0ATtV4Eh7oIAwIC3itiSnYlj3mHLCA5sW1Dtxdf%2BZ5750LdkNhPPprxxSkNGSncVysZo51YP12%2FM60eud4xcgMHT%2Fzs2V6e%2FOmoJKRi%2FEKDnvAGQjRwN5jFx%2Bp%2BAwFmY52KTkgJmh8v3%2Begq8HVJu4qw1ljhtEvdKAGgCmsqeTKXrJn1ljKLIl2FBs%2F%2FrBX%2BFkZyPId%2BQVLzH2CLwxUP%2FrcI0Nhlhm4IHti0IGqSWUEoMXpqfsp9DxqQjWbX0peKVY2JwlPlOgCIKk6nL9cpQHddOXe6LxprGSbLONbUdfx4m7ZiHmnbl8jDCm1yWhhxw%2FvJWC6ATbZbHqQYuyNPZthKvIFaUML3sgckGOqUB1ugbwhMc9DsS9VKdtUFoxUjedu%2Bh8UQIDW0eUT2a18r9F5xTV89ClIK91L9awLmMQAvkmQpaUfLgwaXls9HSZXEiSWes5jMqgu%2FNPdoRBEEvdOm1Vo%2Bj2P%2B81aSTO89VDrPScf9p2DjPWA4ztDVA305s%2Fod%2Fc6w0VvWw%2FDY9r33YggGv6LI6bbgXsK9GrEwyVgcr65L28HWPQxBYwEwdbi24rzKC&X-Amz-Signature=fc6554489709264cdc6ba15c226fc180c9ec3d92a0c74338d6202a4aeb3bfaaa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
