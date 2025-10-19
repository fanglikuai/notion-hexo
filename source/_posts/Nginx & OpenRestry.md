---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7HWVKXN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T160037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCnRTSqvHg7aJqxFTGD%2BlFhJAlPsGKuIhlU6W%2BwYZziQgIhAOndnz3kEcyugRk%2ByrCh27jFmNVQCV05lhjXNxpru%2BubKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzJ4BC4GqPEVgDpzH4q3APM41ajo8IyQpwi5m1IhnbAbfBgG6GfTq%2F7l15g8sJz6HpOFzimF9F5ABr75vFztF1DDzjQ1Fxd%2Fhkh11nsPOeDnljo6Y3%2F%2BsgJu4HZgwoRiYXgCxb9XP4%2ByCosOp3slyLTaAoH8lpWMJ4jsHTLmS%2F8PomJ0fQaJZTJ9aUiYKWK63HXRlhEv4G5TCdRhNLjB4%2BDJihCIBmrqIrlIgae5SB%2FzdJupUyD%2BgpiBzkdt696vf3SyXTJTYsraKIMrO94UdplI50fJzBRP5EagALBpKpnxxrFt%2B%2BabduJBUIesffJdqBNuPTET%2FZZ0yfeNbz6lGa3VeRTnafKowg5tLh2ifE%2BLSnOFTO1x4Pb6o1Ltx6Ur3u1lE14IpEua9H4vsfLT1tBCnYAHiHfoytb0%2BwSTDMHEw9ZUXGrG8Rr24h0SixpXNMSF5txirlqgAQ80ZwABcrx9KaR6nCKqCMUORh89xjoGJkpYup6Il8dTxyO4zUBNLWNUJao99rKkCctP1hk4difAA9j2D1tVHSvznfpLoaI2IyJkqnKZHQ4D2kgigjG5UsFCbT2uJGF28r6MBNmykkqN13iwt2XzIjEFxnB0ObEEVdpdQJ%2FGQ1emtk%2F6jarTq9lGpLaEj2Enj%2FWNTCXndPHBjqkAe06u6L9MlQD7fXVkMI3DBl3rr3dWQCpq64m4z2Q0ber7MWrISQDwcXeMBJTa6URcYGlKZBDMuTWPrP5I2BtQrLK5lVTWNMLwUOPu%2BEwxAdl5s4G049sU5LckyJd09fa1d23ROeOAVwbRybx8bTrW%2FLnddeJpDg2ER3TsxZQcqJkV57avSz5Ipjj%2FTLYBlsayi%2FzmgrDpHgeslk8BbB2SMlmJJRp&X-Amz-Signature=ce02a5976ef1a3857153f4a4823b66a8627c2588bb0e979750fdf91b797eda2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
