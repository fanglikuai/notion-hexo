---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SARI2S5%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjwcYi2fZwftqr5nSsSfvWXMQMeHSpKx8byklsOSOI4AiEAg3gkTl%2BSICfvF35zwThBwXOfsHkRTzRztR4avP%2BIIDwqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFKHjZxL4J26PtEJ%2FircA2nWhKPvw4SaYr1a6wdXhjnzXMNiBR9aFlxeBg1ju3N2gddB8mG3xngYNCLn7DGyFbX2UpBEOYtjWdfsy71UEegtXjk3ZHaN1ypJz0f21L%2FhGF5JNOOix9GVHLaIyS6%2FKLm5lp2k%2FUbC3t1FqmcH40AZI1BF01p8OoEiiHG8kLKHCs5LJOkVw9HZwo8RfDOo9iq%2FpIX6Cn7GDGwDlS2wuwIStpmhx9XTH4N%2FlGTITuZXNdg%2FhxeQKY5zDfelefcQ%2BaFrAeGQ7aVjt81rNrUP3wx6D3xF12Mnvj4elSbe4E6lzfd%2FmSg4ZB6UQ3Wu85Zq9FM5mkoUsj5E1XdbQiJ6OCXv50enr%2BMaO9vNjDb4CgSjNMS4TyccnGD01QLGx%2F2E6IXriD1PqJS04XekqA4Zlm6o7Kb2ODR0SPS19hULLZsokoxbeHo2YNG%2FwugcuYp%2B4HNA8Yg6AGXtWB8aY4WeF%2B8AZ8Ma8%2FRxtWMY9IotxA%2Bq7KKEwBax76jZuUYoyuQ2rgUu8k%2BrXlG0rA8TB8oFzjBHaHgiXEjIVoETr1hpE2ZvmA4LvuLKhlQr2bQaxg03gNbTKOCJ%2BXp%2FLpbRzvXILON520dcSU8OXy1zHEc%2BHyh87rTrl54%2F88HvMyWqMI7E8MgGOqUB%2BAob9GbYrkEUvPGwLQ%2FGfWJN4F4Cgy4UV0s5QsSKs1tw9rljsjH8Wg68D%2By%2Bw4R%2F7M0fKOkTXlGRhXNchjkQn5hv5vQ5vB14h2txF5Lwvjp8T1Hmd%2FAMFXZcJJBQVRDNlQVWvhyr%2Fo90VrA2IePcMx4WBcOGnZEFik0FsMxaeHRr%2BGIj7tym%2BzXpPko6itMsqXQ9VihXZedLpbwA9HKcz5t6ZWEv&X-Amz-Signature=9c28e10a92bec4deb00bbd3070cfc6e6b7d0640a9bc3fae599b5093c3aa7cf1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
