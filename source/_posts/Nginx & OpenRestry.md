---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQDSIY7H%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQDa66qMcwSlMWxzoFb%2FZ0elNhlTsRtzZCDZMtfr8k0TgwIhAPwjwCNQsTqqI3g3pkcerbdyPUd03m0nfsokC6orIRFpKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDjIweP4yynyv22g4q3AODz%2Bel0We08otVcX9LTy%2FOwcoPY5E30TXjaT2uJKxxHC%2FdeeiAUoR%2FbwDAVqSJFouvdvlxpG7Q9XcVT7jf2pPoouDscbvyg99kDw8%2FilAssz%2F58QuCXmqalIFY8UMuGq%2BTSKMBtOfmSTYC4ZtvKZBIo181EfnvTqPxGseg6NmCeac9civdgUbHb0IDrEZRMskfqul0h7JODp5ftM4Wr8OaeqJGuvdmqG2axWBvDx0zmJm1MX0apuauvnIS8AiA14cemeEl2Wm%2FcfYSLb66A40Ze%2F1Yl3u78O6pVHOEUQxiktIhfBz7meaW4gA01p8qjgG6jgPmRg4GLPkFAg8cwsR6ej0KTgQvf5U5h0Ig%2FBluDgdmdRvStd6LlOBpz4M6%2BXSL2lPI2YdW7ZVEGpTKTRdbRqsZne1VJPCFqJ9fpK1uCHjHwBMBK8Ig%2B%2B4eieL3aaTMdQBgFl1iQlpNGxw9ofNT8p30xP8bf0EWjLL4XWArCoHWM0tNxNsWtu2L0SvL%2BkZIp2Hb9EgyJ%2B%2FMNxb7G%2F%2BA85KdGCG7ilmf2mHs2z0nZMPVyi6S%2FdmwHyGQhY8mykcFL7v3EV6EWWDWhGLdJl%2F4aFcTRRjxso7XF0I0IUwwhkeFyZi8JL3ZlqvNgzCiw%2FHGBjqkARBKrdZHNjg983SnaELnHpASpBS%2BL2RnHVUJeTX5ovSHwGr8Bkq%2FjLi3XJzg8pTGpdHRDTUFKM6SbtFCjWacmBvKNQC%2BqO1G9Mfi3iRiKa7c6MpSmTxBNfjbXfjqOKyXPGRJdINlwr8RuNEKqx9FibJFOHsdUEJSJYo6%2B5BSb1vy8rQi9X4bRb%2Fg17axm8GrjvOt0FNT%2BQ%2BLZoX%2FwukfAMN8tN%2Fi&X-Amz-Signature=83575ae26019abe07accebc9ac2952171dd48e6dbe2e9e5d377fed2cc371b130&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
