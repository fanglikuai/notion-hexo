---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NA3RVHL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T150116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAd3uPSxm5%2BlBMKWA31XwcuEDOjeWuB7B7JuqsFsGlc4AiBNrXiasKHnEBfoqtwErswxTBeltLA8g%2FFKErJ3Y8n41Sr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMwR3Dydf%2Bu6G3d3kkKtwDb78lFoXRfJ86I6ibn8HL2UczAAfgh5UZj2ZazXcCc17LF5lEnvApYvuxsHTO4w%2FjpXCcN1Evm9kSF29eq%2FTQ0HOuYIzqu8urIPRuIjlSUzLcSeErXS%2BSsiNYIduE5KgL0RlVWGxnrm2CXSQzRCaVFOuh2LjiH%2FJLeRLTPhWk3mFC2aIjVLADLOlr2u9HBZLCy%2BK6UX5iH%2F%2Bkp6phXvwNIUW6eu8Tn%2BfNE7xbRulW62nrgO27x4YA5UOf5U6dwFf0LwR5%2BkZ5GVzKJlod%2BC%2FAciX%2F0FeGgublAC%2FayG2lFZ%2Fzd0QU%2FnFCMHXtUo%2Fhr6%2BimBAJJa3Wp9J4EAoGOlPHgCnrbI1SWZ5rshEmG0Mqm38k0YQcLbc8zki6HtUEpUS4K1DokwR8FQROAHBgDbctlHbLKGEC1kaaVslPVoJfWoV2yngIkUj26iqeMwaRymWcnwi14hOigG3u3nDErei8HdiFlU6lwCV0pj%2B9JWkbylJ4b7c%2FEXS5gLzD72sFf7Bypyfbw6rHV5%2F2aZoSBjCh8D5pUTizmxYb8MOMjih9bHDQiT36OPxq%2Fk5zZI%2B%2B5fp3J3OPxe2fbttbJp10xP63gtLJPh8jpUgh1zI6eBorZghVkB%2Fymq5ZP0s8I6owlu%2BiyAY6pgGd5PmoXdBBUproJOvOV%2FK80lvwNllebuD%2BynO1Y3xo7N0S%2Fdx41iEhDXM0oIcfxK8BbnquD7wOfKnrZV%2FaRyvebjTXb%2FpD1BBswx2iGCB%2Bf4q7fS%2BeISuwjO%2F%2FfiR8pZgRG6Tu8FsaZuZ%2BCQacr8ARtTvscS9yrYUtB18oX9FKvaUnbyRDN5tbnVF0U%2BiI5sJDQyXo9HcweZ0PjROeWSBKirIai9dk&X-Amz-Signature=f477732614ac2cd007d676248905580315b92df1a9cfd997049bf89bd4d4ac20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
