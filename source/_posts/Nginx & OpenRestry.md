---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3BQJDTR%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T130145Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCwMkuIVNE%2BJnr%2B8Es6iA1LAR2OL8o4gNsL0KY8thLbogIgDswDcADc9aCRroRl%2F2j1HLAmSEJZX7QD1AV0d5sSG9IqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPoB%2FPIpsbIa2v0f4yrcA05F4jqholXW1XkJ6OT5iNSwIjYGo4GiVGhoD8cc%2F0k%2BY6Nf2yQVSzMXyF94VsIUxa6Lym1%2BwA2Vr5wYEb7lD6zOAjS3Lp291CsxPcQK4Ytwe3wP99k4XENxe4qO189F%2BkSxvmrXNMDgzUXSadKDa%2FSJMW2y00mwbF%2Bb9Ek3es%2BjDMP9SrqLUdplOcwsNp0Tdcszo1Rqd10fTEvg%2BmlgzMH7%2Bm5lQyoiXeOKfHLShlnUdM1n4LlCAd7CDCxhKN4os0SGHH6JW8G7uYLvNpZ3MEhIlGAq4%2F2RRANNX9X3ePVVkae1mPYNtLPGJz75Dgv0y3T3HduHjZ6Kgns4Or31UOnD5yuCfpS%2BmIy9HN3cdaj3CY8dBdKcWRm8WnLrue99g8r4dfZJvq8MTrw4RU%2BRRRezw4np0h6IfOySjdpmBYjTc4vAnSa4jaKhv1RzyztOYNoOMXpq5sCW3YFdPdP8MKUUqcCT2JsRq12XAS%2BAvGYym5OENdW9jSR0KuQyulgcxrlyCZVgIBFljKFNlAVcv4POtOCM3o3EsWt1%2B7kr9bdHAgH%2BGIlpKFoc90nu9hInd1%2FtZ0Ve%2BdD31Gxt5eL7YEobXofFnfOOk5aWceXLe%2BN6ds6k4GJRpggdYZv0MLjar8YGOqUBIjuh6gU5b%2BhFXEysNT580G3d6WwcClKeqEpZBjU%2FSvay%2Bwwq3%2B4oRZ9pjnJGISWjiDtYGOwgALhmdbcFq0N9Nt8G6wOA3C3HdSxLK6aW3uW06DFroNVxzfc5AkSPmltWe%2BXxNJCONZg81S93seN3IuiEuu4Fq7Pf%2Bf%2BdUqV1nQaCaCbX%2BQM06ahVXf1ZaQJqXud1HuLNoxpuSCDQ6yRkQ9%2F%2BOUBv&X-Amz-Signature=1ebf5b8a6993490bb5b4b9173e8aff1e673f52508b8c2302ae5e9431403dbb62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
