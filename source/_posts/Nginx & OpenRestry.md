---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5FUQULI%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T090044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCICuD3lEzrcFGOUPsJInNx9KnoH8jpQXysSpy1D1R41iCAiBCtS2l%2BrOHp6BqL%2FiwfHQpI25ykaexTDI1z2CvantDpSr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIMRxjoyV8k0q%2BCYMxgKtwDE5a6%2B1ZOstyX%2BU4rvtWQEwCqKIyMUSl%2FLl4tvMXDzn7jz7jk1Z448OV372sAndHzJwg0vx66ENOJEkj9cNlSm2PnlMnfiIFnSYN5WXBDk0uMIVdjER96UXawrv%2BxWiSl4iibmdUpdF%2BcXMKKfVC5KuElpIh%2F264tUCSAlIgBVHSCvRbhpF7Gsq0c2zU1GxSCI2a%2Bd5Q6VTP9EgROww4GyeKkGiaYfKZgzdF5j%2ByGrZll2SQnYp%2BoqRWJzkVh7QDGgL%2FKHqMCeFHoLecnzS%2Fyl1zZ1Zg3VhLXc5whBStEJSm4pciXqkOJcxpJkUeEmh9Swm%2BXOPbp7UsVPYbbIJBEyu7XSz%2FNrtjF5gKjySVMRfTSpxvvqk5cgoTY3mih8PNFHoMGzIPbcY7HtDd8ZG1xvzfnIJXS0Y7SQASPcrsWhr%2BTh%2BjnoO9Pmf0ywoe4V0FWW5HnptgIhfV%2F%2ByzEfw6Evfdm%2BqyVLjFBkoEOOXZhI5%2BAwENT8G9wB3nEQoNE4Gx4d0PMDG6lFHC4Yr8snzKEAL7U70MAmwUPRWHSXO67QB9JFpAHLpwxodvplEh5GbD3VNXuAkMybvpW67Fq9rG4LPoGRR%2B670f4r5LyfV2cq9BAd4hqz%2BfXBlASwU8w79CWyAY6pgFHSlCq%2BAfg%2F8%2FVYOrmwS%2F9mQSf98sSZ4nZohWTlurYaySyIDKSrQsJs7hRONHoMLRVHMBj6BSJplkjnzJdzGpR%2FHFZLBeKqm52CrzFeUsKw3enfLc9KVkNujDNhgFF59CPMk4ok0lbOlVcBdamT%2B1VKN4L06begJUpxMVLvFdypU8WZP1GTTqFtVdymzA9A2SWQIZ6hMkSdPjQSaH2YvX%2BNItmws9z&X-Amz-Signature=df87a63b81060b67807b5a7c2d2a3451162d180fafb0c0c0de783831da9c0b65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
