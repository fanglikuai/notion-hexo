---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RDNPU6F%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4Gb90RsXmSCkeQPg4pX3wCMlH8aDBbSqDc4kQevRhaQIhANRyfaVNCb5v%2FGTevxpxOSfG2NnOgn5Fx2WIZ1KCPoMKKv8DCHQQABoMNjM3NDIzMTgzODA1Igwz28IByC%2FyHpM9Zrwq3AOB2sRAsTx5A%2F2xyiKzE8mmVx7BVhXPsjjWQHWyB4eJ%2BgkTWsN7MS2mIqoX4sH5rmaMLZoiCfQ%2BBEEFoRU8GCBs1d%2B5CSBwKRtLEA3zRiy75BGvAkW%2F6NxUayOqlPJRCLNa7hWS5ZKuO6xkAuki4a12gbgdho4LQZVRPQnuCCppkWNKRIFIFwoXO%2BlbpVgW4WoxsXZNQMzmXKRUGBf8Tdz4ofQvnyo%2BaGOHKXe3g80OwgcaKLyIU%2FFJnkj5%2FABnkWk3mL39Saj7FlghywkIGATkR1vKHsnkjGeaekYwNOWHUeZS1w9Hm84dFqiGdyXyDRajvRy8GQsIuwk8JrdUnEoAkLF%2BsBIPVh1LFNeHXTtiYh7SI4oflqAZ%2F5WBtZIYdRs%2BumGHefrUpg8zrglYtYEwX0fKDGXzRmtsgUMowvDV2AsBKj%2BjR68kLr%2Bu%2BWZU8O4lTnMq0Y1LpXvngy%2FgmJg9ayD9pUzp%2BwSEsjvJcut4bcdTqThYeP%2FSvaG9CqPbCU61OOru8IuGj7eo57nzKQTzLVreHI2jwn4R6mPbr6I6tPHWzbMSNdPJzUwsuWmBgzPrPUO5OYiQA4YNu0NVytHlF6pXCHqMTJoXq7RGgmEIfo%2FuF6uWjnA%2ByE0UhjDL1%2FLHBjqkAYA2RXZUS5M6ec0AhJKppTHCiMeQReMAWuEG1voAdzDgAG0iEMvXrK5MTVWr7ftmhvxeKVrfs8QPQ%2Fj83DS6g5bgkC9hMVyQXQQJixRcYnymjnvvWQBZgKCCBaFGPW%2BHMpAAQSdxBCAPFkVJ1tRewXYfWN5mpwUIwXWC6Q7URtrHkXbRfbcVDVKrWWEPMJorRPcwgvwWfPbYVh0DpySo9DPse9dG&X-Amz-Signature=ca72325d921bdf92e0d9b980d837bdf3e95d662962eaebb50378179eecca6353&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
