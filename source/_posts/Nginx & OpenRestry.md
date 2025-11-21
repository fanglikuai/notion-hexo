---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664V4TSD6W%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJGMEQCIAc97A4moXfQVMfclgf6KGyzh%2BtVuz9aPninsolGGb62AiBkmhu9oyZiBncVHhzmtk0gnli9siFHiVXYWPTAeVhViyr%2FAwgEEAAaDDYzNzQyMzE4MzgwNSIM6GEP5ULsQcX%2B5rFHKtwDsaEdr0Lud84HIpCSU21Yy0SNlHhheW1jGFDmswaJ4dwxx%2Ft6wygkg6wpTNJ2ZtexdyXAvTWYNFG49A1Mdj3WxG7tKpkaAVL7doTXUKMpmWRURsREoYNSwIqJ5Gh%2B0pSgzCbT3%2Bm7IdK0zAY0QCcuxHI5FvWGTyXmbkjqTtSdj60t9JKunxKvrq%2FBJ5Q6IE9XBuEpwG6eLNOaRw2uOfl9U7uc5w46d2UZ%2FHlsJcEQHQmVQfw0Bvr0aoLoXV92Z8g3kYERYFZtSfaY%2BUPPn50v1sI3nMszdtSMQQL3hfMb4n5LhIFhD6KYQ7g9Vw1BZfiUKPofl95Ndt62y1R%2BGKwaYvQUs2oe2cZ%2FlVr5rOy32dvk20i8NgUfT3gMiCQpQIatXyJyzHBWS4iHPdLFi7E4IGrApRtgtafN92uY5ej2%2FKWplguacas9QKGhaNYHvL2JYFWnVQhp36V%2Fzm6PcFrdWP91v2EK2hOm4lRbpJR2A1hyNeCRq40QcT32xPplKUaneaf45yrX9YBrrEqO1h26BNEJ8dnTZbznyQ%2FRJt4nb4V5EErPNNPRuo6%2FOPVSNgFazsXyTOKbmi7n6xChQrlPhBd04o6ZJRfQNe3kfBqyDqSMGXBJS1Lze3DLIw4ww63%2FyAY6pgG36JDeINQbfSOZj4HDRXQudz9UZPdANDTykIO5Zx9T2ZmM%2Fbuic%2BH0riBgkNY%2Fj1Idoz5Y%2FG28XeOnkh6tk073fi2gFh78Q9sIxgl76MFY7wa2psovvZYeUW5qJhqNoyWuiZF6oxe9Yun76S5fc9YuhNshBditWEK8hhDT%2F5dvXR7YR9cWTa7fu3yj9PpG1zyOefuipoEBMLhmwaZX55kFQyldy0K9&X-Amz-Signature=91a1c915c6a07f3e0b461cc72ebd0eeec2c7f0d85f2113fe8696098ecbd1c518&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
