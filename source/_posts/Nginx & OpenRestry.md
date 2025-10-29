---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXEHYUPL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFu4V7oQE9QqVYWMGVx2H2%2FYll%2Fjt4ZUFOGAqNiL1SZJAiEA4UdSVcvQXHd%2FazVTIjAOHfrCPFg91Hn8spHUcHt1BDUqiAQI0f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCMAO9CDTuiHIDhu2CrcA0CVoPqz2KbbtRB09kpNrwxw2r0rx7TW823tocDqHiGxdcYcoPkdER9qJoF0mh4XsHuCPYzb1KJXNVv%2BlDdty57Rt06pikS8IeqvxGCLF7uKIZondcX1alGVGTfT%2B%2B4B2SGu%2BQtmmVlwGPV8MAErK1QD2439Ef5%2FpIZH2i5hdU8qK4q6nFmSrqVCY54efszGxTKAKD8IIhzobF4qkj4lhSru9IMn5TEqF6a9zev9neHt3gZBXSuEhx2CUIwxvD0duA%2Frm30m9cn0D4LREIh3zvXELbbi%2FTlJbhVsmfNosAVHcfySES3YDQ6UiRmmz9glO7RHgU1IwwNuRh%2BRnbn5ul2rDFbuNrKgt2MCJbUxoBRqqcAYnKBs1nzE35YK19ucjSeZz3q1PjH3sjHXot5tODmiadUibPpiHry1XlGKbw274uVEa4xWizMzuRTFDB5jsT3W80aDvIEU1aogsisDMP2pbv6mb8F6J5BjejGIDW90CTgKUuVc5%2BQsxK6ai9BNsVVmqU8qdW7lxv5uOwfr5VnCuRiNJSbNH%2B1KXAvBqFvsfW7U0plYJzllgLd9MGIz%2FWb87%2FsMAUaG6nZ4YFKiGsUZT8cXBDRWV%2FMK%2BBrV17G4DccBuDLd2802jkbDMKGJh8gGOqUBPjiG3KjpSa5o0vWuUbO4qPxH1197BLIsCotDz8l8O7B%2FnCwp%2BUmmnE%2BpjpT1vqnRoE0sA4WH0IDLrrHycdYZqJSzQ8Rck8qL4pzJJkDgsrRx1EMpaaFGghBClHzfpYlqxeH3gHUcIbaKLDHJ4dKuY6yaz0ra9S8eOI0o74CLYDTHGc7yCOGjAFC9B6HPN03%2FWyHlN8q3nuN7kc3JRqe0xL9d6rQX&X-Amz-Signature=623d43751e473d4c7b4012cdd133e90d44f966157132ee336099d32b8531cfa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
