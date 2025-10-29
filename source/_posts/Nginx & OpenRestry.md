---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WTXK43Q%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIHjqFkSOQMzgX%2Fc9qncM8umQ0yeDQHhulgR1pKsqRdM4AiAXV3isHYLD8Fj79ihth6w8asy8eVT5UgcMBH2uyp1idyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BDrK652v13jYfz4zKtwDK%2FVmB3fTId3iJHSC9gZNoMaf0DZlP5qM2vgB%2FJt0QlT80B9VqpCp3tCIAy%2FFsc0WHNXkLht60WkVcuYITnL4kWarzL5pSU0pvnr8%2B4kqWWprwk17hGz8RME5wIXwVcXZ2h%2FlvNhC%2FmZP0KS1uxYS3Y2JuTCwy6oVTVkQps6BRtkkreKaBopIRgUZT5M1d7TGR5WyQGWRvkb2pe96%2BCdhqAyNdI7oDzRJ1WVLEkaOzqzdbRbs7SokSqQtZ7qa0lV%2Bw9GodHv3dtQPtgh2bcwc7q%2F%2Bf0uqy5VTkfsMP6TC24XI2Gjoij54Hep1HpJHpeQI5O668gtd7UjzE86oGl0rFUxpFr7uomPKl%2Bp%2FGZmHj9D2eOmX%2BF8Yr045UOk4fC4xhUN73LNcbSN4IY0va1WSdGJ0Hj7Jzu1Fax%2F05m7ZaW2FbfW406E1ZBTE%2BE9TQ3xlLBmB8NABRjRq%2FX4edxsL7dL9SfwaiR3TvkK6vBaT1wZL%2FutsCW%2Fmdh6FMgD0zqhnyfMcV4HYxA3i2hH%2FSETvxDiN7pDPIqBQJh3j804HQqza1t9co0WJARGyrt6W4SiYuuidAYcCwXCZ6vTpKOoQ7MpPoCn1tu9oaB%2FDvSWILcKVxKMKnL0nNGmfJwcw0ZOFyAY6pgHxg%2BV8VZ%2BfgWIkcMsbx24Fjx7ztPvjWIDwlL6F8sypKH89jNgpUsOyOWms%2FMwDWe4jZl9yNcBokFXC4YLLGXFPyZZh1S9Hdj%2FfkSy82jZrT8fYfibuQWci1vlP0fOz8maVSk4XN1DK%2F%2FaCsKiWYkTTDssuXl%2FYts7rJwTrmQEL0Ltg%2FSVvU5qY%2FOupbyzGF%2FCoJWCb2bFY08HZZD5ji89JHsWERzvt&X-Amz-Signature=af9695516d237f1d5a5f3dd61a4aa026110b11b69a422b346dd3e69e7d05bf31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
