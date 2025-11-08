---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOLJQ2L%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCID%2BuNytqZkqKjR9WDxU4tyvxy5WfYD0s5ZD3Kj4hFrlKAiEAy2zKAYe46DkIgdK3poT6cwxlBg%2Fxweoi9jy7blKN4SIqiAQI3P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOdj5PbGqoRjJtn6fSrcA0I%2BeVSxCZLGbOkzcr0ec5IClVOoWrwSpqKNw%2BLcMkLw%2B7D3TQL47qzOl8ZMGm3DT1xtjytBBEPBhoInpCIF7IXUudgyT8deChw8xYlN%2BH9anDZXsD6DCpg0Drwc2lghnUtOmMJsN3HHPq5cM4bnK9kQYmvVhcN0VREdS5VMMxDX2foz72BNaGCMhu1bpOBQ6yUvqCh2eqeLCk6R9BOer1HMSui9jOj8zRwBvALbBb10YSCgWy%2FdCfznnPNQlCYQrxAUOVDaVWYMk3%2F9jbMBLv5q%2FAO%2ByQgRaeW8gcvTYPtBB5goUKySTnHbuBf4JbNrgux%2BVKDCYZSsSUQqP6RAASxr%2BMxpD9OzpXnvy0aMoLkxt4QRGGcyYMavpIpiSCTVFVRVfMzRd6LLi53%2Fsi1BW%2FHvym%2FiGA9spILqE1evTOvWTz6wb6Osz9e5USwohfrzdV1LTNjHPwBV4U2NUwDStVPLnAsABOelZzBoxird3YIUwh5dq0y3Fld7oQCHgt5TrMJhTpVH89HrAQYnFKGn5IUBLcPKKDgC9b2NGVRN7JHE0NuCzyV5EnbhSZz6vaEfk%2F0sc4QYIw7SI4lGk%2FQmbd6xHQU%2FzMn00u7Xg9G%2Bda%2BQl4Hu6mZ7ZiKXOk5TMJGavsgGOqUBxbtGNiAkqYI9MSCBPvzm0MsJCQIziFhXxUkTfEiuH%2BpY5NE%2FYHCwMhhVKYsgo8sse%2FOmk53gMn8rfVzdP5gUI0zQcmkjWOUhUGf72lVF0kxD7S1svbXjJsys9mz682VFjgjFvYJgNbvdbaS5IjrjUBBgaDEIMd1gaAZwaN0vHpaNkbybRFGpATSo%2Bhqjlxrw2%2BoC949MYE5prcxhQjTklGFWvUKO&X-Amz-Signature=3e7f4e1e1d319a9a4188e7e28c2a041c5e6cc991bc5267de25b9b1dbc7432180&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
