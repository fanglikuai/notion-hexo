---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673RAWY5J%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQDWLThJG2R89%2FnxDSGKZQ2EAcHKKp2gvR6AjSbZudJSXwIgPWUrcMrUZR%2B9nFvEbvKg8dCES%2FZ600ZvI%2BXTft2B3loqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNG5Cg2f435P1IoX9CrcAzN4zsLH1A39OM9WK6ou%2BxLOn%2BZ16fHtSBLSQk4MpxWkhveXQdXY7PSNfWxNOFmBjVXFFXOmEEXZ%2Bw83sydkReTYRKLZvS8ThUqXhTfso6b%2FFgNeNxXexaV5nWW7fyj2y21LcjJzVb75sArURSEpu7AirbTPuggu3rCdP%2Fl6IfEOGOVJR4XVIcPF%2By1DkywrSpAqibCEVfUnzKN3PBTwAiD4xXTAwTFclHZQXxa3ouy9%2F9rM2jF9cFSgrccq09xKo2t7OzKHcxktTxU4pqDnnHkLyNFgLldjJyZPoemZXpTvuqtZK6ac%2F1al4VibVgLylLWD8bmAy%2Bc3Ouiby0qzGWPzsROJG0TCfM9V1rPRal56S4luIHiWgZkOseVrYBzHgD5chGHvnz1g6twYfrWVYYGeBwDotLRPTr2TX7aQKb0S2PtLf1hVm65sTdjOzuVhVJWKXcLCbMLQ9o8A3pIsrfBCaG5XA7tZ6W7oH59o8mzkTCyPsdO3ydfkwfoseuGFhlZ1nzphFWkO2mdE3M66m%2FNiF6VuqwwNpQSvbWCCRsJMqML7V0%2B%2Bl6SOeD2n7J4Je0wjDtFS%2BFIHs4GHjhzxJqHMVVPgC2q%2BDHwrWm%2BHO4Cxvgq0enPjNmtKt37gMP%2BCzscGOqUBJxoy%2BzHjPT%2B7pxOJUHS4KITKaZ6pEKy4eCWfT4x8wwYduFmr%2BvXY6FPTvdUnXE2DhtoACaL%2BNPw0Shv%2BXm9VJffp99G8%2FFCclsOcrAZNBXJxC7K4JKw3uPNxaaxvsf842Df7JtExidR%2BkiGF4glPk7lP9p3FsM%2FjM%2Bx%2BwC%2BFA%2B8OFlfyw7KnXw1eEn%2BJ9mTw7QmsfSClDNKFIMlsS%2FFnKWvvYdsm&X-Amz-Signature=7a4a8cdcfae44331d9b0f9182855a84f915ef9a9b076bfa6aa623eacbaf2ce8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
