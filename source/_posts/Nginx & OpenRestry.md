---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJ46L3K3%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF9WpGMsrtNZUlJ%2FCZM0NUZwhVUcp3QYGMm3S%2By58bQtAiBFCXMP7ZSc5omBzYMh5PMG5xDTFmWy0djoH4epzK2TzyqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxIrAbK38sbJAJl1kKtwD%2BQXfcre4uts4UMhZW6icDp9WimO%2B6ju9xcSpdO%2Bin1Luflmx81omyWy8GYBf%2Brb4mMGakFQULpDQVgFEIYvDdQze1rK2Crs6T3pNudp5rKo72IFBuWR6aer0dghSqtzS5AtX9SQjVyjv922xeRtwVw4B7edMjm6lGgTV86blJHPQuVg9TyEAF7cZL69SvvT7kmteDk9L5k3oDS3mRPmP%2Bw4vWZjFm1ZJyZ7BE5n4HAXJ7dH%2BSFNPNn3yHwhuH%2BVLb1%2FwRgdzkPprv2jl%2B5TLqsT9T2FKFWdN%2FYMiZNuUudV8ohi3npRHDVI7ypu2rC7GYW9IbBnCanCLbdRAMYijlDm%2F8JerK446VNnytp9HsB2bdTsguswWBr%2BIH0Uagj7R3nujS74doDEKKd2Yz0WG4GKiLEP%2FAalBC1MuprH4Fv%2BmRiZABi%2BE9N2YXyQZE825EvnFsK2WqysuOVAVlUbzMtOiqDf%2BHXBa5FJ9SBsUNn9AegbNfEHwhgqm5NsVwIU13O9cLTMmgIDHt14Zl3VN8x9dFKKGp5s0W6ghj07H5lDud3mX4fJbxQlxca4jYvpCv9%2FniNF1qs2Mml5OEsw2rnflxyqn5Z4XcFMdu1lWRcF9De1yf4b87GRlgg0w%2Fdu9xgY6pgEcO50AYN6Xq8XCMPAUZ1ZM%2B3f8DVBrhfyEJzZ8v7KuQTXt2WTH5jYawbEigJDoLe6Os58KSkTmgK6gybKcABVKa%2B8wraRJDEFqDxqpgKWVMgJRMm%2Bg9fsoHwIzjCRo5sp9m6E4rpmuLk33m8tzCtg8EyLc1gpZhoKAPL2zGVdcty1C8B284OMDmsLaA%2B27FuWcKXspXcllQhbzwLxoJwFbxQAdbzbL&X-Amz-Signature=90aa40bf10ecd63040ae2aeb3de327011e98be4fd1f87dd73c6bf812df6f8826&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
