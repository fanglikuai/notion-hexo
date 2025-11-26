---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46647J47XWG%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbbByXY5qQ0Yp4YTYCMzDXJSzQeGTCYQOKzlh7fvryMQIgJqUEl8ix7gWxwXudqofh00UT0sp4QW21L2q9nBuuJJoqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKBvEQHXv5KtJ6DO2ircA3Id7ybQRIm52x8xJDMvhimMf0dwrIj3eErKe9KYWW8p2UVLtdXV2E6BIqO0O2sFNHDIyfYGwUJWV2NsxbIY%2FpJZ%2BJ%2FMoK5rxLDmFmY64oVgfs8pdUmdDLKUjSw1Rhc58oJOE81FtUoW2FZWg9iVWV8VWv5c3HhTYSWI28jD5%2BKba9X8hkb%2B0s6NlUWC%2BbDifuK9k%2FtJAqMINZBXN7qbXTkKW365a9jhBRuvG1pzyemVRkryqXw8U9KXiFpsdTOnkThliDcHPlkGRx0C1ey8hMQNBZvOhA23npDTYDYzIEv75PJyMdfXYpnDSwMYt8EmOsdxhgVfw83uj%2Fe5jhSMhjBAeUqSUro6RmXRZW5NL34qo32tZX5UB5Q8C5APEBcE5c%2FDsAIndXyenitTayn4pMW1nYnrQMpsR0hiW%2FUD2GsrnyXJX0XtSZLWeItNqDzJj49mai03cmLkgXhbL20upBfImMulOW%2FRu1PrqiZ05Qv26GFanbdzTWTBTvLBadAeImKZMkcVQ6TLHOG2hD16lkxZpJP2jI3V5axDNQ5zHtl1yw%2BEUD5hHMYy5wWRGuknBmWZ9y1fcuPZegC0SIZJ%2FBR8cYwC1hsK%2B%2BvjtVHtLcccIDkY9dJ6KqICSomUMIeNm8kGOqUBieLnblKx8pFNWgz4uxo9DNq8ISEFF1Pv9gyFt8ZRjk0cCEkhoB7Dx5jPgNQSprpC1ag2nY0iPyb5TPOGnKLgS1dch%2BRaIJWoPbVv2KQ0R9KB%2B9Xetke4N0YHNAjhRSl%2BUJYWF0xhZFS2xXyFYVhhUJwp3OdyN%2BqyFaD4Fkqg7%2Fy%2BvK1KgyxoFR3scRQOwHp5hc22miKo3ZmSw3%2FjpT7EcN7VkE6c&X-Amz-Signature=a81a941f49f0670c9aad57b276361e50d9510abfa629d2ce915a8fc69a870af9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
