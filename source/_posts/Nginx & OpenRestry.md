---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNTJERUB%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDdeo5%2FH8z2hkrneqwSwRxj6j0mBh%2FZ1Il30VF2%2BD%2B9cAiEAhyf8MFgIqHI3DpP1bXERutrsniT8mhxOvEcP2SVvhHkqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuSMbSaH052%2BsgnzCrcA0OBNzz8bcPvZbSNojVMsmPTp9hGIrW42PK6X1B0XZ1%2FjxdSTxsfYT330P%2FZvSEAWxlXOjxsjVBfUib0CH%2BVl%2FMYfgvk642BCjdGz9ZFw4vwzvyQV8zK95pXHdvl3GgCQGwQ2Xhc7U8EzMVbMUOphKHntVWaSSvG%2F3VGwyvaFXq7PurvhGyNJ0OWcWz9auq%2BsMTLoIXtMpXy0NVgq%2Fyu7dX0NgxJjMgKgrgWevhhhVtNk9n%2BH0fn5w7HjW6G9fpfzyl5FrBlFDbe4eFfvfOUnt19Ck26rA%2B0Ea%2F9rRCzzq9dhvaRWncmAOSubZnN%2Fme8K4X%2B1Gf%2FQVKJyNO%2FXZ7AiQ7H1X3QVOc3Ae9e1Ga749jPSYUNM%2BplPBe418A%2FYkReHt%2FWBrPLH5lNHsBcQkGAib8kOyl3YkRJ0Y%2FMSTELcf2%2F5MbtLV0I28tvOOua5AYaKp6QIvppLMwOMaiFg%2FzgGo3Rcar2uK4fPu9X0NH416nS2hnhOndiYQDinBUm4QlyaWJ%2BLgiwvJ6vF4znInrKpNYMM9UhcW20xGh%2BIXN4pfmEIkVTxXVmIcASNQ4Wiy4HCV7Ss5ehBuovlpVJ1%2Fn%2BFF%2FuUpqL7JtxNvxU2DDOySgTio9JM9mDDW%2BduVy6MLnDs8gGOqUBlHaaa40nZWeePFa7ZTxJt47y7vPEHACvXKyOEKOqpZDCKWyqnR9jj3z8Q%2Fqml2454ARgXdoqa1w2BCOzhG2o0qcGqQR8i6DxNsUqeG4FFxxHoXFvoC41lTvauYuwZCFxam2xsz0HKFrq62OLFOBpyQ2rhfvkyfi27L1o3toROcVZdFI%2BNfg8kKpQIl%2BVN2ay%2Ft%2BcUqSlbveivJvCsyJOiX9%2BFrq%2F&X-Amz-Signature=dc93e8c9a14aeaa9237efaeb9f98c2c213dc23d6c1c7e82fd2f61029321712b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
