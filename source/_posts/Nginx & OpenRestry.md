---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN5RANMY%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCGEq%2Bjcudng5uETDbAuoFMukIzvIvidz%2BciSrk3HsmjAIgYUqI3eF%2Bij31vJ5jKepRtd%2F6E4J%2BfieIxISFtdGoXcEq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNDD8kBxg%2FCqiGkHHSrcA9JRxbAqXwpYNOBmOZhcIFrNZsg5upPQZ6gKKYwcfRSJl3FvqXXw2aYkSPihjN1jj2UAl2ik1xg8GWbpSlE357LQykiM6qqRIzh3obZder%2BKu1Xm2a8OLvTeKPP9xwhu%2BmCQhzacZRT0g1%2F%2BM76HKu0%2BxIHSy7p8O0LH%2BSnaGXyL8wim06bWkcvtk7CBesXzmlvUZmd%2Bis223FakbjwjSL4uX9oWnFxOfhrKj8iPTzx%2BVP7Rm%2FGqXDfnQMY5d%2Fp5qaaQa4fV3FSyTWgLy4Ob7%2FO9n6%2BYcHmfQuuQGd7sJgyNGMr8ItyR5K2uTI3mBiK5w1puxDp1cdTO3wqNRrWYMsPgbV1ez7%2FznHkJXK%2BcPwLaNA1J%2B139iSSU0fnVRWVS5DYootXfx1d3g87uE435Qpx6EjfUDvqh%2FxjNXUHHguDOFPJBCb0COT%2BRadXPRVl4zhdkpUA5gVByXuoOGHei6N1lQVlMOJ1mEITF4n2mC98VBpUP4YLI1ohviVeUEtYtIYYeKSnQbbhZ%2BU4StAqs4mXR6wRmMnfR6cl8alX3PiZ2Fp0B4I%2FoXbZvgpW9thc15cMHkHqac4dTxEC17MKlweeSd59umi6MV7s65xnDYWeQRKkkq5260a0vblhPMNTTm8gGOqUB8EDHoVqd%2Fmhz%2FY%2FZc7K7Fh2Sk%2FMPBF1b5nWNL2gNbBAiKqG4amXm8qqxjEdzUMl3k11IV8uKrPXp2ydrMP7l%2FtWSRL%2BQZtmkBAsh4W6hHOkJMxsatw3HSNguWZdsREKEfwESlwbMzFkkuVcso5z2BH%2BwlcqEC4S4Dl7LopeW99W7IM6Iv%2FQS4aERPZQjNw4iZmeNiHbpHvfzu5m3MsddMPG%2BrZNc&X-Amz-Signature=d1ded0ebc9134e052d3f42e3140a9e5fd8de5734a64d6062ec04b0226de8d297&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
