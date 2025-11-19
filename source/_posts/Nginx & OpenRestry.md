---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JOU6ZQ3%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQC%2FdpBjAU%2FB%2BfHSrSz6TCwNWNtS0IK5woEB8FQBo3Q9CgIgeu32%2Bi2wq9hfDrEVKaT%2BAPPt3jbNAX9b2l7Jbv1r%2FoMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG07%2B%2BLoFeGRvCF9dyrcA5XirEX%2BA8cTC%2Fe4fCj1InLP903TOzswBbpaDoaiz3O4aF0JCu4%2FUjPmzhptyMs6xKCJArzJ0bsqgxBtne1qFcjB%2Fcst0%2FzT4C4iBpQfdbYnAd1Hn%2BP5V%2FdhlJ8E8wSKkyZyf7J%2F6Wz%2FER6yw%2FBJdDuw1fL4g4Im3hbIIFbj7%2B4SpWLZLAk6iYqs27zgZVZW2PeMZUkqK%2FvSckYzp4pffYb0%2BQ07UG%2BRlb9I3kWPPnP5k1UeT8o23WGkeEogCpH2S0IE1O45GhaDXsdRfzb9Ve9n0cOQTgu4Y3VCTCK3lIBIZlzqr6N8Pjxow1Z3LdOCSCeXaEly5%2Bv9%2FnNFe4gL%2FkBSTgVnp7mCqIdRxOnFNnD%2BieA3WIT41fWwSHbu6qfkNDC9NSYnkP1%2BGY2R89gQ%2FZXuZ173NtUe1tauJdKSuS%2BQ3RV1ZeahwhVbk%2F4vYRaiZw64xdbLfPy69T9VBjXTlOvb5Cd3li%2ByKtEqi4%2B16ta%2Fsve5EUhRNPxfaATLNcgR312BoareTlZV2m2lLXD4YTpwiP6NN52GwR%2BqwTodRs%2Bl6SlvpLgElSFg9%2F7b58ApNr0sU9WfIx%2FGRrR4mOhy0hcX8KwBbxIoGAkt6ewDM3UfFo59%2FGMW%2B%2FyzvSD7MOb5%2BMgGOqUBBWlmoNyDzmk6pFwYgTVykCOuISueU4U3db8WsUsyWveABNZZxUCgEh8noh58D8EXSvZhzlN%2BXKw%2FWVtn6vV28LVcaojVBHtHS1O%2BzROrWBhW1DGK%2FSaed34CxGZLVitAnzVn4RzuWK0%2FeRkmYFn8Hc12lhz7sFEuNmkIsAxQuQuDBnhL5y%2FzBmlFLKZpsYDLgbwsnOcPm43p%2FlJeiROWbrP%2FKfY1&X-Amz-Signature=de4a848dd909042501d614cb29143c6e5d386ff233c4e5255e2e2e766e0e2407&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
