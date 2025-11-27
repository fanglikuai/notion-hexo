---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVDEQ3NP%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5CDOGyI4Xuo4mNFhgGn7KDls8miihkH1aXYLGYF62LQIgB52qVgHmdD%2BN2z5U83ZCCTmLoaTSfjp%2FvOCh49P8Y6MqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNg4XObdxUkc1uqj9CrcA7aU28Nww0vLtq%2B7JsvYj%2BC1RTl8A4U8JqpP5j8%2FQUtrg8mOj5m2ZNH2TG86Rl9xtr%2BbDmxP2qGwQ%2Bm4CFR0LT%2B%2BhhUzdS5DVFuTj8peMR%2BuBqj9K65Km%2F1xCKLJf%2FO1o2fxmHqLv0F9ZMBwWehTKtsCErOeJojEUYNyxYraV3EmhFl7UUYVep6pzXBvGOUGyv4eom67J%2FcXYFbY4NuPqT2JBB5gMjDB0bHh6Axw4QleB2%2Bd24w4vuxowpISmgsBjvnylluD%2BIFBOqSA15S4SWcm2DauQy%2BKfcUXoyIGSoi0LzxWYqS2qYxJJeBy76YOTyr%2Fn%2F7rWDeP4C8WIDXqESU5utGGDSNFQ1RkYbckabjwFnDADYwfQJK7YXkAV5%2Fb%2BjWLaC5F1xUR1Zl4jnDwllOzw2oZ6Y09MJyUnHiscNtuAvGta%2FTFaBNZWA1OFLow%2F51JM9e0wtcH%2F5iSARiMZftvw9PZkPf1sz6ecfbKB88iTUHvBQ9fMyF1Ojt25qd2rQOO7UvB23AC%2BlHOCS4af2mHCx5BbwnP%2B%2FcDKIRsD8cBsHz1IqKxqBffYlmGxB5L4wsEyuIglaS%2FafnIUyaD0lQvx3MT6gX1JB08vVRK60mM78BDrJO6Y1vD7gxuMOuVnskGOqUBqs9emzfPbYbNGYFwXg8cNl4rUd4plzmfeQBD9HidGTVmuV18TZb3e2VHtOCHs0Cm%2FBbJXH3YPRGy0%2ByN5CIoBsVW5KlHWqpmLFo9JASdOhrRV7wtl6RZTLHFPv%2FjrjpoTdNhcjrDvv3mv47WEnjmoWq7ZNM5elV6QlTefFnKFeg7kUbn0nFPPSvY3ktC3dZsnoXE5C0uBiXMCCQ84P1w7HMkiGAZ&X-Amz-Signature=b7dbec2aaeae8012aa08eb579f5313adc4c06ca32c9894355b4705fbd324143e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
