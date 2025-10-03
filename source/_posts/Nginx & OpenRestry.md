---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTF2UMWL%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD0Ct9FZhuqfaBHkXADeJ5k1t%2F%2FdTNAm65GuTWD8yxJnAIhALNjF84O436N%2BTz1hEHfCqZTi4YapY8Uel3OwGM134UMKv8DCEUQABoMNjM3NDIzMTgzODA1IgwR%2FWDAqVP9LFiJrYAq3AMGUpppCeJynkaapZvyLN8DAb3%2FPPWPgXlNCEtygvLY47bZSHI%2FWt1iEkvxl4N9CLwTDaNCgRYLr3BekhXk%2BZiOWqmn2nfiz0WhhCOi4BcrWMrVhxH0fT9zqVeBe6AR8hUh6CXq1%2F%2BJ%2F8ZjMOa68WdKJ8n6aZMqJH3ybEhAZJ8%2F%2B%2BjpIZmdhyZg2MpOyiFBhtaG%2FYw%2BjNcEM6dWZfg78SK%2BzPDATCldManfVLzVo8ciPZLbEtZ5f%2Fnb4wrdSeUmbnMSWvfDrq11JctipSyg%2BB9NB7XQ%2BtzszD2jIEPNi2AipfHrNfgeuWjnq0a5Tn3IdLIfiVVwjMLa19znI1RN0s4YJGM6G7QIEvmUb2z%2Be75mq0t8LTZvXkQevMhCPeDaSy2085bARtGYjt00%2Fa82fP09dgeurP3TDaEk0r%2BVX34EwvDnVx%2B7QPZmfSGf8kKT9bHaYI1ffLLG7LSi3HYs21DYpB84QJ9MofrX3LIStvBUd6SCQ9FEMKe0bchU2jrhWJONFn7%2Fb5WzDFcAzdWCnlxlVebAtdPBhWZWxl%2FnOpcLMtVgypQYJzvROmJFpMtMLr7uViDp9bWtH1aTqnDYVHPH6zO%2F4nlRnDv8sBO8TIslzrQXcg%2F8nFbJnfqmPTD08P7GBjqkAY4ESHnwtZ0cok9jEwE41IQmTpuVGcMFQCkRXPS8XAiN7hnHQmyxXsY1g5nurv%2B%2BteRMtMvyPN%2BCr219EGknAKy%2FbLXco2RFNx1LLiBYnh02IWctPqq35R4Lh59yH9qV3u5%2B9N8yMJYqZQrFonEou8TElgevQMpz4N40GT1e9HOymazCOp3HVjucLubEiuAcdoLo5DZtOlkXtJnBQRMiI4JJ9n9Y&X-Amz-Signature=78e4f91a16273145a59f7a8b305f29a44a4195dfd9d78b2c347cb8d5dc43ffb5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
