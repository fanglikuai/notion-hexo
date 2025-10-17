---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCMDPTA6%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC17y1LfvcEJ91Ek%2FzAKcJ1fSf5xDVoRwMQhoZuglydNgIgSqx4RpzfGrTY89TZtYJUaHiLQqWeWsm4MUJmDDCqkmQqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBV8KXmER%2F2J128nqSrcA%2FenPiCueQoFJ4HPom%2FarDB3NUEqDjeyTLkwF4qK5CIy0a%2Bhsfn2Vmd1szOs2RUm53lJCJVVqJtb5MEKf6aEzVKZyGeYXLPV%2BuIi918DMYhf%2BaKLCQWp4uQe2hwE8xIZrQpap%2FifhluaDsk6SXiFAPODL8cf6xmCDNToItnl5CvifHD8i4nqD8DKJ4u1pu0dbhuk2U0K81zC5RvlnJRtXV%2BRjFFn53yKBX1tkSIBLVnXCAcuuoyuvz7M1xBfZ5w0SO1dBd3RwAXq8FmAHwAzfBwU1IWdHmv%2FhlFKScIhOyjBtVJtMxXDP9IQ3mjQdg3QBgR1rid255AGN%2BPP0tVEYqCEbZGwO2fLA4kOdFpOL1LXO%2FrWfMbXr7atL8BWSvwadLAUM47VmFXBYG%2ByTAP4R4%2BWOIL3Pq3fqp9OmK6qR7RPiEn1zxF%2BZaaGFFSkYFK28rX0vkg9XF%2BbRzl4DfSSz8P%2ByfRWD5daYAPID9G9ED35HC3uucpRx%2FudF1KAWPT49FYaCjeg7F%2BGTZ0ZIgORRdREj8NX3OKasRK%2F5YbbyHcYnBEhgkENzGcXbOJa%2FzBA2WOJTy2LXU%2FG5vRHmZIB5lsfP5AaxOjPqpOeZpkBw6Kxd1K2L7Z%2F83K3t8LhMMa2yccGOqUBH2E86OpfMHXkFj01abGPhIKQulTjP8DSiKLZttFS29M6o4Ts5xeUgP01m3ui334qZIpzLiondMRBPq5ea0maI4xbDS8s1xQ5h73khGoLQMAhv6pdPwQl9GusIHMN9KXB4QciHKTHMVjhcxB%2F79xmzE4KWEM3Njn77%2F72eW9SBIQHOTSWM0QKUMKI22SKhiH0Ot8Su9dhXVa%2FJu%2Fvyqb5WHFRjXxn&X-Amz-Signature=b05936eede291552181f17e5cef71856d49d5832c2c00822b61c50aefa4f6df5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
