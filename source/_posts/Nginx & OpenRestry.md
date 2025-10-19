---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NYNPVCI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHYMJINH5NVIY%2BoUIZiRl5UnM%2FdlVH4t2kd1QuiBGE3oAiBApNgZi0tLZY%2BeUd9lJOHCWU6%2FeySKZNJvsAE2yKRMLSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2vYbYW6QAn1y8xzDKtwDenZMwk3eARnt%2BLVU%2FBNpqADAJy7TRtk0slvp3MKjblFPI62Nzo9usKIDAz5UlDDh06A%2B0avILaYHDES6omjFaScVSleJzoHUSLJtPn4gKRrHnKGA%2Br0DYf3A8Y3%2FRnKYQNmNEnnglQ8CeQ0G7GMRiiahN9dRGxqsxbxCg8tEgIcpcCQDwqoguw51Y2hP5R01b%2FgO9a2FOIxFHFud%2BPVDcWf21uWWx9dogsOTUBYOnxSBA%2BN1azn7jzLmkFXBCmGR24MaGCQgpL6EU34fOTD%2F5xOiy2KmDR9aP5rYYc3uoU5p3wZ7XVLJjWbqu8R2kmzwlrmY%2FG6DnG%2BG%2B9bhRqMud1p%2FiJ0Pbxm7cWr5xpdUGgJIunRV6jo%2Ftow2%2B1xFpGxsvqG48rQ4oKsDFrt%2BKRnzLI9NJoIU%2BeeGUpGpj6K05amtLxEZxu99JFo%2Bqj7CM%2BeNoq1V9VRiU9a%2Bb7TtZsk5JLY6Ohn5ajqgkYw5U9c6fTU0I%2Bnij0Rzsdih7SJyvErlqwfJWjced5%2Bkhw3zp%2BGXiTxAk2joGKcNI%2B57QhOPp4BDECAoSY8DCwXDnaet8%2BTqdX%2FAHcrJQ69EevWSk7IaiQf8Q9HNhsozVNhywu75rtJI%2BGJFoACCt7X4aJIw2tbUxwY6pgFXsg9RlG0U3xF9fnBJ0Ph%2BchKkK0jZ5P%2BhG%2FNyDt0MpWYuMssakX9j2Cw1YnkoHPaU0vC6LeOGCIykCI%2BToN3RgaLuo9Paq9C7Ne0wRE1BEnlIg1WK9fa%2B7LMN7YqnSGbLn4lGaB1VKEEMaPd8aoz%2FUGBxhquDsLebHedFSrNfw%2BVaZIffcyuJ3tD46t6DDVcZb15sxu1ALDucYu3LxVfJaDA%2FAKd0&X-Amz-Signature=b9929377a45dbc61acde8d5639587b30a4b292756e2fd12fcac8e81b8e778e03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
