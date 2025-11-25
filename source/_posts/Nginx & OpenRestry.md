---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SZLWJDQ%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBZZas9VgP%2Bkdcn2kg7vpUYNGPxZMuU8oOcXP%2FbQESI7AiEA%2BaE3RSN6QcHWmapQh1AKJ3TM69Vth7WhSR4DYnRLD3oq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDLQalkTrocyCpon%2B2CrcA9WeT9ZBt4gJS%2FPVby3hTXdxGUgCOYedhdmprdeIIHCzvBb2UBLWcHYvhEzkg8m%2F%2Bjci569sCFYXm%2BUK9GPU4fL3NaJq%2Fz2fb33Ed0l%2BW%2BHDryAIKv8pEQAdLcMWzaGRnLvra0VsEf%2BF73t%2FA%2FHH3Zr5Ke8e2n6elzgBQZYAj4Yzvum%2B5tcu%2Fu8HaxUd5uH6d6Z1Eztt73J3SwTGyVLbkHA5LYoV3thslOZBqVDBV%2BwaI2xhgGZDahUFFbnXtsbBrqAI88GbusZqsNNwo87Xtz5ZdhmKWfsHLCZYRV3e1Sv2UCkdxxHidunlJzQ5G1TWjMD2QCxSjUE%2FAZTNKZRttmXM3rBciHroYr6wRjxFOTjFDbWl7qEIM1VGDAw80Pht6PLx0wZE2%2FynwhqrIFjr7g7iOptVVMwdnUADoy3sosDNE3kGcGF3t8K2ht7cYISIoReslfeD9xS6%2B17b2uV1oCwRddkn200tgyDrhP303szComad9yZakzPbaSla1rxzMXzj3YwiMhP4iGOZmiKMNjMrxsHPtdGWqeIMwlWdkZ1f9zgbGCyIwrx9wLYM7D%2FlSx6MsEMCZOfe3uucpmWN9UgN1ufAM%2FCk7Qs8CIAC9TXKhdoWKETpONp7Ab7mMMmElskGOqUBJewfJ3GwfvBBeIdyiGnZdlFCges14x7muDcvl3uu3l3a2WLoSB%2Ft%2FjCPAkRVrT8hvIIhZ0x1%2Bi6IA15KL%2BrDkDZfuNoPJBpaA7XcSc3%2B%2FlITmy05ujasa3hAP3%2F8d8CTh8fFDcjQTeXKGl5VGhz5KsQY40WyCKgH1kWiM2SF2V4%2BE2Yr7cGfN6ENgJdzA3YPNqg3M5rXSSWzJDAghHO1ay7sWkGq&X-Amz-Signature=16bc1ff501dff5424bd8d01a9fe6e01002185fa62670745c59284f57bfae1818&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
