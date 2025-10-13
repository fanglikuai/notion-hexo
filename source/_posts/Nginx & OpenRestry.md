---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXT7BMT4%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T000050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICpycJcN%2F0ye01Q2uZa0xGq%2FAO7dHm%2BX8Y4spTrPTbn%2BAiEAqY3RqSKP%2Bnm2Ui2oYf4oTWm99LJDxQxVna4gLyzNU2Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDMrxNiw0oAN6QNzu8yrcA9b0EiAR1F%2Bo4QEmQKULKRTJ75NkWiBjN017Y4x16oefDO8BBHSjs4YcWVCVDFG8PsSXHLW5f7%2Fd062MuAal56eRLDq%2F%2FglU4UITAKmaIcR2AqRWNsNcVazgQHMasY2QM5c82bd79LQr9tIVUSDIs5Kv4K6RgWawje41JZpH7Xsn3oxcf6ZVM2DL8viwBXT1BLUgbq1nlDqbOpWISfFyCgeZ%2F9Knr3nZW5isE6dT4wyYzp6SJGjjo1OwJEr4vA5yaAQzuPFXHFh887W1CNDjX3x2WO9WGQR3oVBL2Raj7XbeVkJEEIzdm%2BfqrzA1D9WG0dnaX0vcdVXdU22qseJMzLdVwmrvaXip7fzIWUucoKHY5psZy0wEq4pgAj6FcOxPRqE5k%2BH%2Fo5BfLvn3BXK1Yc3WqvGJqpHrJRKNGKPC7jDsT1QFzq5yMqL1JeRAychwK1gjc97qa8SZQBi7dYj1BuxiS3m2u%2FTpc7dl0T0wKAG%2B3ygUsywUIJXXSS2cmZOyjhSMK5jhevMcklbkZwJJzOEAsVr%2FLYDWsGRi6MNJ3A4jtvOWykOQxjzw9XZ%2Fng4kCBfQTo7Qb%2BDV09wdflzF%2BrudA9V2SKanno6PfdKzkHtj3cVYhGlTzHdUuJthMLXqsMcGOqUBcCD%2FyU36C9Kdv3iUqdyaRL1oXwG%2FHSt2DEu3h4%2FG2xsD%2Bov%2BEmxlnMoexL6uOy2gkTL5hYfROr11yOCTn21iqaUWeh1ZBtHOiU92NLSauckGua1Ix9E8G86HwFAR1UBxTDTpDZ8BQDpUswVsC9UnUBL8a9FNoNku7L9LCfWBzTkpkQbAtMi7uUzs5ymKmUReGuV0lzv8QAR5yD9fadnAuQwF%2FJSF&X-Amz-Signature=7e69448774b860f16cbeca85199414f32ec4a51d79a827fc202fabc4efccafc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
