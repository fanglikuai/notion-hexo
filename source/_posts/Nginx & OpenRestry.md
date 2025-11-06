---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STO5A26N%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T030051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDreU6rZDcYwh6fO4EbjCALdv6opjr2rB7DRieNItuOHwIhALbYt5pU334QSxWzD%2BUN8D%2FTYHpx0YJ0lbLSQl%2FgKYM5KogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwurSWkoDqcvV5PI98q3ANlaL1PlUvWttnGz6GynX4BmlIOyaG%2FnX0IxMkPsYvfW6w3ht56W7XX4O68FAutcWvB98K1G91hTCGtcCJ8sRxxAqOYtpnVfBDjH9hPwHXspmOA4tjD0fr9Fn8NEKYTy6NDB18aWfsePHQrj91zcM8y3J4Qeoyo45L4Phc0LQg%2FjUf%2FnI4ByE4sqOD9hft8DN9QchNXxQiA4Tty1TEFN4Paa32Q4zwaT43VoYTOJtzxZRNXE7aerd7uyfCJnKImbknGEy%2FQOFfWSsnRSGA06eBqlMs6R8HoRlC%2FA%2BJg%2Bd54VRvmMH0quSyuDlIpwsQuflSfeK%2FnpySSIcGWaMcRYswWLSKXq84W2cXky6rr%2BZDOhgt00PHSzS2b7%2FAWNRkQhwi1S9%2BGUA8p3VK4%2B%2FSNX7rfWYSKEwW8gyu5nQZT6E2fm2GR1x9e83CZiFesawVC1zEfpb7USY442Y%2BhLOUiClU8mCu%2B0V09ynirqC71u6Rzb49oMzzHl3GyNiQEmvTnfP1eEaCqGValttsS80VquEKlqCd72BQ0kqzZASBUne1OBSP2KW1OOytN1VRu%2B1qp4RMRcsOcfGEqt%2B6k%2BzzbJoxXvRaqIWOUZJILdK2LSjwb8cPvJgtAzzKyNPjhtTDN%2F6%2FIBjqkAdIH8eg3fgEZDY%2Fus0HhhdFYX7giRXPuMlouZqyzdkAeHKQh3pfBrm5WfxIFyPkqjcV1VnrH%2B8KCd26Om49QYGkBbgCvT9xW%2BNGVjwVIEo2G2dS%2BI9P%2F3hdxNXEjTudqvi6RucbONHEaLQAx37bksHnjyUeyn56bqzLy0HoXktijMihwR8z6oqaTlnivPQnaJ%2B1QRLIiZzVcNYhDSVREFIUKB5iL&X-Amz-Signature=5473fe741c2410c2f63863d2a8fcc8cc1ccb0610df81edfc5554b4edbf86065f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
