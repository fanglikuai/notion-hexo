---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666OIEGWKO%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEGGtFBXKorbQiXWqdErc6M8OPT52HJALqXAv6iqc%2BdMAiEAvfHzI6kc3DC0fhWSeGXlAQ5r%2BtIxe3szPVmQfRdoEXoq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDOnwl3fk82p8L2xyzCrcA3L1bfiKFSpOXgdqd%2FC0Mw5juoJ%2BAXEsgsCOibf5nTBl%2B%2BOJL83h9v8AmsvRCTRHTTO9cMpi8zixb3leeYbSYDkPTmSsDPhcpvP557Cfm%2FjwGgwbWnEPyZC3vZWfF53PG%2BComuc3QLjh%2FPYGtYyMYZKRwuA5kuAbWX3iG5Qld929FTxUXV1vN5NOyuvoKsPFYW%2FFpcuZhtttlNyDPjwOejP9WNHIkr3ovzmV19HSEKnBxsGhfO6nSs4r80ei1A%2FXEoDAY4LBkvUWa4cC7kHG16lejSoPgzAJOstjUg3UCUwMGtb3k5a7DsXtIBmSdDSNb6FNPtBaJ8UTeLHxiWUprZdD881fDG6xHm2O6X%2FL0t8nf%2B8r3g%2FHM4MQ6h4hf3B6aK31HVOG4L3txAGa2TJfTBnKz%2F3%2BTQk1m0LWke84vHrXjrVWPkM2HExOTPKMgqheSzy7rBog9bfZnXt%2B86qUtaRdXBCYLWdSNSTAnxiL%2BJfTlCkWLs1kpWAEz9s8ryU70ixogr78IjoO%2FmJvMuCDlGmXUvV8Fb5V1%2FK7AMPILQ70KKMaO99YGjCmKDhhOQJT8hHugdBcI5vMQo0nv%2FT0CM%2FD%2FdSQLwYqgdxnLxUiNRoAAgaa4K6gFgvqhDUCMN2Oj8kGOqUB5UGb24vy9GPq0KKQ9RYANBsiJb8E2a3OQvmSCLfn5M9VatLdtGssOsBd8B8XJbYy4TuEbRipG6fl3jlZoeAwGxsDpFbLfCgsyuaI%2FnW6g4A34IFCrnNAQqrS9NiGwIVuLBkvoKwrblIYqyP63xiAGyK8qOQ%2Fx0%2BpI2mSMkjatmd3u8%2FKM1GOfLlXd9ydWsMigOOt%2BHwBTcuTSrFCClt8iVPPB5vu&X-Amz-Signature=e0cb0bf6c4403031e1723ac7702e4dab33246159b226967d28a56ebddc985260&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
