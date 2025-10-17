---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676U2FF4P%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG0ZHP6C0%2Fm2ljowRekoozTN5O450gMwkf8X3c9afB5qAiB9%2BaZHSvGyCsvtnLNeJqpWVp6J1Zb%2Fb4eHTuCjAvu0WSqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSGzxBVIPv8rYxH5tKtwDrXJMBYBRFZbOO2BezbhDwd6QUPIMIyruXm5uR%2BsDSqr%2FQ5n%2B6mfZ%2F%2FsCHZU7skTBSArnDiTPt4twP3yRv1Ut4YJgHeR%2BkbRgSD%2B9uA3dCWKGDTUi%2B06ou%2FckxqywGg7UjeQhBMr8%2FpKH9JtDlIary43m3F6PAgl%2FjPkxAJNU8PDDzP20gL7YdYZvhEzDxcTUCzZHWVqDqMr23X5kElcJct%2FGTA7PMpTXukvCFN0ny2qEGieAs3DkxFrSO%2Bqtg50gPFXuB0qF9wfK45bdaOpEOMrOlAjB%2BWqy1Y7G5PBrY5SWKAvhHVMC33l2FdRdEVRKFiEprc8WvUr6A9KqkV0476ZSsT5A%2FS40iH1q2SygxAyAGg7CEV065JYkDfb2ehKboLZvZPNqLrTGXZ2da2hxPkWucvRya8IEe6ibr44mR3S4I%2BoTKHCjwfupuf45E%2FoAibJN1TCS9Q0yn01iFTgewvDast7xt3TlfIg4viyinER4TKaZZiDiHOSn4L42hF%2F2MJf7G7GiFLq4DF6el7pEIAP8Nc3NxX9%2BT%2FaoloccmGDcCPsifqei7VTSbip6AgV1b7%2FhBfQnLEmt00XLeGSM3iMgEDFlxBreMTnGt1lzH3azSCG1gSEtQJbY9OowxejHxwY6pgFZm2Ocev2bBL3I%2BrZuOaYj2yGNKjEuuN1TR08rUyeG8mR2%2FbTEhE9i%2FnzOlfiCkRFWhtD%2FTObmp2NXa5whGsaiWUbiNGN7PidWiLL71BCEdpU6EIisD%2Fxqb11dQf6ZbNj87A5OFfce85qAg2FkmXoAwHds3DgFia%2Bsu8mVcnEMuQ40q7yUOhp3Uhr%2FnSIZh4gfFaTk0tVCoeh8FvMdApvNWX73lNVx&X-Amz-Signature=2d39938bfda4590ccd218356c1ad440e26133b8e014a9ec452f90d3d6d3474c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
