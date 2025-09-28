---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633IHO723%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T200036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQC9ppbhkp1Ji23GDgcIdbjYcTcpPssIYfeM0be8FP5rigIgdmm5iIqvhwZrYCcq1uJI6yIBUZGZ7d0ucAixKJA%2FApIqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEB3PFbK2XIJOvT4bSrcA2I2idlaiOILbAaYCP7zFZrkoVTWmqJ59llNm5D6Kcui1i36ocWRWbDP1nVLjm8NFXRTRjC76K3LkDSf3iJbJF%2BxXsZVlg%2B4f9pZrilHh54zIBufU6W43ZIQrC%2FWEuMdSF676Z9F1mf5HqyrJ%2FuFij9AFgVGZLC%2BJXKTbzoHlNVqw%2FLMNgFJulekVBdZ9DQ%2BXEYKnz6FImKVqGbFthiCEhyBibtVxRf7VwOI3nTpsrzCnYcpITFXLWuoF98%2B%2BEvD%2BmHfjGKIfY%2BHWrqEXIIGg%2BTLLE%2B0tDvt%2Bejkm8GVFOcxbbJjg0ggYIpmmcDIgHdCvTZojrFxpEcPP6qpdMgpQa%2BB3FHAE206xC1l5ZiJC36RcBLcf2QUNXd%2FPp%2Fm31kET%2FWG4peJ4T1tKWqND1DMQQvVAhCVffMCfKfg24%2BNCXvyR1HeIq%2F2UFLZC9DxVJ48TJk5hgCCjcdcqTexuw%2F4mVgTDtY7PN1%2B3M5EM4KlJu%2FPpRtDAN5k7qWEwZVPUo7r0NEEzfqfDDf%2BEq2u5UqsqxYKdEGRsHiii4MOt%2F5Ch%2BWjjrVHMYp1unzroTpv5GByOmMXwjPsiEwh0JSh%2Ffq%2BSxbreHHlEfiV0anCeXecq9j8cU5tH8yAtqDYeUliMLbY5cYGOqUBN78M3hagkmiLHRtG5u1jBsPUtaUc%2FtqDhll0nBrsc2OIcyDvbzILT9PzW%2FkWRCt5bQ7j6hggfvoFvvTxl5RaUxJXR%2BohYCzM1uKm%2Fe6ot2Vsxd%2Bd3Lp%2ByztCJTLBrmYXr4OV5Xx2c8C10TAEvEeZ9s0WuO7mPqPH2IRvP7ucqE7V1yq84WAlbtrV8Se%2F3ltffe0kGD4KNZ167h%2Fu%2BlbYDyMYmv2R&X-Amz-Signature=4a70476777b7e16915466468b6a9dccf6d578b75e0bb7ecce38468e4dc0f2945&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
