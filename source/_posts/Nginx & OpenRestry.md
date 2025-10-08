---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VAQYVB4S%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCICMZTYXBiBMZG10WhVRz7ouDZuxjOIEMxAcdHCVXUApNAiA1RSCZ6vDOzr%2BuOLb6eZHWHpc6oQS6R8I1TpnBYtUEZiqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLZNCTttdD31pJoo%2FKtwDxbioRNMEoAcqin8sZ72W%2BoPI%2B7ZgQZeNOVifNnEEpWtl%2FDi%2B7%2FIGvxL0oQpMwt4bfPp8Xjex%2BfLO%2BCvAHJG4Anu14GYqEzMKwi3DhxEcC8BrV2iivr0P%2BTpYr3JqZkjHccCLDPHpHPEj%2Bf38tM7ldCbENTjQww85ksL%2Bqi1h5N5zpspJ0yqFfEvhOrs1dfG9M%2BMnyEQomgRqFoEavMKpGXOk6uVQPiI5vzxehOGar9WZhzSoEIBxhU9xMMlYCBRV%2BnTkIbAt6r3JkaGoAba%2FO92PZzxSHI%2FXceXMaLM5bJfADiWNFt5XJfQgDDMhbgVyCEtqJaD%2BDCDRzqN7%2B6ZkGeG34%2BLQ%2BghlJYr6mUgvkjtxtVHhLveB4rGeHQavsnOy555OkuzGmrTPLg7H5HXsSQJ9x1WAR0oFMttlJ3g24ozFV9k1sCZ%2BjBBCxdnbVr4jeCTc51QDoDwB9BtNLY%2FBcPSQ2swYD%2FuGMozcr7h5DowGUt2kPgrYfBXCTq%2FE68hYujM3bsKllO3L6hFxKfNjBrBLyIvMoK0AuDdjiDgSFMMF8UlHt2BxIWxQ%2FKCb9%2F34mtML38hGr6rYljvuufoAjM0SVRSeIZn%2B0bBmNQrxbu%2BHd9gKTPK3Nox70hYwj9qZxwY6pgHCUzURt4tiWHwcHzn23zQvgQRTkXYbsug7YxgMRxzHqsmIFW5y1Fk6C2n6EINkT%2BfebPI7phsQqsnjknC9IOKfZH3RowYJzfJ8nm4nI2fV5Av7nd0WMRIlrm6qDRqaMEoQuL04A1c1fAPmPvP979M8ZA947s5pXicNnDqsDTrtJ2Ic%2B6crWuwySyl%2BUWmY25FXxV3BAByklQRP%2BFjC3sa78iB8ddKK&X-Amz-Signature=1703be061b51a24523f199ba11338acbe786c4cbcea743826695dbe2ce1bbc33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
