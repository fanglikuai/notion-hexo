---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7CKZCJU%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3HB3JJgpWQCAwShfAFJ885540iUovHJljdCmlkq%2FgBQIgHBozo7mqKayytT0J7Ab63UBnFIBOiC03For8qMv0WigqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKdQ70ncmN1ZUet24ircA9o4b%2BL09C2FnGpVOVuMm8x5QxQfJjMz5I%2Bk4uUKed8XIGR%2Fsj%2FTO6jg%2BKn2Rtqnr06%2FFhYLcqBEXD4tFYsuJTJMIVeiXDaBEFsJyWoLXSe2alSVCpMoiMlGT%2FC%2FBYc7%2FQ4R33OutA%2Fzl53CemPkVSVrzJnVK%2FYZYmBKsNG85C1d4Pdpg%2F89VFoi60IZw81MbBer8ROqXXELRhNR4IP93%2F1HlC08m3HEY88V28keajcvxKAIMqYWb8ckET5TT2ukoxHKHZoVpDbDPd7VFhNxS6TcykAmcqFknFPV6PE%2B7JoPHV%2FMGtqFGeXHqP6LpoI7qXSVj0i%2FUqlr7yVY8sLsHYeYCxlh9PjwU3o%2FeU3pU1%2FuESJYWOVE04wwRtdccjhnvTimnDAxO5a%2FsQKAutbsBLlA%2FKHKdRVQmKLoe9GMcWs9DJq3dnbAKqOM%2BxMfN6YNT4iX16YyEplerQLtVe68GcOvSaF8Ii%2FsSdQtBn4FHWOJP%2FQSidtaBe3wg9ykotTd5Bq01%2BbjITyD6ZmySjXAPSUD5s8RBN6gkKkjm8o%2FEeSDVYfuKLk8uwb%2BVn2NINDivofMRrsbJ08ma3srYcBOp679oc%2FpHw9O1hgyXkPChgjNzPME02S2IimCFEzQMIT0%2FscGOqUB65J4x50qkv8K5RnycMQ1Pqv6X8oak5T5c1hbyl7o13xrcQGdihc1O8dBnzNjYwwYWZ7yPQmgalFq7ucawsfvONBmDpurKn4XS%2FW9vZ3%2BwqshHpMVn0wVkmzSXULQJ5kUCFbhriACet88drqYfNriA5uSWX2txynDp7SDEC05OyUWaztYlm0cLomlM146vEnwhHKOipXDAMYfkYvvpHdNnswoJy3a&X-Amz-Signature=91124aaaa8821e3be3b613ee7fb0379ccb1bca1ee616ec44680b302b255b7de7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
