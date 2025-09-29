---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOVBHCGX%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T110128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIFDsgBZj8tJCRVP9QuUfLNP49Q3g3McDDVKBudMpIvArAiEAsuhWE1ZIcJE%2FL1lb8%2Fc5jEFMqIueFLSDuhgutaKTPCoqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMxV%2BW5omuWcMo79kircAxKV7udXGFQVdg2ApEzRKwGu9gLSw0Wqdw%2FY%2BUThlCaCSlyeN43z5kW6oKeGgiNfYdd4nuWhqWPjLOOB8sRu095axl22e9mZJoApDIMmLqD9XBIj6%2BB3CjvAVP7d0%2B%2FFMfVbVKZivi35%2Fje0jqepJA1X14gb3LWzhe77AwaDX4misPZeCUst8Rpdvvmh9j2sP%2FjawnTMgJ0oCb3ellmDI8e%2BfOZcDcOLGkeUKQuFgupNOiTL%2BcqJDztMAQEC5hjBdxvmiB8psbO%2Be0mWlgB5fafcJJfGuCs%2FhARyVgFNcJ0aAdY%2BaQu2CH64%2BR%2FlmoKpkIg7ZmSWAdJxgPuzsuaziqyll9dbea1mKVsBAHn15tWgUW9Q7z8EfZu61i%2F3mxWS1Vomku7MbHryxAJWOFvWkneIRydUH%2F%2Bvu8J4udDlRa3U6H2bF0%2Bdd98Ken8tvf3Q0aXnDUdvxhGuW2mEfJVpoqE%2FMwTETVSKnuXaCmNXWyFML2pkAwCjs0n9A%2FWFH8yfGd6tgtQwS0rLMXQ7vumMV6HrhjSXrcSPNpQz%2F%2BsiJ%2BctsB1jBOnRfUquu4hRISbBXgEnWkULmX15la%2BxLg5CFCCLvqFdyeCM3WSNcugJ0KuyHL8LBojPQa%2Flhwa3MPey6cYGOqUBV%2BPlYvAHKPqfv9N5F9YjnVrECggvLZBv%2BWeolXXskpDgUglmG27o5lJgTHWpTBPOOXN38WcLNNkDU6kD06e%2F77MVhPPmYFtyTtUcoJPoVAiSzMSPJCTDjYDh7Kew7XZzVPa3Tsbjkh6SuFEYmMdR0c57bU1%2BwC8OZfY0dxJQo8zDKcM3wwRA8Ewu%2F6BSGXMP%2FvcHsYtwLUbdeuRHjFW%2FzJMDEG6%2B&X-Amz-Signature=31579f1c968dd96373e3beaabe182b4a33ebbe0f72ce9407af95950527663bb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
