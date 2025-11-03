---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBXJ64YK%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T160058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF2V0GURuJV7XZ6jTADIN9QrYSvqJrD5RZvG2Et%2FYJmbAiEAzbJtO9kJYYrSvy9%2F4mkQoE4MBOUzfgbqkpvi2EJMAIsq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRAfK3zLEX2GUcYvircA9r%2FP3ht4g%2F1P1x619NLZprYucy1a9b0Uz9eYYGZgSeaW1aBdh1BtJKBGfOYyOJVnJ0GettK9cOFmP%2BDN%2Bes70wg3mAdDlKEJhaQbKYVBR%2Be4Qe6G6l%2FZPnhhmH%2BJ0scdm%2BIoPz5C1G2uPbp46LztrS4j0wPaZh2sI%2FU%2BOffNX57VMN25ULvvedMKafqnicBW4XT4H2v2PHiBjN79HK0cu7%2Bb3TYtE3ogtxnAA0YqNhyeE7c1%2F8yKDDS9zXZk%2BXrWcInhMVc1TIYOciGAG3pqN7WqG2ztpuekVm7%2BtfIRjaztZYycRyCQqziKYTGefq3jlgyNGlWGdRSbaeoJSLdAgydLE%2B5QtGaY7UBSoiL9mrl519%2FOCYwrdMREXq5y7fCwkH0o5q8e6wSFIKaPJQmrQ7SHgwTlFENVqh%2BtdoEimh70vj4Gs5edUaVwjkLEDk2nfOHL5z1cSLR1YScNumlb7qbNpmt2xe53y8BC5iE5Y8uAzf2Dxk6Srq6TZiE5DlQSrruzrwKD00ySYtZ7HlTG9aID2EXZDzSJ%2BFC0ivxo0CkiU9KaEbM2FOo%2F3by%2BTecRBHmvxC3kJiyZnn8fdk5W7d0ouYcjP3b5HBWIhZtDzKdsNlAlFpb0pvm1Ju4MJ6Ro8gGOqUBKVEMPDpiRBTvgOz8MTae%2B8xD2h4QB2vY0Y99OBiaIHHCpP4FTWc88m9vuwE6lM%2FYmbYRa1%2B5vYJYPJ%2FlW%2B92asIZ1egik6%2FWkEUxFIt%2B%2F4Qp9%2FmGR0yz13LJeFaWVxh5J6RLggFZZTu30L%2Bu8ARy2uL9MWeO0XQ6SdosQa0ufKR6eLVCoGjZtiatHKUHukSoW5gXBGYT4UDfRzaPntesDB7GwuhV&X-Amz-Signature=bac992078b43a80e6f57cd4ac115cfd152e293810e15fd6098fdd7ba6e026676&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
