---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZG6SUGT%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEkU5FXKLRKGspDOeCft%2BpVlhfdcj2%2FFq7XJ9b0AK9r2AiBA6dJPo9%2Bk8OwiT3fJQRtSI7DILCFu0Y2bSShrdEAHKyqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6UW3KakfPTX5fpwRKtwD6AlCe5k%2FW4efehCgNYoFuOd7ljIt1lRszFmpDmub1KOxlCQxmkgt%2BonJV1p4zY0TZidG4oTIzhdO6kozwGNh95xlc1jCxDw%2FYbi5k9ov3zXDB2iyTOAiE2reIEL3IkzlMAq6QBMv380u3WPjqZtl%2BDELByhBnPuKmiTfSCNduFi23uQ4K%2BGePF3eh0sQkusLGGeA%2B3HeFbEKVdsR8d7CiZzhRvR3etrK965ZZqvl1Z5qddyCRn%2FMCCT4lV1cHq3Z7tcmSd16JT0IsgNTkPRkdIq7yI9S8amqecl1Au2P%2BCq3ZreCzjzRx7wm4OWpomtpRLchvyHQRLcb3j7EI0D54%2B424Si6OFeqGv4%2Fag6iQtsuGGdUuNg%2FdIQCwvuVA06RvrKemv%2BY4Es3fHLYrR%2B9N5iJHFKl310L5PKXNcWD%2FZm0SB53Zmv4UuY0XVlj%2FG058ZtItDmJ%2Fs0U%2BnIH8OKwPc0sTQnh97jcok2syI8fx8bic746Mc0j%2BYidjEJZ7GL7s9356QLhFg9IXfi5U%2FqGAgtINCKx8DsjbWTvXAjM%2B%2FGoTfebPPHjVm2kgwHfl%2Fclbd8PZnreC6%2BAMTDpxawm3CyvpqnCHpPaVT4oWBAGW34TZky5%2B3K9Ui996gUw3eC0yAY6pgGz86G%2BCEtLHhtWqdJoFllbtKRejZ4pygHTr%2FtehVpuikEKalTgJGCLOuLew4CCaPE0vyhByYNTvDlPEud%2BGDXJTkTJfjT3345uZq3pCSvG3Ea7GHi8orP75CGxIAwjwMyjOCQMrGt9SJbKqZ4ZkUAxnRXEkwTW1Z0VJ29gFWbQbeOXGVNbQIxia%2B6F9RtEc7Na5P5%2FDLSBtnekpoFJGUVNMcPwnIoU&X-Amz-Signature=70825bde993c54a624bcb820b20f139a9ad218c78561ff2de3ddcaa25b689523&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
