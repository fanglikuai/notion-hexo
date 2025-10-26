---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3OS44A3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBCerCjW3602lzhdyoVfxey6NPMlZW7Bbj04Af1csxqMAiAgWCq9UMAurNJ4KzfipAs%2BhF1A3jBhyihyTpDgxoMNliqIBAiT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuywDyOOyPC0B2xChKtwDTbunwH1yQUkRWaJuPNUU7tKESqsk%2BzL3VkcHqZYz0XXES5yJFjzbVt%2FQqL5UomZizA5nJ6R0WS%2FYqMb2hAlpBkMVIOSvOAuxP1t4oROrk1f%2FyVvI%2B6EKTaoeH5G4SZE6%2BiGqDwchTlRlz0ZewaIETX6ldESr7iFig84O0o0Vw3%2Bh1YN5gcjHVhSkoc%2Faj5X3Dc63Vv7MlWmPuNrS6cR0zBECbNTr4IpOd0WnHnTq%2FfLEM2XKOOIPNAoFFzacKP1nli5JWCLonSsT43HohCF9AXqCntTIj1C4k6EO9ON2hfojJTbDWLA3XOi6ml8IRCeUAEs7qJIbv6%2BZufKCIpBQqqhFqEFwM4HivrCbZdARFS1CpTEB5UZnhIk%2BVX%2BE7zU4%2FeFXwtWct7HR6Sb52Q3BAYdWbOSQTsSd0vssz4cEJ7GVePYaq3Mbai%2F2N39JrjI8KMGO7CASpEehLZ5pR9fKhatSm%2BF7PO6aKSvuWvhIfY9IYyblYIxRDxQcs6kRM6fNIivRz6XpYpORlN%2Br0cLmR7qggSqdURsREbZZNd7D4C%2FWS6YwxFgz%2F21bYw6rY4IHbzS1zLPR1n%2FFcz2Zf4tV90Vs0CP0cg0s4mO3ZBos5j12Qoh%2BLSGDI5MHHM4w%2Fs75xwY6pgHPb%2Fk0X4kW8jwVsztVvSAaGZW6NmZVKbdvInMrJxioI%2BFYy%2Bx%2Fx3HIiiyE5BO3yQ1wd9b5tbJ3QpJOQva06Ir2z5wEKQaiDnGroYPyQTKPSDxr2a7HBbCoYtT7nNuBOpsr4yZ2pNEBFHxL%2F9cWwoZEtZYTLoW30JZP9fxCArJNxxffT%2BB%2BVmV%2BxF4odPIIbTHm%2Ba2UtO1%2F8nXPiDcx7xBlFQd%2FTGKQ&X-Amz-Signature=c58ce87ff0fd6f0b8abfbb2a0e08dcb40cdedb45c70f08b039547442dc9f7224&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
