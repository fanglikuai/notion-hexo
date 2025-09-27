---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRVJ6P%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIFpFOkSjjBx8D1Y89ubEMHrDnRzsqHbUnExq15IvRoEEAiEAyb37Nk8UtgIRoo7cSn5LxZSTGUtL3Zq16EbiPmLuKKQqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAshf00XvrXAAIstqCrcA7Rfn3ctEZCPDf0InczUr2nE9TpkFy%2BaI5kQITKtX9E5GGwIVaw%2F7Nu048c7SETowgcKdbrtOCcAfBptetwpExJd7s9dortCWQ07jqd%2FdCH4iMJMC4810V3LDAMOoEbDPlWR9PAu7Lh%2FfFomhFBp%2B855DNTY09CXaV2DPk8ovbUr3r1e27oiLRtO3PamYPWy%2BkKrsUNw5PZXlAnN4hTBQQI7kBSPP1pxIxW4SnJby0IqY9zQaEiZhIkGvf%2Bb2%2BZKNulSPViGtUV%2Ft4Rzfrg3Blw47cIb9ifmsuM87RtvzgaLc0sOgMSygExn0lMQYSXnOo7gR3BEdfGWe51T2wdHvFsandTd%2FgNeaY799BB3OeCBp%2Bh2yR4h7kqmiWgImgV3G5zJB0geoxus61N%2FFQFGqxJ%2BRuy96IezPMlSeEBToXdnLfyL9tyXjZqvoeGawdMDwvh6x28Jd%2F7ioGvf%2FCXWpOqId7P2UOUDPAyNCXkC9ushlB2qETWY%2BRr5C1MQ2OxITnEPjQYJdh2WsmKL32VKY6erWCWJiY5BrFX6mk7RuSDi8dZ9JYjaQJ%2B8238d2WvPHBIgN0gA5FVPy2i2stB%2F9oDnQNp%2Bw84uscYYVBgouHZy7OG%2FQs7QgZR%2B6aMIMNG83cYGOqUBWPFK2H1zTtIosTLgGBvjr%2B3c27HKQ2%2F0av8eQB6mYQx%2FakLRf2bcMx%2F887BAqxeqVWSBjgym9OeWIjg8rKN3zaeqMmxapaE1YLTqbQU16I9alq6LIQxlHh7rMRGEVAipxYbhnNlcXhu1u%2Bey2e1k9hpAQhiPiIrqK9Sh41xqgZRvfSZRhHc%2BuWgFLd0uiK%2BwiU4IgxTYtq1heTy8LO%2FV7s9T96Qb&X-Amz-Signature=af32d9288999695f7c7eeeb56bbc280debfac9ba8952fed58953db04989110a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
