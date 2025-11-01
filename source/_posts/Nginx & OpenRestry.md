---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664OQOGEE%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQDEkghuyXG9QlHgyUGl4KPdZqDsCwC8YrfydbU5xtAlhgIgZl2hHrrKhXyR3LtiGmcD8D%2BSMnDYUoqHMkYb4neRdScq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDItgywClHzsh051LNyrcA0Lm1mDkaIn7BF6TjcKS8k8zofgkEqYsmETdM%2FoxI9By1FkBNbJGEjAt43lB6OgbArB5yiXg6847KtbN7Cn9b6iGQ5pdwRlBqFgxZ9pFdiic7Ab1E72%2Fxuch8uOQBeUS7ZOmqjBrrUzgU0NP5B6D6VEJZQTa6qdUUMZDJ%2Bp9mHxFjLl10A93GHZ18ViVs98FII9v92uzHIlmq9LlAtZlCBch4ixUvSqcxAVivzQbnwBWcwfI1TlMyo8fpNkVzfAlPYpJYjDWoZ5Gfffv6HNpXao%2FYEW6sG2qOvuV06T4JoYKDK9u3HNVU8Ejbs1zMILR9ayhee3ApFa2FXMwxkUDxHfmKQiL33bA7utEZaAW459Zx45Mi8L6HKDsMrqMfhtpKeiy8Me5rcENBurZ8NH4i1Td8vlHc5rk3bfwS4BxNBbF9snJvKjpvYTk4BmPE2YMX7axlIxfig8ajN%2BDTT%2FLdXtO3HIy%2F8wSVnMw4ftWdS76Pcd6yqlKQK7BC2pKHwPjlB2sSAuBWBfvJtpl%2FNk6WzcpsxjJ1i3ytKj75XLN7yxUSTDzR8eXox%2FPMkeSpeSJ877Azlill46itZbGv1Nu3N%2FeAWNWCUR4NbwLMxQj1aYYvR5rTZ2n9cQVWj3kMPf4mMgGOqUBtUiQUuvX0GD2GvkfH%2F0dKruOoQ%2FXJEarCAoCRnUKx7xhBvPCcQvZAznr7Ti7LWzXG9C8pecuums0Rv2m5mT0Swf5QaP2m3hA4SNii%2FG1ByWNT65FKBJ3QdxWOJddXn%2Fo65tnmx8cfBud4X%2FeoYh%2BVK0vMOe1g31rlpNnFMy72Z64ZTFJfkVsxAOywQSaGZkN2sOP34j5pEMipT1x6jy87XnTyRS5&X-Amz-Signature=25e20c0ca99218e6cdc122f85ffb87079739bc94e4c75dd9ab8ff1541a133749&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
