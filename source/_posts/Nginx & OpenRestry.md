---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFX2NTGL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T130039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7YXZXcJj1Wg0ybLIYNHMdl8Qu5BDPtFVXDJoU1E4W%2BQIgF%2FoVgRQ0JKDzgRht%2BOvN%2FXNjiTnnr5oN%2Fweb52aNvooq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDJG%2FDVOcxedT89CEMCrcAzCUYnYyhgngWxmiT3kqLWdOi5eerMix%2BOwjLZtisNXXul43ulrLCheVZ%2BobQsRkp5PNx5EwqUmUOdbCX9yooaoUwJzRqKtU44HHJ53Sb8Cu9ZGWIMhyWqeUIOdejndA1WAD6qy4vgk3VSwbNVVq720NVVtvigGNBPXT83HT4IaM3Sc2Fltrlmlx9x5cJq1woFGyNhIEfRss6kPltvTXFmeH97ucpQnFvCT4gteYPfC%2FBjqVdg0f%2BPHCiKFd31BBu%2BZhvecC8vo2%2BE7%2FFXGMRhQN3Z4dWNUFgbxlcm3O%2BAa1C%2B5SH10Cu5GGRjCDXFGPNrTXsUTHkd3wkPvbQ3I2v32kZSdvTeqX8TmXewhduE3vJUamrZP7g7lEJ5B3dvI69cjIIfI47FZT06TMNhescEnleWYn07Bih3%2FSEqSONJ8CeFknAddnFowSoWEnRMdkZtNpkZIZxR1LkG5Ylv3hA268WyQiDD86HSVGDkZk3xP2piEVuRuELog39GqIXeye5OUxQd1pZYKRs7KUC849ml8Empdmagql78MkFD3lxu8KcBUCO4flg1yDGIVOD%2F6DUkltuOvC6PE8X4SGPNLW2sxMAjFCqhtLxZDepAfckvgT5Ypn5C4tiATWcmvJMNXCosgGOqUBIb9iIlg0M%2FL45F%2BaSoJwCjb69j%2Fz6e8B88Na3nnweYMyaPlClxj97OuVWieGZaBmnTslhvbwABiTtoSrzI9I7%2F3X0aic74lzJDBg%2FRlDDACmLJZlTK3xqyhn1TGUk3VCaceSRU7YR84qvKmGGCoBUymWQS8N6puULJHg8lDtnT7VVylFbsRNkqVodL1yj%2BPRPxuC1UeGQDdi%2BFxxd5IIYy43Ymlr&X-Amz-Signature=65be31e0fa67e45988988f1ad016f0afe817f181c14184489a60255cd66db53d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
