---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGDKKWNT%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjtU5IKJv4lPPrVodWkrQvfOC%2FthF%2BPPOvqwuTZVBhOgIgWycs3Y7Ny5hRq7NEYWhDA7j%2B1dDvFgln2qSB5iekywoq%2FwMIKhAAGgw2Mzc0MjMxODM4MDUiDMU31cHgWaVsnsnsUCrcAxEkcL2xT0z5%2BUxjrUDK%2Fkbd8udse0hoLlU2yJRp5aYhNbsF%2FS0soqEZHf1yfj05ogOkcx2uBjDyChzMBZBsVfMNMLHNsZmNm93GOccyjjTYAxENFEHRjyOH4zBw2vaWiKzpiaNqqLtE3RtK39fb1s9hcMz742v0rzDTt8B5hPqxnc82fUkNkvCkFHfKrQjpwNtL7FQzZBjgFoM1DfROvyuOGC3PMam%2BmwGQ%2BZjopgLDZFq%2BKmwju1KLT58Pd1uJoru%2BS4C3dcSahcgfUjpuq84dI415HD7eeYYl5C4wh%2Fwc1NiG01wwcn1nnZ2UTifyP%2Fs86tIVJVQahOP1fGUSlIRkIBL1Cae3zv5FayzQgNwAEu5Hpxe%2FDa5VyzlGYYYxRpMAFVmC9y3%2BAk7KEcq1D4hpFJUpZ8tZjmLAB4ZuucUG8EH5VqYVW4sWgdaftB5J6nFqGzRaqt%2F6tOCnWiKJfFHceQXVAxiU%2Bdr7KnTTYmQ1M09IK9c3tMDz1%2B0rAr872K6FJDi%2FfstmRANyg2%2F%2B4xg6Gik8aex8y4aO%2BDX6vPsKrz88w7aGYIPkJ6SAW206oBMSZXJtqVTe8UVPc98ff7Zw%2Fm4EZLKQ4mBBdLS2oSUoPEszteNbmGthl75sMMSdxMYGOqUBYrGk3elpqGiVtFmtOycsRE9fHwgUL9h24YIf4UL%2FGPoJEqPjexpRqeva2FTiV%2B1%2FCBE%2FD3lK9yU0e78WSWxZWqAqrZr65i5mRfSZhKejxAWr61CUXxu1I%2BoQOUwWWK4EQu5gujd8iRGEBFLb4LjXlxL6IPNGAAcssfIK07M0Zt43Z1BzKePu9EdX25cyzlBpM02C8saIPwjFdulP%2FtQBPVh0pn%2BK&X-Amz-Signature=caa113f46e70577b8bdd6f6902acf8ed3550eccbac05045807c49cfd80262795&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
