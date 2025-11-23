---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3ZTOAG2%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIHC2y1X11gp46P0%2BiS0mxgqHLthyi5mjMq%2FpyEqjxW6wAiEAolhALmpbJEoC8R8nXUTa7wD7HaW6MBwuwidHjEcItZ4q%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDFxPqo7zYUliNUcSgyrcA0ae0VCbNuSABK4J%2FbPXY0anGmWKS%2FogGoJeahoFKP6mjM%2Fr46mdsQJD7gd3kah4UwyhGasoY33dQOpkYqLzttnBv1dYTolHuvhk%2Bnj1IbZXkwOrgSwAtS1ixnHKlDv1FgJWu%2FV%2B%2FS95y%2FvWI5wz8EpAF6SkikDWZxV7GvX5beDP7Mt4fsbCspGK5dGBf%2FoPTip0UzGzkv0vmlFQtrD6%2BOZeCIKU3jUqnGswPNoP57Vxx1e00D%2BF9CVSEZwBM7CPCPMIW5uVq80i1KCrSBQTVZfmxdJcRjinM9AGOXIKJxtZWLRK7Diq71oXfx9aVdtPcazE0RrYSi6rBKIrk37wovw7%2FtQ%2FtDUKuvERHSnhAsXg2TGJKmBud37gU%2FMpx%2B2apVMHW4MvjNwhvQC%2Ftg95zXuxCVLqUIXrjNLcPf7W5fDsSW7ocjMAdBWpg%2BaA5uZyrXzOrhG9t0gz%2B%2F6DsTt16QS0oMwjlykXGEbaO1epW0WAbrZgC%2FKRPiqCqKTGpJa%2FBMjWg0YqZQ9jpuR83ewTVYNxI%2BBdIklQph9tCvHfhK%2Bp%2FCqAZ8wpDxwOm5TdaBtwb2GAc5EYdRscwvA%2FCPVplDkPuQ0rLvNvMA9KrZjKKLyJW7ODvvz2afhpdoMvMKCJjskGOqUBbd9pBzkYoaXWnk%2BRZyI3Di%2FctG9kT3T5%2BAB%2FgffSSx9QNlNum36CbKDW4GPwaARIwNFc9CdWb6XgASljo4bUoaKdYF2EuTl4UwfiZUL2BRRuh7DtZBaiO6xKT63wSWwjPa23DWIfmRIuxD6oK1WqhbZZiHCxgnYZaAhW8nGRSq9rWTDUQBYb2sxZ%2FPT%2FpUgbQRQN%2BABVgqIcED4Q8DaLSMaFgIU1&X-Amz-Signature=4b1f1f623aab143c0afd7359e1c0742ab8de12bd8c426e32f03377c5fc92ceb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
