---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TBMIJFL%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC3%2BeN4tr%2FcsaRbvkxaEp3OpBj2TAS%2BxhxJBBJEnddFSAIhAJa4iOi4qZM5HhFigZrmMVCLjJKD26OdTIGT2ELkRHMVKv8DCGgQABoMNjM3NDIzMTgzODA1IgwmqoUWrwckq%2FG5Q%2B0q3AO4pnncWly%2Bl6eLE58cHbgANX0vRooLxS44LAoazgZZWLBTIqYrrQPCfcam1YBaJ%2F4uSmpz4zWIAA4ptE2ydtfJYxcO8uA48FIFwFvcVAMhEmjtSVn93VkK6jqU4cXAiu1FT1wpsIX3GIV4rGLVLutUGDN3LsAASKnaA0isU1iCZuxUywRZ1MgtSbF8Uo5BwpKwMMieQq6T5jJ94SPrMLENiR6RS3XDRs%2BlG0y7sgS4uEF5wH3QH89WmuAn9aM0nVIhBDc4ferSuC%2B23Vap996OPX2pFRP6iAR3cBIPW0Osc4WksV6yeeZnJC4010dYMeOtSHRTgDOljldQM%2Bha6AKI7qtT0YokPNiFHKQJDju7iu1OVdOuYdHnKio02amR9nMsAclthoSX3keOHbPM7ClCwet0zfP1cAekdkJVQI%2FnpDSHgCF9zHKI7MnVANYM64w%2B8bXI2V52YlZzEgSjtdiCor636tROREtqilnDqpfmraGWtIaa3ojx85d1rdkk5cI8YbnLo3GBN0fczGEVmTnjWjfoADsrd7%2BMSxEB7%2FlQIH84nbXG78XUrvqbxo%2Fsg9ZtKTXo95bKjFjwYIzw2yq6JE5MoALCTsb7TtFFxYIRlirh4OS1%2F1WzKcAAUjCf59HGBjqkAQ2j4E00pJAqnP4tzp2%2FaoPMxRy85lY%2FanU2%2BetH0GxxiVV4tUzO%2BMnpyOztBfXbwJeY7ZKgZTMUD4d4oZtik2yg%2BYmhsxlfcyd0vyj7BJg2IKnCym3muZegx044QlMBa89bQyZfveRAAlOFFvB8URF0iBScQ7z61ffhNe8mXrKmmPJ0GivQnDKJK3GacGImsOqbRVpzo1Y402AZiqfq5N3kO1Vn&X-Amz-Signature=2b4f94f36ef6cc6beaccae315ceebf88be8216b493d28a89c5d2f5dd4d3d5524&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
