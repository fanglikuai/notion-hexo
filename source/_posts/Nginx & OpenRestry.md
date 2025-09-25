---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGAFW2WY%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCk8K9cGydvFFRMNyhxLmFwptgNLx1bCYbYEguPlzpJUwIgATMDYKOnagcYmTTIIMcU2zMzrwOGizQ30zrlJ4Hnz98q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDN0DNlghVrF10AxQSircAw%2BLW3I5Je65jLkJC6%2FIJUugdjU4OeyYt2cMpLW2Oag7%2F1m1XPAfptWW8Zqi985KymgDrexOzRBYwFmBcJZeq%2BsWWabp4g%2F%2Fd7Xo29ZxW5OGxxQ2qPZ5FShDCuA8aM5pWRn5fVK13hQzDefBTZyLG8W0lVY793m2LtexWKs2f%2FSk4SIcQzsxwhLhScy4kfb5Y4iiBnhcEnBBLsqUaffaWTGHdQBbxVwtDR4X%2B89T7mRtzVU8egUaRDEfWIcyhi%2BnIzxmLawDD0sKS3s8mJa3yBlf4DpnCWj%2F7sTPkaXo7ehkFzrLMO%2FcoR8YODdol2bMHf4gPllnr%2BA6yYMpjVt8NiuvAtex8Kjaw5cKCjo2QCiV3Kw5eojAKXqoHOFZuaT7CAlC6slXEb065FO6ugsyQbB0LkH9%2BBIWgPukYiQktxVfnLdAVb90wBGRGgZPzPz6y4RDV%2F4PQlfGBxascZEgBbdtFnwE222uxxIWlfndBTiixjL8Gww3SZI88MO0JW0jfzv4WSnt04kcUU2kOWw49rGKDtsIM1YeyUOC1rfFuMS08QQGyAmshE%2BGKGDMde0a2U4eEbBKOR8LRKuMjwZNmhnKN89ZiRo13OSBP7QQ1tAiZQKroHgdSRvdUegwMPe71cYGOqUBRm7Y5QiFGvhrd1oJA13Kb1yDNmIHt1QiN0Nu9YYiTMZBzovbUi5Nn9e%2F2Mf5qLSIFj%2FKAxlBh21cWVFrlrmS5kJF86Vp2AT22IJX90Bm%2B47FQ196je01RPV281t2ejsYSigTiC9fPl4aTSvuwGeQ5uk8687gMZ5Z7zi9fyzor7SGKgL4wZ3uukoiBx%2F7o%2BnuGwdLPVgzFezd0a1nP1XOLEiN2vPw&X-Amz-Signature=f3da286f7fa0388206a6f9e89fce46b7ba647f2592511f3a5ef3c0f960d110d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
