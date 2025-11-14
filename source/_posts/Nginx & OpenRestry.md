---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QA4YTOS2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5n94yeCnNF6U3Bl3mMIWq9lAA3TnZH4CxCaBIFOTSQAIhAIetz4slqsS5KCi5qTmC5kwE%2BiEjLmAQFC6F9T%2B8%2B2TNKv8DCF0QABoMNjM3NDIzMTgzODA1IgytTtnZ10STAkiH9bcq3AOFVJfP4dLgJS%2FWXsLIMfMb3ucPqFZKLEfqLu%2B4rSvUuSvUNHqrRAqBQCNcVWF89kpc8TE0tObOF6BXNtr9Z25eSuO2MJINm6C0SNTfUgeBuE5P%2B%2FGrHe0yDNF9Zz20mVSS888Jc%2FdfPbUDaPScpPhz2fBrY1BKOaJe7mc71%2Bk2LxmG0IDlxS%2BaWwGu0apxEG2xxSdb53C3CA1ofkZbk%2FyrK1FciL4hT8bqnTnxPWI90A5sYM5CccdruffJOAOr2YpTrkJwGbt1CaBj0FbOCk%2FpbplZ4hyiU%2Bmsn6iWHRDUW1FMoOuPL%2FifsuZ3XMzbK9eQSj%2BQYoQx2IKoG8p8pDCk3C0T6MgMPgQqDuia2GgTRZHB1IQG%2BaG95GOyDhw5fMqQfZA450wxF4TnhY8HvORmNKNBzp6pFCuY1CaFG1sXLcl7pwcddD2SWutztHcxm6wd3my%2FVH0t9Bdr%2FBsiM8GpNxRYWF278nF2lTHg9Bc8kyjyvHNgHH5xfEueQMXv%2BB9v%2F2%2BL6VLxlHxRZU0id5xQihlJFnXgB3z3djVerjT961SWgIu4wKxMiYk1UzYS%2Fra5Cvm1hp86POYYobJniy9XzLgduvRvwLFoxdWqTccIR8SP%2FXNr%2FLIyd0bp%2BzDe29rIBjqkAVcf%2FnnbZj21RV137LJm79p6GOT%2BBL8xma6465j8KGHVzj66PKt6MafH3%2B%2BWWFro8PfBl7aNCIjBteGBwghcEaXRJVyR%2FGuZnRWXwQRtPEo3M53JXPOT1IZra4Ol6nn%2BCzV0l2t90A7wsMffh7HsQgD0qS3Pdw1X1hU3qxrOuEBaPM6xVhNI93%2BIWFoTHXpNWfdYzdxJki2k6Nwt2p%2BPQizyH8IG&X-Amz-Signature=a9ba000243397eaa4fab4448107d50069ee9047be5e0b9a57af6faa8aa3b77e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
