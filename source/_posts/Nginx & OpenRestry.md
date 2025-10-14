---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HUIN2M%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvZGmo4Bv5M1JqIcl0qmFkv1dfRH4NzaVRxkQyCWgDJAIgJuEdMuWc4iRx0PYVS22CP8Oh%2BtcIvVOtfHVn4sID66gq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDBnaLklzKPnEMs4kkSrcA5%2FcIEi9ONE%2FAEvqYAdg%2BUH9oF05g4lEVM7Mz%2Brj1f8A4VErsjHwm8ZcSCdr11GU1cTbAWxhcgzBD6M2Bi0vauU2AWB2R7HhnAuikEEJ6vNr2U9kTZeI6XUCuKcfMHsbn7IFCnW%2BbIEr%2FkJax2JgVarLS0SG0Gl6JziyS8SQsrSBBSZo%2B1ZnSn2hYAPawXTgW0TADnRSV07liwYHlsmpMfemr%2BWoxhbhP67uVjcYbQkVPIPjfyfSZg%2FOevp0xNM3yxb%2FWc1p4i4V9%2F2oz25TfGy2VK8%2Br8DHzGlh0ZjTysa2xgvSEL%2FR42lM7WGhNusnrhiCc2puF9HnV9wBxvN4RGYIyAzs%2F2e912V2WVTXSGgj2Sov%2Byi68117Lm6IpM9TJdfkRl8Ka4uqMO%2Fcx1pTFQkcA%2BC4jqc4jf1HW3N9yt1BRXOt15OiVYV8Fb%2F0M7S7CJStnzrtb3G7PR3yBnIfKSoAcDozJ6uSuyN6kT8EMWU0bp0t1Cgl6WhVaL%2BPJLFFUY45zj9clo0M5Se3y2eG8xhkyOPMrhrWP4qU6wAXATTfeP%2BxmZL344fIs5P0nWYGgdJtnMO7kA%2F%2Fs7JBy1fkl5D7JXx7g9Eeg9W08Uyf5boduzdLKYoSqLWB4I6sMI%2Bct8cGOqUB3WUgSN5pp9RiTfXH5yMbhlN%2BiZM3buvbq%2FJ%2B9YEmU8cMnnsgbMSiTdX6PPnY%2FZIV9hQXEDXEvkwpdDJr%2BIGAhokaMCjdNRo1oHQh2QseqyUR2nyIb%2FHXw53XVK9%2Br3pze%2BacBCvq7c6xZvrXmu%2Bq9MjzdffdW9A%2FuI2vYEeiq4k4rxe8slLaHsMCtCNZK42Qq85F07A%2BZJWBn62%2B2V%2FPZqsCfPXo&X-Amz-Signature=58c794993b1bc8974c5dbe4544baca3a9007fec40c684fbafbc6ab22b0f112e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
