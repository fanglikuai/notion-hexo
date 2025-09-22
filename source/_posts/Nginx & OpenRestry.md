---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKIGQ2GB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEKR%2BZbxdrFhOhFuhC3EQ5B%2FWUWnq1b0TKxWGV%2FeZc2WAiBHkuB0goF0tIFtESyhFzF1YLkcV%2BN3CO9t9zYGB8ZO7Cr%2FAwguEAAaDDYzNzQyMzE4MzgwNSIMKpYhLcS8Cn6aFNA8KtwDrD9Kv1rvgTH%2B2bShWGiCqt%2Ff4IRFlvTQMAH4CNBqEONhlmwKVin2cfVU7PNtjSAFLIbT0nxvQrSNWayfqT91TrgiEiwiDNhmo5e2%2BPFcApBXX7AJ1KTtbaHkiuiWdHNBJIWGzb%2B9Gk2ih1pNrnTZE%2BfIxBrE8CVHnuY2HSh54BtnipLcGcKdQk9GqY91oB%2B%2BDMDp%2FwtuUkCdvy%2Fkh1VJjtIfOZXv3%2F5e75nIo3PYKM6Amcv7bEK9S323spOatZZ%2BxVBsqaxUx20oROB6XUzoOmKdMyujDiE1a7lOr6d8WxsyJW5iNdadFonzvC%2FWPIDFL12PZOlbvK1kySTMHpX7OJjePRHLMNCfVRQ58tASEbfdw0E0%2FdJ4o0%2FT%2F4hOR6qpgv8K9DuWZ5kH013wXTJAuydyuh24T5oA%2Fr%2BKqy09zW%2BDnlZ1jsxNqVK7U5WoECaf8MAp8A2EAe2zezgLlYpNPDOFzVt0nvc0Qf1E5uhEGYyxOlIRMCUFcqoV4gbO01N7ecCHgtIyK0AfngYbKr590ONWWuAJaNG0qIAqyIqaaaBxULfaNEfmptuRF1MgVATX6e9rOvV15QsfX2Ka5gsb%2FlYxGjVyDvtI6lJEPilzEjSWGNqaEPd1%2BvDpmegwj43FxgY6pgE35fUxhqBX%2BzITySTORJR8IGd7SLKdAeHqnP68m4eZRq3ARqXA1BC698Qr9lqKYliF2paQRteuXVSJ231zgygn7JvUwmHxuWp25eRgKtQDy4aM8SHJUotphnCngVkwrasY6zXjGN8zHlhdFCs3SWnULdxmtU1F4hAgiPV7Yu%2BzJz4f45jlcd8%2B0PyQy1e6nxG7VpJpemKFnb69l9NhN18WleFFjprs&X-Amz-Signature=8a697cb63e878d209302511bc63afb09b0eaa6cde1f524eca08febc66d622bca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
