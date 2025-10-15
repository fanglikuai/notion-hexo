---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMCF6LGI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2WM5pDNlm3BFIcGA0INBjy6IiIdGsi%2BUHqBnhfho6mAIhAMn6IyTpWdxU%2B%2Fis4eOUmxcrAm%2BSOL2DGpSQcaIT9NQTKv8DCGsQABoMNjM3NDIzMTgzODA1Igw%2F8Bx02FOQfXrlQGMq3ANiKlChBrckFM5Ognc9NqQB5UFRozEAkj5BQDz8yafJr5CAtN3t49GKZ70ffgIdjxNzaVyzBvA%2FtGY4uVAvtiFZCgBCy190sa7v2i8WWxf6Zrlruj82WROMDIH7zYnOd67gcA%2FBwvEIMhBv5Wh2mxu337eo7DVI9GI%2BO5XffeuUDTAkphFCiAln6wdRA%2Bn1F9GL9acmJDvXLdm1kPEP%2B7yuZcK2PQKhdA7fLqx5f7YkzL%2FbpEUv5aSzVSF6w199VWVu6jTP1ewdz85kNFdf06oMzoFe2OMhPlERI6vnMxxVApEkKI4j2fqMBq1ggzAMTegqVFCwYmehQUoWLU%2FAYGv2CEXYIo76lvuVr3phDGpcT4Y9%2B7O08IJh6XSSBK9WyfZpWXZ1t9iAyhcYBfrnLY13AoAiIjv9wP52LlTIz%2BiYcpS1cQaJU6pd0PchJOEqMUvHeu21raeCg0rfvffSvO1dg5hbFqxz1Qx2zdEz1GOmr9ICdIUjTx6QceZWq2n%2FHF5X6Aisi0yqMvy88Ns%2Fs6B94lTMB1MYWX0aoJOL2yKximINYr3hTzhG5uZlIQjVAB8RZVYmWw5cQMHUAg5%2Bw5%2FbZExsudFIRvEdfaeyUUyCPluGYZYUXZeg40qmLDCHibzHBjqkAePG10imlOd%2FgthToLhgmVcScBp6rIMVFVVq4ohZnlQb5MFTvkUYOr96tNJ6of96m30%2Bi8aDZ1LCB19yirsHfpZ0t%2FzQYFh9p8kFZ89XOoWpzacg5fyjG8EBnrhCxiifZgVcuxhjIFWFSZGUx%2FDqwFo8BelvBsrF56j0XcO2iTJEQQ66TPmilJ9MWmlLJ737e%2FHyac2xtK5vO4uIjHqLCRvWB5rn&X-Amz-Signature=7bbe5bd38b4f2dc34b4550d1cf5ca6886367cddef3459b7abfdbe132a75c3ed3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
