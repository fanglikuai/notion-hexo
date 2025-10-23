---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVG6NOS6%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH5x9DC9KYfy1s5S5NffZD3ktlY60avi%2BAGl9xQ21B7XAiEAuuCMB5XR7nFg%2Fuf5yjOlZLNfBz0TPghpe24klcRL3RAq%2FwMIQhAAGgw2Mzc0MjMxODM4MDUiDNeeroBCswTl9qyPUCrcA0B0xuubHmqXEyGQPV1U9m4lDgHEpUpdUdqw%2Fuh9wKa2mDBXbyFPICWEeidvmviASXG4OfQ5BeTtLp2DHBUIarp24z2fBhDMc5rT7Vz6MOfSRyZ%2FutIyIqAtSk4%2FB9oPjjv3E0GpKMf%2Frua6I2%2B3qcTlsdY0O%2BxYF%2BKkBAtyQ23SQiTMnsj6ty9aGqZOhkmYuaUXYcZZAwOJTMDlRtb3Q3XynNe51Ni8rGJ92pORKnGxCIufs8wlOtkhYR6Hatiw93BYf31wtq205MNlPXTwwLEg%2BFgLnB6Vvw2cUoqkqdwQt2dDf1fwRrZ9N49kxvnumoULLp2AU6iPaAC3UJOKhqJz77lYeZTXfmEE31QL9%2ByBwqy978xdPz%2FENBqZ6Pq5tRGCY%2B0sNwdMczqC%2B3zctCuT1N%2FlPb3LNMRxIAzMUMaqafoygYVziezYa4FenJGi4%2B3UXK%2BcJuJuU8TDRYBsDA2IqXKJLRxnmQkPsam1SIpFIa3ZQt27uCMjVx5PqtSbr93W56Kc4wQ%2BFcu6uTUQYguG12vc2UwaNBjUKnHGXvK9OXPQMo2snolYxdStpTW3NzMHprMynLVRCtOzJZNNYNJwYKjrWJESF0hhCftQAvNRO3s1Hzzw3Eq053J%2FMOLR58cGOqUBrHsWy3nRYEsJqkTcB%2Fr4RuGy1yKlOBig2vEGERdWnUXYUsAgXRQSw%2FrVexB31EUIa0uksMznsYihK6iUHS9o0vgb7uIZk6DwSkpdCby1YexTKi2zjPOuVS2B2x4Ob5BMbeNU%2BWjlYWUytACzXN2oY4lc6HNSLd4a%2BtrxOJNImYeM2UTb94qVRjxLZ5117kmuk3Hi5gyvhZueGn5MBeiH%2B3Q14ZEK&X-Amz-Signature=bb08039caab2dcb88a9649b6b497963214188be20e233d938a171d050e6af811&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
