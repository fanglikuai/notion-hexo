---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQDMUME%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGwy2o5f8%2BBEqDHRM3tUbygtbFVvr5c1Pr5LHa4ixP1WAiBW6gUpo8gNOHCFTCA0Tszn%2F3vspHzlQ3lH3CaOihbtCiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM89kTXxADQqY4Kl8iKtwDK6VMFW6sOTaQSAccHWA82abR4RM1DXB8esoYfWTFgUQOHViM0NyylyDT1Gvi86jv1UYCn0iFcsin3505nopxgDuBAh3AFq8lS9mHVmOMNaPKulQt3guwu2U8yauA9b%2BuEQ46edSfLk08KLltxb%2BuVTFxVWTuGamShN%2FcmAori%2Bq5J1V1se5arPu%2Fj9yecw51lnvDDoCq6tjCGn0eKK0G2EdC4YlInTgKAEHMJPc5G%2BdQvyC1v6aK%2B8blrhE9ZbwMW6FxSUYxxwzb2MQ11Cfdz0eAErqOkD6pkRU2HY%2F4iIduZd%2FtG3WmhcLFx68GI19JPPyy3Oc8ur1D6w%2BkPBcrieFJwMvHU8SL%2Bb%2BoDgE3DGvdTViCGwr5tN%2BULdOTyg6eixDsiZo5Hd0Vz0c75ysXN2z2bEevx1oC%2B1tmvGXGT%2FczI7TEBYrDY9eLHyAwIAiFCTCpKM2bCOuRf9NloxK24gityex2uOwE5N5YY05fA8ra3Nzzb6iFhZiLNmvze8z4MScrR4LADOu79275Jdd6gHBGDeS6xXzSQZJZwgCqGUJMMb6ix0Ua0kltYLFJNjkkb21sh4gTSMrw7A2oGM69C5h%2BUD8ixYQAouhODtibx%2F2NTeLnA%2B0ZkVhNzv0wud3%2FxwY6pgECFfLayoZeQp1GUobVLIVXhazi6K%2Bc5H7HQP8lOtbCr20plL0F0UCpifRaP8KfcJ485rfLKLOxq62OKznFof7yqB4Rqm8%2FEIY8bQwOsCz7o1gGXVRU6AMnja3sV7yDpjKmQQ28ZfAzPtKVGguI%2BhXUMNe%2F%2BPMdL7yNtdV0Dk3vFALU2fpXHw6mWkNR9eHpUq%2FKpnNyaRh461jL9f0NoMi%2FhbWmAM3t&X-Amz-Signature=a0a48d8388c651084bb602d5ebf3b0665bc5bd70b9e4deaa4a0efed4d45bae82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
