---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635FA3UU2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDQbhmqcso3nYsL1riEDTSnKlDfjDWsIiLkoQ%2FFHnQUNAIgccA0ieHjPZDTC79f0mbIDj7O%2FPqINkvDHfi%2FJe6w4SAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA2jWe8LD61fPLXvrSrcA5qEA2WJDGtL0AxtEDEqOMEgeXHXXsxl%2Bnm8%2F2IjBdbEQtC1GXByY5%2BwavLpup6yScZdgGoMlAlhzMjmH6wrTH48eLaVZXvEYJWQHo%2BiGHi6qATCBKldYmbpoD9bCrS6nE4xUwZtawA7cXkc1nIlTbwavQNSP%2BJJcIkN1qXjg35ipXB14CIp0T76XHJPa%2BxUODo6Nlc8li4MMIK9OfGJV%2F2yggGQ7Od6hiyzZOu55zdXv1Blgyn6fnB4znHf9UqA36QXBGhhxugCD%2FQc1D2dZNQMkrpfZy6mJhji%2FknnO9gDzvCwZy7vRWqpsQPnloFYr%2F0j0CoT%2FHm03nOsrybLN1iA%2Bj%2BkswgFpvHed6s6wqDFaKHZ9%2B2K8bDGgPWZb6kY%2BK%2FUIOeQ%2FwdUtGqhdxpBErmzbaSOSsh8YQHaklkZN%2B5wvLM3hJCmivXlORlvWqIKmgjHn3mbAUDJ%2FrPOZ9a4zqz0K2a95TIDSftqqz7JgZSzE4HMJXut%2B0VrZbC2Y3wOTcOApiBDTLVpY7mYnAxw54f%2Bk1rhNnmMkraocGX9Aw9O6HyTyU%2BsNzDZ9SXLrTETd%2FmmOdkZJWs7rw0iKGvyFxIMN1O7XM7t%2FEbQNJ08UgOdngDfqJePwmkxtOS5MISs3McGOqUB%2B8yVZRA5cyeLZw0FuIifcuN9aufYOYw%2BKbDr2OT9tGU4rKFGBWV%2BLPrjH7FN8Sy2jRfuRS9xUHZsrbNcsTFfJpoIIdbKWCENECLppKVOzNbgXCgnJsF1Ic5W0acTZoZaIlJezW2rT6Whgdng4%2Bojb4%2FTAkE954m8wXDKcYyO%2BIlBznAZHi5BsSbViimUL3kMvEbSAtxb9oTQp02QMI0PPQcT1RPs&X-Amz-Signature=31e33b3d3c2607bee27e5dd18942568f2177222e78cdf55ec120cc3a0d4e7cf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
