---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGM43ZL5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T110057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIGjElHzX%2BGEzbMtusYCehh1GWneE59H3PlN3kzxFuZHqAiEAm0iy8oZqMZfITR16Ue9TENAmhj31ZAD6J55bxSpPVsUq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDPNo2X94h%2FUPfton7yrcA91%2BkMYVj1mJ53eckqusNt2gWfRyo0w7A6bxvZOq6hsHri%2FCPJWyQ%2Fnbmbwn8MIxglL3rtv5TiRj1P3ZQk7gbCHyxr10G8nplh5%2FEMEwEdDZl97EWyPemICnv2tQZZlqC446up%2FZxVo8QUe33GgvQQghoisnTxCiLxqHVLqISH74jYWAupwcF8SjP4ZjJvpdVH%2Baz1AuzpIE1tBhixbKZ4rsf%2Fxc6cR5XNQXivhGNFRM7w%2B8peITL225Ku22PlcChypppO8rJJfy05Bsv%2BSOgH%2Fc9RO5Pel5x95fc345Hbhk9t%2BLR%2BVQ1I4aDKBs3zK6LvhBj7KPW5tieKLni3M7JKY93%2Fr5g%2BixL2GhR%2FzTOTUEtvAFeIpLLVHz%2FrCfNYoVg%2BYlDKvPuGmb3bwDr5wn8F6aF2260dechEOnnYzY8IRAKkHFzyuXtpY%2Ba3sljsTdy2KIehtRvZa%2BmkekFH4%2FTjIGLUbpOERDmjOntOMHfbXkNZ1J0PMZtLI62Z0M2sa6hpD%2B2RgdrLSC1J7uNsebnwsEInwQY67CQ0OlP7913vOmnThQE7FaMP3JqnLS3zhsGDqgTfDN492yyHs8OLYBWLsSx4ITlJ6XftXGGb7N0jTenegU69hPwyNFd14vMNDb4scGOqUBq6rehitC%2B55jShRz2hsFJmP0dyJUP9ddvQM%2F6KcGxSp%2FNeV%2FPulh6iu%2FNPMafl%2BZ9eTHq1b6bnEpF1nnPAvBPqBDOvxjJ01x9pqT7829lpdMkdrm1uCmiTqqWuQ1KgTeV7jNhNYEXYZAE2K9G8peW%2B%2BQXTuD96azRWjdbdhfEwRwtv8%2FJsOy035iWicIGHeS%2BBlBh0gACzDlAKBGWyUBNUQqNn0g&X-Amz-Signature=f86948c5662f1afe253e91384aa2324edeafcbfc0da1c3afbcf980e596a2eceb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
