---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MQ5DZ2H%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDcPAq0LOYPOF%2Frr9pZ7u5oIxGeMFe%2BWYqQHCC1e6YP7gIhAMg8XODWRlMFfbPQqanOO3NXbAmUModGi%2Fzt1QS1%2BkEkKogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzAb73j6ijMhu45B0kq3AN0cIlzftYrSKbUFlAZVvEKLgeBn2dn8DQCatlMhKHZQk1W67hcN5x%2Bh6Kf8K6sOaev0NtQ0WqqdLgUbKwtQDl97N2e4RWtBzYqaR0%2BBMcfU%2BtONspObmyOF8%2FmFnHyayEDQOswsGs%2BTybszXaKCSItel4q%2BOAdn7FjOw9BZOOnp7oRA7CgQJo27wC8ZXfgVxtln1k4ChlX5bXrxj%2BZJsZ1hm3jTkxFLgveqd8D%2FopC5F3DG%2BPlnOHM51PHUXibCs5rguaEFgcyQLb1WwUYwmqTby1VL0s7VKQX7ZeAWCZhi8TzVIzoTPDI9IRGZVi19L7fEy30DK9nOG%2Fl5t1ijQWw4bs388x8VYwLBtM8dUT73VWQidPNy3caMiqzNNyXJxB4MyFYBSVE4kXjMCPQQiy52Rqab%2FQKwutKXooWfEdVIKAhTLu8AVyAO%2BfccLsiPcIc59dHivuyNbOVkWTorvKb%2F4zu%2FEGOMOt1%2BCTYx1MHKr9%2ByRiP17nTH1wbvpHpGtOtt8ETusjDGJ9nNBfbEuNcnESALhzJLeFQ44Ee8N2Cm3ner9O4kpaZW5IYfqHz13R8JYjgEhQybvIDuXlntV6ew74mrAuzYHXo7rnFFN6u9mAgvsiqLrb4CYLCmTCpspLHBjqkARwfpPT7X2lWosEhm6E2TvcnOo9hhS8dxoyFxrDfpQ4NfGPJ2qyhoz9Ok%2BRZ%2F6I2Zh%2B6Jut77MyVdqaQEbAko89DkQ0A41HHcLvJuMU1HHORe%2Fa2g8AVw6ALEwWhsKuyoSGwPowowzVM0wcPfUEWfZ77vySnDcpHXRYZfeMbAMA2uiVauA7xdDhtz59fJVPcUpufd1ze4JK9u2ED799wREGxkLG3&X-Amz-Signature=267a70e13dabe6ecc3e4b5d0257095ce0c6c7338b8e08da5ff1f358a68c09a8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
