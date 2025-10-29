---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPVR7K7K%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCHHKjwtr%2BuNnZO3%2Fhz9N7BRAo8jTnznlGPywyM2xg3dgIgBNf8BEVtd2biJ4hjfbh5OneMTXRn4FYNViY30YDi31wqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM7TK25QDzpuDtng3CrcA%2FBg%2FPlijp2fFBeEGt%2BDG76tM8mo9sHucJjpfoVYHp1rbMOO4CjpZ%2B8Eu5dKthAFOKe1Af7ayoUd4f1z7Keb4mBpwkNiVUTRxMWFKMghtpAp2SdAjUorPwij%2BRi%2BkJgg9ymonvmlhyR6BImBG0778WgTEyuEplxK1aUBJSel3NZvpWLLCgWw%2BANlK7AekP02b19hRCR0EDVLHEKP7PW3sg5WaijBLbKTaq56Tqeuti2FYnQxWuMWSEkuPM56U23b9auvu3Ad9aZkhSHXKG5sYcbhcnWqC%2F7K%2FHKlGdHFFiV3BFTzPthVVDrhWOLdcHZnkA%2FnlRBe4b9YMQYB2pzEwr%2BpHoKfjdnIEccCy4zrClUx61D2a6xFCx3Hes8Ev93%2BF7zZkq6TjDNzex8Puv6nz39EpSvc%2BFt6UyTJuLdSKTSm5PY8%2FwPuFeMubaLTXh1yCiPB6imiIo2k2DyDeSigTyF8o97Py0Z2yemEOUjOJW%2FUkOKe1OHHT8vTeL8wKtX5Eie%2BSbRZpb%2B5zRMLTQn0mir6HJ98CIhuNDjYfnzMuopB825TraC2DEIpBt47IdQxqu4G7jYD2cNOKo9u6GMO538MTk%2BcFHEJ4w9eKRajhjMFIQPay1Q4HFbou1iXMILKh8gGOqUBcSB7NaI5mb4HuFcZ265zPHZIs1EewyJ5BPO%2B1VX2MOEQZUcWqN3sxLslxrVjgxgbTrGh4PvGsNAtaR97W%2Fp2k2NMymVHOsNYO1cwSlWxUPRjY0e7vCEK7UPZch5%2FT09SbndaH1Pi%2BGtE%2B6Pyz6nB9LuYks0NQg9s5y9AULxL2r787ia2EV5luW%2FWcbzUZtWZlWOjQZzntirgEcsWf1%2BrR4o5cZZV&X-Amz-Signature=abbdeb9b48b1f3656e058779626548aa3ead256a0e11b40297a336925c16f8dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
