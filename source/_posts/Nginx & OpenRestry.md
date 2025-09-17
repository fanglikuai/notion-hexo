---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVYQZ57V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIQCEEctbmWQw2DmiRZKGHjR%2BxUAvZZCosR60oHWFrTylSgIgDlp24wWlGDy8X0kaVvZIqPBdfU9GbN1DFKRpiZS3TWYqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNAQHdpU5EpWooQJaircA%2BeKC9lK36CJ2NnDqf%2BquppdmqFnbshpLp%2BZKCDXRYXHGe7tOY4CuJ6iG82GxnqIFdjHGJ%2B6h9sz62IKGKewIrK2Fp37ApM7NRu2iS1XIGC0cN%2BXvOI6vg6HMKLBr3G7sZloUn7yBQ2xtzkdIdsJaemEXA%2BE51u62k13XpHZ7ODFNE%2Bsc0YlkhW4CqkyLqCUpJYOwBOzWEvMmXYqnwfJxD4WyiGV979La6lHfcJY3UAMN0%2FfIfxgWPZgHlJqKvk7U0yf3YWEZkOn9KQM9ZQqZVc%2BzovVdsKus6LZXB3k1bL73n%2BQ2RE%2BPvUFuLgyKA79Yu0vB3CZqie0ZAs48FCSA%2BEPLDLfmzdak7WmY%2FVve8%2FqCOMo9%2BqGGjy1AqSoGU2RCGldweZ6o3O9bcoxkKzdgGMon8gnic9zMBK356atGlLYYmCK%2B9HQdJduJF3%2FA%2Bw32t%2BrfqExU46uQje8uKFGq6LjrAa5YwBheFVP8wJG2t5cY74PgCUIuZjnAgQdCejjN4ndRTIUOfbziYrc1eaUnYysRn54ZuKuYvFaBLXJL%2FOdGtFnItO3Tl2caf146CNq%2BtfKrRb5Mko65DhuBDV5JGLFX41P38KhE081OVEOt%2Bt%2FfN5m5H6TjK45Vmd7MIyyqsYGOqUBw26wc2PYEs%2Fjlnftazoz3jugC1j2JpQQphEIyBLbR6dPh4RZ%2BHWCm7S%2Fd5f%2BlnIaUOhtJvWwQ%2FKN1XF8H54jROU5Ay%2BNEuAkwJWdG5P3Tbf5EUY53v8fLCGBYbZSp56UzWluN3sPROOHYjys8a4c4lT%2F8UCpTDV0JNjvf8m8x1mpBLJIh6%2FPPz3goAnqkc4J%2FYnT%2FHa1Y2qwRuVmSaVBc8QYBHBN&X-Amz-Signature=c6e87cd32c777e6a5e48d262daf552eae205c2d4246abc3b1be6b870b4ad12b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
