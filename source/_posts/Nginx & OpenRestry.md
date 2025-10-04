---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMJ73YHT%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDA4Lb53K1hwd6Eo6Z5YbKBtH5n%2BZlVA3XEttHRsNCbnQIgX0GcWCI3%2BfvhYJ49mwzvc00RbHF317pIP6fkRafwRx4q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDCMI%2BaCR74O0mCUL3SrcA6M2F7o259cGLLc6884U%2FjPfifTOc8aV1sBd9tkNGZJS40IJWH3NGFxc5P8caWb8IeQitTE2oI1t1N1GvPhtvif%2BhYc06KNoFsCuJIm5SVg43Dc0xJLoe3HcD0t6a8RG29vHnkLG286KQfjizRfo8hiiAJEgWSDPEuhWVdL21nbUP49e%2BQXvhHDJhmPt0E3JY4hSPIzALmgJ87m228zFTE9IyTApWbYkb69xp18KRt8e3dWu9q%2B8So5DIjW9%2Fy47O6GDY2yFf%2BpybRw5hibAIyevTPO0lkfkskiHxf1V1X%2BxleipGPCG3y0hPJTgOq%2F7Xgr8LUEeb9FJArwRemv3abT98IhoSusllvjO51MKun6KGk21zSXkRKVo6xvvAevQx%2FCiKG%2BYyUtgho2oXnUnB3cWlCMckgI0AU1ggz7xRAfBhZigrCGhdLjy2WD5qUJS2Yr9Mxg%2FyrukTRtMisnsOesN4cGuJ1rVZsns8M9A0OKftXIfP3q3RxMCYgNIqTPiTu4yky1ryxJhNF0S46etmF36A6jAZu6PDDrmEnioasD4jbTS%2F%2BoNCzH0vi6pTLWL66rKy%2BhZn8vOEa2JjDQrMIo2DB%2Fdt05APpfb2JvLeQr7FbcEuX8s8es8Ifu2MLecg8cGOqUB7Vr%2FLE%2BDAkO42QqEmutV9duwF4hvBnAi0ZXUpbjf6LQmJg6XqE4Xitq5mRT4DsQO%2FMWCioW4jI8x%2F0DrDJe1D4dEXri%2BOg2PtIcNZp7b1%2F5DuJKZWpIp5zQb219tzJqFvqGLOFFwj7GqT0e44O0mxBpcIgThVaUFlPpnM9arH5aut2xz3soM0yyvEnaWJjMXATBNhRmm1ieHD%2Fa32eBYgBBHRgNs&X-Amz-Signature=ab6f578807215c2125773348dedf06c3f63372818cf35cf985a0772c297d32e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
