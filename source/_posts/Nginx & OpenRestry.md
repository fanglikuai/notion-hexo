---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643GSYVCE%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHqkRjLkF%2BRmj%2FnRYiH%2FbpmLxXiOjg9f5JnkCT9VoJhvAiB5uUmf54vi5XXQlZcOK7JdiyRBiZXVnAJ7AUGE%2BHvplCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsi%2FnELsbsuD4MxTVKtwDN%2F%2FzW1LTXJoC%2FqujJcrX5uLss2%2FEOGFCRRuVKSo9S8buhtD%2FEWUnIEDcgW7IUpWendSy1HFzCkDD8MtYTsPz%2B6s3iMDddrjPaa6pYDDsYoKp4BgZ6l%2F%2FvbVBHIZxJS3R6aQBc%2Fh7cfdEL9%2FnVmRY2u%2FkX1vsIZE6w379hNlhV5jyoyJ9LS4IP9SIChWnUoBwhDsxcez2eIq2p2sW0gmQ2K%2BjuuaWaHmEDgRVc205mGunmvda5E7Ha%2FQc4LyzXThdVAE1MqbHurTqdVwUjf00IVfMTHA1li9PE9%2FOzkpcrKEaHZbmi5QGErRXVrcNz0OCZjnoW%2BPWHWtbXjKpNBRLo2Ea5rWXtyyEM%2BnulGF9t72LUwwCqb1YjkKx%2FAy4Ac0O7lQcfmMVCPdmrCaENHOhFsA8MFFYlQGvrIdoKc%2BFvACcnGy5BKC41GAefnVVoeCUPYOH6fzkATbIGzKrT9sSdMAiZk3msll8YGsHb3woMJ9Z4d489VRbAuQrrI%2FafdQnmW0DCUPHD8AyCjHJFTmFBAPnTrzoD9c90okgUOVikBNIL4hvnWdo01CU9N8PqHHfgZLOUz6T%2F9aqMl9ziPzKN%2Ff%2FyG6NZUPRBC4j5ZQFnAOgIp14guy4B2pdKVYwl93vyAY6pgFtofXoNCXqSNp%2BZCejm8tjUXYWmAQE1GQ5vSLcFTanf999beZEy0uTKwzftjLl7lS1L%2F1nR8LCMmOnQXVebGZMR1OYL8TNyTY6k4ppWdeIpiLsuwgGKAjrs07UQ%2Fh03gUFZ2EuyvJwkFMvFmDLS71z%2FXJKtx2%2FFDhjQ%2FnM8VwrU5dtJt1eEsXDagVixMhq%2Fiz0AKbIfu8BNXbtfl9%2BQtcMo9ES7UfN&X-Amz-Signature=0a1bc9f666dedddca1db854568d8d1166d5bef4af1d6e28d96a600d99dc6f15b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
