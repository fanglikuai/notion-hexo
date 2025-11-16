---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6UA27HN%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T230054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICdbc489tu1lbR8hNdeek5xNMffnLeTYUp9nzFdvTz3TAiEA70GspxJ02NjnSEM5uk5qYi91FGh4hWcpUIRhXaCST0AqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQNfuqDicn%2BoKkhjCrcA62Z8uSBAzIsQ8gw%2F5a3kYqPCZ09Ho03X6xxQnBMAttKWlRiRMn0WaPs%2FwM7sLCgsfvR3NNbDnvqJ0NzW1e9dl1ZTVcyAHhK5Vb6ymYkpgt3u3bpiDD%2BM3fM%2Br7a6cFcpIiJAgYfi2RWEV30Vm0GywUbVq09Gd4%2BZsiMTt5lcMBurk%2BHnE66lgR137qBQ604io2Da70U1rXkgi3m2GcLUrLKMUlt80XWel3RQYtav6cE%2B70Sd6MofB%2BfxTf0QLoJlD9ymKNQi%2BOpNvfS7i%2BtZL%2BNGdQh75CzaXc3wDCMav3mIYUSe%2FBOADWjU6pLoO8pWUrd1YR46XDWRjuw6UbfWrKvE51mQfHegyD2g%2FDg6I%2FH6nn7U3zibjbFhrySbibEdZj8lE%2Fj0RZeT9v6KQ3VrNaD06pU50ALyPrPsLzkjoREfmX503nXZPvEKX%2BqA3%2BUBAM76yShV2LN5XD%2BDcYVPgeUb2H0u2dmEgxjW0Q41rXAnKt%2FSXgbImMBv0VPAUIF0%2FWTZuuvCEGRV2tlTb3KLCs%2FQKdynffRnnJGKuQZuUDsi%2FCa6O0G0X%2BjodMtEW3cDjH74opnZ4yexZvrYHtqkpQa2dq7putqZnxct3OcQw7W%2BvC114xy0NaEr9aXMNOR6cgGOqUBcrT8oE7cI3qe9gVsYaebYj1sro4hjQ2qh9st5ZlCc4T94t6OnRzlE3XFI0giLUW%2Fw7Sli7LPC0u0nS04JxIhMdddm5hhu%2FysqYwza9ho9wYrsw2UlkTPIj0rNk%2B%2BVb794oooe73vPKbZ5H5OGHCTm8XtI4IcC4kYdII6A3OB78sW%2F7jIwXX%2BYJUsccNjtTZqGaK%2BFJXncxiTItVPzXJ4O97cXfAF&X-Amz-Signature=5a06622fba3bb157b17f31638a298fb5544b307cb027da631a92191d2e0bbcd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
