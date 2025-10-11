---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCMKTKO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIGsBUnJjE6kMYAgltCfDkf6vf9GRemXVM8ElCDi1m8nAAiEA2JT99P4GN6vElkz1oGslhf%2F3ve3vekI8w7Hbs0Fs%2Fhsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDGkxngAW0zQUbrmdsyrcA6XD1RVPey%2FR8DXKtZIm705%2FBuat3TGo%2FcG2punQbQGRmy28nfSNLOfCCVU2BAP%2F5cGNEj5MAEr1VAfvmV86AvsA0kXw1ggp2VHH3I0A%2Bv3Fu3biqEk44gg4aGcz7d3UXXeUda2t29qrUADLagTqIn6FkX8BiB0NYJlsJzXPIbnr%2FILCy387FwGk4gNu%2BzNLO2i%2Bzvb0xY5WYWHsgXI6fooHjtIMic97D7TyVnrsn0Avk5A8uG%2BFsvN0HV0MMUqMUmGsaV9yrbm1BPAKBuBmfFzz2pxFlmrbRPs1Q70e5LIcAp1iizqiCcVp%2BPh3TG9apphoZQS45VJOAKBXng6HaiXMDb7UiHtsRCB6yRhFZQ5r4zKNa0F6%2FChcZErX%2FpjsNDDSxHE0xFf43z%2FDrr8K%2FB%2FRtVDmUym2gVgyomAQv0acvL3mfQu5KOYgdgUXs2G2QHZ1snDUM9DvYrZkWiJ%2BU8bjX5Kdn5qb%2BLvQ41WZdnHT2MElxALXpPTkkrMtp6dWq3E2aMsJ2HEzKoMT5Bz9XC7VvXKGbGibF8%2FpKuYXPOzP5Pemp1x4ypnZaCa4nJbY0JY1nbfVzJ17%2FZP2w6Jpu0fd3hKYQ8EdXC9cISpWBikt%2BUPycZBbozbaHdXmMJ%2BlqccGOqUBiNW%2FtuqVdzISyjgXzG%2BYMWsvi%2Bn8t9EYn28Gl29vM41y9w9gOMUpBZO4c0HUhhP4ZHrMLGF0uO%2FqVVU8oq8Q654KzLDbA%2BTm59ng7d%2FWZS4wdjEVbQ3gHnJwZCTIvUIvchV0yvQ4x21DW74tsSBryO6i3fcFgXewZX5eGDInFMCtjTQdBaxkiKTndRaUfUslSZLVs30hU%2Fo1Fqeh7shEJYpaxmrH&X-Amz-Signature=e8c6f969e8ad56870a27f14b5813ff6579c3577c0bfba2a7fba4669e5b277c08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
