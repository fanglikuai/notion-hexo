---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466257HR2OU%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDBZn%2B6Y4AkaYU3oEYVt8eEv30LDPfg8QJVfZWBKqqkVQIhAMG2PWT2rJLZgVGj9y0ZntKW2Y%2Bn%2FOlz2zUAO0KxetvHKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwPZhvN1tpZhfmkDCcq3AOivPze42m1CWuYnRYkzjnpUCvWqIOU02S1R6kuG0KFnEB1UCkL6KeqGMdTv7VlvsRZXli%2FM4277teduio7nRFDL64am9FJILPHDhT9prQcLjaxw8FprH75JLQXQ9scQJzC8%2F3hDcPAlb9WhjRxkd8RVV%2ByO2UCrsiUyVsl60%2BrNDECtALVlESptvpBbnXHyXAUmo%2FzSkQkO66hq3yOYoZDtEBZ7YqbH4gYaJmOHAoxtb8RyExj2MgSjS1v6k%2FuqzQAXttfoTdaHnbp4Q8SUGZYxAWiGhtGoedhjaIRdOsJo8t2LR8LS9r5LF4g5xqbbZHhTKhR%2BQoKu0cT4Yn%2F6LoURKbXYDIC8uszV%2FdFgIF86OmtOau9B4dR9525AAXfqs6oD1LtOIDyzqCLHW542aI2GM1hXXgqNGWDkWuUiCNlyeNBl6CyiDcCvDtWDP%2BSK%2BnIYqttP617svX6W1X529MWXwFIutO3Ou320kWLRHAoBl8%2FqqZkWpXOQ0x%2F%2FatkRZ%2BjbZdOrhlc1bHz0Bo0zxcBbQ9AcB9ipzf750jARYcso74y0iyx4uGT5DTCTp14EWF6Gjm3ZxrOrWOulIEXCUZOIpzq54WiAWBQWLOElScYTUfSF7yB6kERlw37mjD8s%2FvIBjqkAZ7%2FJGPiGurSMgYJZQi04ftvAARrbc9uoL%2B%2BH6R3v0RMfTczAPso42CIjPXM9uYCGNiyBOS4Hjol2JUBrcYI%2BbgKm2XfODuWfubxBgQx31hetacU1V5L8QeCWDWVMDUHtQxUJ9fD4%2BZPUiS5ZmK4DRHVJRii8Ao8wfTWsTaPZLDwBJQx2BCVym8Fe1AEGPVr7nMpeN%2FfurWDFaf0No2BzbrdwLLA&X-Amz-Signature=d37acc7d624515243b2f541dd7135db42087f97d5bbfe65ec2d8a8af84c447b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
