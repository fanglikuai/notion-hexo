---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHG45VRB%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFjerAtq2f%2Bw95MzS4BHqIzXvVB8tVvGWTNZAzG8t7FHAiEAoT9PdTI2CEhPGb0GuyNwEyj1ZearXsgAgKBTrn5RtQsq%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDG09s2RmhCd22egJFSrcAx11d8f2fRo9l4IT4%2B6umBpHrzxiTMWIrq65cXaFnjHgo2uaN7krKq5Se2VT7UGoUpdfYsm6DBkO%2FxaMVBVaVkmNmHRoL94dKiiPzlxX8ijiWkq1fupex2l4dM2yNB27mG7XVCCYsSs02nSoFErZNekH1O1Ei74y8bdd3bvIUvMWPEgZqZ0VmvOfjhc9nWhRMEI5NmqJG8kbeXa6QiowjjCpRxBvOTKC7MVSg16EnADmyezJ4tPfe%2FkO1rsqYIUnwWL88kGJVgjpyIkHumje0z%2Fv83aNZvTDGjELhS7efzPUUKsaJ5%2B4d2f3F%2F9BAwNEX9Yqkvn6ehiAaqCo9RkKZ8kijD1A%2BnZKtrgLRd26COdbgUiVIoZL5gOxzF2BpjO4jGnu1WZxxLURJNFpRxPMPZdifr6rMQTvQEndModOy%2BK%2FU0PasMuQxYjF1%2B56R3duvRNp0uOM8v3vZM9yYMVoJFlSrnjEmfG4Ewlfz4EXFyCacN0GRspA1F0QaarEG3lC14H12Z9XTdly7oaeE%2F%2FnKmiTjtGO6XYirZLOzp1zicPBzufwD13HymDQ3IDS%2FBnw%2BvGgKF%2FSwk3wfXOXDkdF%2BzNZRB5NgZaOLa6dTu7qb4fu0FuF4aVSY%2BA9ZBMqMP3aycYGOqUBpTQZZpkkuISABXhQcaYzBXfcBtoRjYk4j0I0wW4YFAYlrKn8y9Ay2%2FGIDuv5Wbe8VET1%2BKPRatVXyweev%2FpNl3HnkJsuTFK9t186SSWVH3GxvG%2ByKblyD2LP5tWszb5SpHDBItXmY%2Bh%2BolnKoaDUA4V245KCyPvCACOTShgz08mTxQ1wf2sXtLUt3Io%2FFOiLBghCEJkI61QkQ8ebblKskzdNH1lP&X-Amz-Signature=a1eb04763b2828a604e8439ed13484575f4c814081870882e890918cbe5ab3e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
