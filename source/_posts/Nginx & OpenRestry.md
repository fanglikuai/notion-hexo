---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBPFUTVR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQDNdmYecUQImvRGwdFFV0HlC8XBECQ1oqT2RWGVhgjfVgIgcByKuM3l0Km3OsPW3U6jstAzXO2myQD4zgKG%2F8n4t5wq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDBVF0Qk163ZTcG8dQircAzzWvT%2BRA93nJDJbE4vMO3ScN2f2tJVpwq6zlW9C6wg1Q51ocpLXMf8bEb9NfvQUBiIlfe1eSxuCpx%2FhRRwMEBM4sV%2B3TbqSwByhNMzJ4PMd3py3IrPQm6uIOYXrxjMZRYdsxY%2FCmnT39Xcg1b6GEOOcTPAw8lD%2FZgTfFpPQC7CL082oknJbLIsnmU5Tw%2B%2F0028bCtdEJ6vQ30y%2BKj1UnX7NSGcsN8e3ShPinWtj9aq5t4rJm44XsfpY5fref%2FQgfEc%2B7xuHY9CDgdlx%2FG%2FVheNmgem4BitE7tJ13yCHmWKol9EFmnuTSWv74jeKpc0uA5pfRxxGocgexBAQs4W78JZcP8EsDwMLdzF3EuGKmVJlODRoTNUUOkxUvrQ8YZ01%2FmSkOVtG6sJYwUSvyeNSj7Y8hEljYY%2BxLcN84jjZ%2Fi3AgCpQ4tsO18aLIelCC5k4wNwYfhHoUZ%2F7q8xMtFqdtHdo4y2on3zqm%2BSTBipN6WGRszR7RdDyxDM0rOoO%2BMupTEwdIQ1sx0wSQLagMJ3TaEZd8GsqNK6rtQdSNOkw%2F1mCWj2vfWA0QDU3dG8iOFUhjVP%2Bdh0oSAl96Ww0VnGspjUy8LD%2Bu704JTJzBRd1AFZxg9tF4TxQ7vN9t%2FaBMMnMz8gGOqUBiISxLUjfprh1bsLOF9tQZm98jkKBQXTinoyhI2PM%2FlBn%2B72o1HURPTLJSIhA96q3vXOLPOyPyYaaAZI5LKV%2Bo4w0Rl31xV%2BvycdqnIQyj%2FaZPl%2F25qH48BGWML5B1w4tQRqfWSnm1lPdZweGaSqk8xgavjPQemFofqrH42AMIdS%2F%2FmuIw685xkMdIoLKD6o8nvnxjv0gCDXgyLy3sOiTeGSsatC2&X-Amz-Signature=1aeaceb9d236157669840613a75e86e5e53bdc1ff1649a636b6977cf10f94a88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
