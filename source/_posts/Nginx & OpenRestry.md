---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSZ4NVRM%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7MKLQowdzFWKvDEt5v9KrIt3IrbNkzctTRsZ74MWOgAiEA8SMJf4VAbTgbVNIpBOOTjayNLCq%2F4UIv6pJnGgcIsp4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPil8G0nqj7lcqaNbircA4AkEiEfrIJf8IcXogxpda2VxTR74ZA08EJeXNijUtP3zTF3t%2Fu6tlTTUF9Lcf4A7Peg0zkwcnEfXIblHmXYw%2B40eD5Gj6u38lUDnxezyM8bEx0%2Bj7qwY7xRTRuqtvdWrqR%2BK1UA%2FxdgIZeObrCXud1oyYNTuWJCtpkkzGGVCmWg6%2FZcrt78JaTVcQ%2F0FKHmiLkxds0lwfdWBDElA7f2HfYaFdmQUV6O7wPAz2wEQcYGddic99EmEwJLPU5gIjOR%2FSHMiT5LMJLw40aLHWiatOFiIa%2BDg60EkoXK0PHjRsN8InwY8drABqjPWzO7rAu7QFxU5%2FALotlYQ7tXO2OMtN7J84FQ75w2MpW7rdbsMxQqlH4xT2B%2FezU9acrBII%2FsiB9nn3dp6SGp2n3Cyf0q0GZ%2BKr7Smi5B%2F8sV0acgBPYeKn29Ws%2FSNKp5u2HQsal8IfGr5ay1fETxBK5COduD2mk6p7n10ru9KYM2JgnVAh8cf7LXUDpIfZ2Bu5qvNJ2Wy7YH%2FdtaRfQ5o2nk6ah0n0lMDvEz48zP1Go%2FbOciViek6%2FbccmPDiBiD7onS4BDpcqGUOhK3k2pgYDRUsx%2BAbe%2FMHPKIQNobiiFYUSvhVX%2B44PTX6fiKGukR8PxkMNihockGOqUB3iWlpKSxMz2ny5NuuHzpL0V4iDjJvBjjM08NW7MAqnUDdTsAslzD%2FB8OyU%2B1u0DY490KvTaPHeT5rxXdiOCc907PD6Ujr%2FoKHZPhfWJTYH2%2Fbe7xkTRg0Rg4mBobxXR7nJB4gtX5%2F0wWp2wXqh0mjd8et%2BUt7%2FFmsrL17RtDcJOXzYqtD62bRrrSEkR1FWnpdEy8rop%2B2EyQx9QVyXA%2FDSlO70vb&X-Amz-Signature=8f73d51fe8235f2e18980d9390d910386f6ef640d761bb4b6a816b1dd0ef76b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
