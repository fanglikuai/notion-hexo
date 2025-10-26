---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNBQD2C2%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGU8QH3v5BaJ%2F254Yzaq0IPpT6zq3vUjW7kNROTMQXWuAiEAn1TYROhDkTxbS8xChfmQKH5Qa9ji7BUe9D24L%2BLOdOUq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDDv%2FAJP7mOtZ5wm4ECrcA3Mzzk0wRP8j5A02GMI%2F6I0yzYwqHXmYnqS5yB1ajNyVTigPeOzTYs3GZB9eZyYeAiS3yow8dokPDXoNX%2FruVHNLskZ7SV5g0MxTPQ4umrnXUuot3IO9ZNqf9Zw8Uj3nUkTpFFjA3HCWdKCJSqVcvO07Eg1v3IiEaxQVR03kTukYO%2BWI5blAC9mglSZEF8hfDWk%2BK%2FjRCzyVaDAb5Kaxz3LbSSzLEACcU9wzK8fRVCUy66HuYOWwtbR9gQkz80fVALWdYbC3Pa%2BaMuDyL07mYcw39%2FZ3vJkuYNTerpM%2B3LzlTxBcJQ7a2BDOK7n6WMFhpFxVE4fKAtLsZ25WAkGe%2BKN2PA8%2FYfk3FkbPKPkIYemg%2Ft28Vwirk%2FJnBfNnuo%2B2FB3mrbTnJZJ6%2FQ9ETHi1wZXgf4WXkbafXNsPe8Nbyw342Km%2BHeQLFhqEbgz%2B0jRm0Yt0Qsqox5enVllMuZ5G3ZA%2FqVA2QJJXpUlyaRiFsntK2k%2BQT3Nrsu%2FPk2DSKBMrE9QyEjos4MQetbAaV8%2FWyKCEmb4IeKXiLK1VaNjYRNZiRv3yKbQq6DJDgVoh7YUwLz6I9OYeGCumVCQxX9fPyyl8GwLCynZtL7d28vk2JIlnOzDiEdBabMGia1lzMP2Q9ccGOqUBUEngZPa1bwoSCJKVbykWV0bKRIXdswYLhBwD0bNYXchObuhdwczWSaXuLg4pOcWaY%2Fnm68evZ%2BTIlEmlTGnKksX6jtFhAUuIiP%2BajqiKImd1I58zgD2NSLqq7xiIlrXUFC3tMI21pI3ux7ANePtw0ayJFzg8gBdCmYjDBa3G0TWIWUZyqtVjX%2FusMoyHUTpjVBMcowXhIvSyt290KNczqYZrWn9Y&X-Amz-Signature=82c6c9a4b457beb565fce8994f2cf389fe42db849e8dbd7078b19f49b771c81b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
