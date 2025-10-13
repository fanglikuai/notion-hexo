---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVIBCONO%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5fRxR6W8WnJ29gVgLqiGBnDz2u3gDg37hzcdJ%2B73S5wIgA8p7YUt9iHWFa9%2F%2ByatbRjvPkFqes%2BJY8XAID5UqE60q%2FwMIThAAGgw2Mzc0MjMxODM4MDUiDEB3RSnDhKA5mAAZTircA8mcSfIdEV8dGhGFcRAQ8p6%2FysZ8W92wpJBTEHJFqnbjtQBgrWbZHhNlnKjWuaLQCATxDcbCRiyd3kwOwfzyFNhl6%2FVIxRcyj7m3XjAh%2FJXdoxbASotJ98bBwptHzzDGxJXl53gXBW0CVFe%2F9enIIwWfXjIwCwX%2BiVbcnsA3dErj7gjhVn2UtISoAJ0J0B9leGYXf0NkbSyV9qAQNa3LvWyQTnBGWJqTBbzZRNbEm5lGkPb7LjfBabl45umNS0xFE5TO%2BrMTxoEnzWngb6Q%2BvZg3vs9PQ2Cz3BUxJ6n6ZzXe7QWMig4eDW1StDC0ZuWLTka%2BXikjJjyp2FzaY2PpUlWoB8047c%2Bu7qf789d40npFtXXnJirE2xrcbaN3ZBbIej9kL6lIRIOK7rVxn1AqIJLjrm838XzFjeX8qvxhJ5MEkJqNQXaiI9y%2F07Ysvbueft%2FpazS27AaractmvKkH19CQKEls5akYIg5P5NsTAyl6VgAXVntgH7PVIJA%2FjJnJXu5pfyWSHh8e8j3FDBPUqx0%2BD5wbuzJ6Z5p84UgUJam8fA%2BfqvM0xCmIZzuTs0hvRh%2FP4tftuWAGKNT7Nxa5uovPbJQoANWkVrwkr9dDaOSZNzV3pYvffpkTDKTQMIPTtccGOqUBvncuzo9xCWahR2qQvUfsZXTCmnvtRXpb92CwrHjM775xr2E%2F4IrW03JOr9AsHFFFZMu9q9ncIF61Ugy4kHg%2BY8T7hf2tdxamds4SzMY%2FxImrILednshb0U9speh2HnZaxnEW03cc%2Bdho5J7AZd%2B0U5padQZTtB6gBk60gn6hlyLNFTX5EyFfWbhEr6gMUKNtYJ9uv68ba%2FLPWY5z3wYLB4otQ5FF&X-Amz-Signature=479a054a5ea7afaa60350d6b93b732b5695f828a4c37639763e114018013e427&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
