---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EEUS7XV%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCzJ450AQHClT6dpcApp7U6QDHdDj8IoU9xd63BtY0pnwIgUBfCLZHKB0pjYUfmzbapgsQkEBsyAjbs2Q2uN%2BxhXqkq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLpNpRu%2BJLeMn7CiZSrcA%2FFGnQgA4%2FtTuhCTmaVW9qPSNsfRWs61dJNdnIUoV0pc0AOD0U7bupiLNo115URzZ%2FclZdgV4g92iGxjFQ7r6jEHynz5dV35iumHWxh0bTTuEeGWluWVO4SQzaUHXve0gqt%2Boajzfh2Ykl94r2nB2CkeOyPpF%2FLzGm3mo4DFzO9bo767EXV6y%2Bd5uzJDFMFsKfdRkp%2BZYVXsVM%2FwutZ%2BurmOEWLYOs3Zlvgrb4T1E2h%2BhFqE8TTYwkQHW4B5iiP%2FbosGtqoIbwC2os6imAoQL1HcJEUceEVKpxsmcaFpffHO3QkLJ3FM2U90MvKgMDII%2BbqfkH8jmsru45GjtmH8T8V%2FZzsarvhx21DV6T6NO%2FXNeqPAXDpbkb%2FykcjXs44teFoIE9aBE6IuuPGF4DtNaSDIrXQuGxECzOyDUIVm3G5gd7ZlnKhLjFwzNKUnKDQgvy7QUJhbFAHlXOnkH2DKVVfT%2FVdhTDO8lxbj7oVI%2F%2BQZ2h43jfpyHWuHak7ibVrEG09Sn3db758dNpx259xBpQ%2FOiWT%2BVTmBah359gN3GVvPDOSYW6L278ZfuPnh7Yd9YYfj06uAHeRbmKyyQ1qbj%2B%2FJYwgyjj5MaTeT6nsCwv2EQ7fmcY4O6vNEBmO0MK2cv8YGOqUBDDpNtfSad13RLNrorvhNU%2Fv8mMuRv1OOjmQNDxJMm0oSMNF5rzeeIZoj8F%2FuP%2F61qHJvUkSTqCxQZisWYIk57v5OuwlqUgna2xkGyzQtXgKX4K2ktDfpJ1x0edvDscaWW%2FPRWhA968BoYqfHlZ9uqzTy6k3hVSGQRjtd65TEkYo8M8nqvoyIwzrLZAJ6Ov850V91pdud6Ee7cPp9cXXbfzcUBZw%2B&X-Amz-Signature=104d31c950cc0d534bbcc884c13fa9a9125a3764ed5cb6065e74408894724809&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
