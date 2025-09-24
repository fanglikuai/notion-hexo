---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TM2EOCV3%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXrBBN19h%2Fg1U6SNpOA%2B0%2F6i1VZAzrbdZ2fYvhq1ahngIgD8w0Wlll9YQijW%2BURp1S4eV6lILKU7Bg4UhhdZfV770q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDFRkm0CIRvXN0OhIvircA6hyNn92wizzhK4WfpzNo4ax5UeIuUtI15ZXEOZIRWc4%2BUgO%2FNWZI%2BBGvDMWgqIUFgHDxg2SPGLekklovoDO2ABKR0uPscapulPGvCoQtEEL0nodD0nvJESRwD8FSW%2BiB1WNBt%2FPz0klcaEOfcrsOBnULrYnwiJ2cra1LZ4TnIy%2FS3u9PTCTCFud%2FyfeQVsXgcnOIM0AMyVs1WC3GHFgEQnKOCk59xZzOc5MuSnjd1e2xU9QVApKcr9j3dmTaNDGkSPZSWr9NwyZZR3BxlVjtZhwLt8OISOBi2ui0bCYQKXlNtvmcVyVdy1JiqVSs0%2Fr3B1F6zfkRFp%2FhFewos1kOJR4%2F0gNZCFETcl%2BYELataxBtRfw2DxmD7uCVv2EUpYIuSQKYT8%2Fzf8bzKeF1aadhJElvdOAh%2FOYLiZhUuXItUPmVBJuhso%2BUYdSHDJgTyBGRJR0nw4RxlM9ffRY93B6EWIFeIFDLO0hVQhq4JtfdymJDxYcv1IhO0eyD70%2B%2B7mPZwKl4IfWnUgW3Dg40uWYdgrnpBgIuCVb3%2BypCJQA82%2FTQWFK9onMoJ2kp%2Figx%2FOQPeyMLtWTOBc%2BOfjZaUfwu2ynA5NYa9gN1GoTcFx%2FwcnoouGdYKwTWcijYm7GMLyzz8YGOqUBRJfeJedOXpHp1puwIYAzRmZWETl5DrdCn%2BXbpkDgVqkB7nQeL%2F2uzVnK%2FEgPUzo%2FFoguKbZ%2B0cUpr4VplCNHcxC%2BVd9ogzyKiTwXj70zsaQZVV4AVC%2FAVaADhzPbZ7lZbuOAJEe7xf3eSRd%2F7dQ7FWOfT1cu2jUvnIsY70Xgn85neUXojrCE%2BOrdwaCOT3OapmyyfO1RwlRYtpWqeKMDvMPOLRDD&X-Amz-Signature=aa770733845327f8cbf518c60d545213529c8346a36be30d63e3cddad0c93e4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
