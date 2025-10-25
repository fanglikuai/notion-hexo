---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMATCXBR%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BJ3Vsfe4fdL%2BiJmWTV%2B38qERDO936SVuSkDkmyTxp0QIhAOqqHuSJ6udflUeiFOrqqBqbCoc64ZBVDIxJrZYR9n4bKv8DCHQQABoMNjM3NDIzMTgzODA1Igx%2Bd9yroh4qOuD3%2FcAq3AMus6jNpPDU%2BuEHZ0NGOoRLax1aTGhOT7r%2F6AF4%2FvtOClJf%2FFEnel45Qun2JvAnDivV%2F2EaNQDNFobEf7O%2FwkfESBoRaAw8qU8EC6Js1zU1hkDA2jaRnHcLL0sxWeD4bf5DR8VYsmDLmdHgbAMuiYkwXd6xmJR1xfCSbR2SAa9LvMaGvxM9i7g4qeEz8cbK0vv9XRWxyoGZY1ixqRvrzoMwWsPw7W8s3ALymw3R0eEbR04h4o3K1G6oQivqDSZKJlMVEKAq%2BjMJmD1PAH58S6MP0qCfryWbgulYxdgsLEFKUy38pDCCVRPQXou5nM02anXFn1jHCtaGYyp8UVY8%2F4WmUsOwIe9aoX%2BMlIoc87Lr%2Bmkp1BxGad9Kel4Q18i3wHQlNJV61XXW%2BArAXKklOr6BOAyFm4TxUqx%2BhbWdMXFp9JNqC46IB1dF17zVXxWrYzjMeffLRdvsdLI8E6qckbQ5F7jK%2FXKoE8AMoHwAhH8kijTaphXO5qBfsQdtbU2nXR6AqeFVE%2FAGHJeXt5zyDH9RYnk9aGqDgSuIkppDX3CnCLPHJI5Jphv3xvgPiibwiZHYT4wEvlwVF5h4AiCYRGPMFhc%2BgIpkEsllqOZ7lDCYBeu0Bb8YJCzL884t%2FzDA1%2FLHBjqkAeuQCx4X8dOvLl4kE4223CbIIqUPSCDmkBiMfPiqObknoE4qXOeyR3d4M%2F6%2FfX8Rh8Zk20liB74g2M7Kx78cDVRIt2ZZxPaVB3hAuAUcOU3rwaZA3ANIi2FYrXTC4x2%2FlVxadday1h9i1VrkRgtLmJafAdT0ZbN64RUni4ElWMxLDyvws52%2BqQq7D6wSWJUxIl3R5XZlggpk9UEv0EHdEaJtO4X1&X-Amz-Signature=44f60d9a3edc82beffb025da7f2b4c444c4f191b9eebefcb1ec1091d5ac3b228&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
