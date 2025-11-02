---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCAG6TD%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBQhJTuhuqDmw25kk5CmrqfrBxIqLNMxVA7a5ktIU72BAiEAhBT6UJDF0eLSLWbz1wSMgIAPoh%2BBAIkYsHDV2jfmnp8q%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDAygsP4%2FkpCGvWPplircAyk3s5pKtVg3f4IgXKAJMVWEcrURv%2B%2FyT%2Bp6WZIxkycwGPRrnYUu%2Bme4L6vDcGWgOMOkbxmMbRisfeZK4g9W37ScD%2BBhdnvO8CJrHx3RdFQQ6eHXySPHtmBd5tFnkUAn0c4%2Bk%2BDJlgx6RNH9gTdypU%2FNHDFwq4mQexvaZoyCeY1d7ii0Hp2M2S5zhM1J2jIxE0wWy8vdEDBvhVoXJuKWWSjwcyXVC4xMGWRYpneKhQO1SepsOBBeVaM%2FCEEcZpvBZBQ2ChO7pHFOtpkETFUc0BGdNJTSGk7y0aJQI5bWp4LGEpkRKwL%2FSfM0ZP5XAZ0iXDucu4PnyBuw6vNkGtCw6RrA9Zh4nFJFL2fmaWwLxzd5P1AYnWodPLoNPO6BsRQ1VRqJWDiTVgh7SHaO6LsbY3AieVP228EFEdenRtgLjJpzyyI8rlK9q9l3mH43rjy1jP0AfBHSdHPHXZ1oIMKmjk3w232ZGZu1XLYPMj6d%2F90kJBvV1VrXeJ%2BsKYu01kolrwjtJ9iEiyT01Ch9XcCPeoDHG9mWUl6LKQ%2Bc0AKnClUU%2Bhtq6%2B7og2gS%2FTwgBswu92h5aNl%2B1RSDNOQhaRJ%2BnhtUKq%2F5dLtjaevzOTDvA7azsEJ047zkgnXmufC0MN2DnsgGOqUB7u%2FYkGzJJ0RBjeO%2B86PvP2808SJS5GZy23HMmL0hMY0gtVXF%2BIL%2BwKbzA0prG2ysNAda5MMiiFEKWuAXO0gZ44%2Be7v36yOgF81uTgfJ9TKtsi4om3nqJssre4zQ00O8DgQAD8gxJ%2FaEHlkgC6W3AG3E%2B9%2BE%2B71uypUlTgr2MkprMQs%2BhKIklPrNGnWDycckDhz5mdFsukAZfo5yHVBMn7bj3M9vi&X-Amz-Signature=e78d45ff4729817ab0dd3d8514c8895a3e100324caf5b2010c650f2ef845ef8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
