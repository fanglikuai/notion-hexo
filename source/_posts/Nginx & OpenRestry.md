---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3H5H23D%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNbFbekt%2FW5CVxbVEuAvpFu1fsApsVA%2Ft9umrkQ6LdpAIgN2JYD2HGSa0brMzK2VKefNFfV41s4nFIsSNnOa2yNl4q%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDE4remX5Mus9%2FZ09fSrcAyk%2BxIP50v7PtC3%2FyKjYBXE162ix%2FyDYAT7hWSuBBltYJCmHhNYdHwRyErUeLj9h2oSuZgvYaYbO6bc0LDpW0Dv07WrES%2BiJfWB0rUHthIo%2BRjZnqjYrtBDT5MBfnWb9GQ6iuWAgje1gMycUZC%2F36e7ebCxGJpnYUg%2Fm7fNQm3qPajcPVxRa%2Bj%2Bn4z9GEcTjBz2wpGB1Z0RxJiROtHmyEIcsg64R09Hvjf2RPuYZO4xy3xpA%2FlyQcMVKMyQXgoySmrellzRC88TSuD55HbXq21CT4kHHcIUeAJxF0TYpJdcQ6rnfQpaBj1uHI73wJN%2FsUQXR8LcM7y6b4H3c7PlsSzMGVnsY1vz1PESJHHY%2BkMhjmFNIIzg%2FKLE3h5OO%2B%2BXmVqZYynELB%2Bd8ZET6ZGRuxep8eMJIPtMZGdnISrUPiB9RqfogzADo%2FEJCnbxyzX7C9Kp7PoQwa2aCnyrwy0tPasKgiTlQ5RReYGRACC1Gu8hPDeR%2FfwE61ii1oW4T0v%2F3sIiNmH7x8n3MsdOoDih0P%2FTI0plgGyRx3yF0G6xj3ItxZIVt2d96DNcikLruNbFU5dc%2B%2BjVwK2OvyrAcS80CnIOyELGr%2F8hX6jcjqRpXAu2HKqMzFXjAj4UIIDVcMJnSz8YGOqUBLXxiHTJUQ2d1A4KUDFYLDVDw2qiL4LnIssSzNWLO3IMSo9K6XqS%2BhXlnv8I86IaigE8At0oHkeBGp2aOSwPPIB8uxMw%2FvlfIrZ3FAa0%2BUE3A4BSo5FRDVliCXuadfIdJFKT%2Bn43%2BIdzhsHLpW4bZiuHMXrQ%2FA846gw4X9grm1IPN2U1raM3sbCGxMWyacQRfiasm0JwAhIheRLiRbtOEyTjyXH51&X-Amz-Signature=3cdddee9b5fff8b456cb5cd2bb5e7897b27fce93b441d914f6795096ab592a46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
