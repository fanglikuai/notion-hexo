---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672N2A3UK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIH1DUFimS6mfG%2BSVy2il05tJkjjd5NV6cG7sz0nmZP0MAiAEps7vrtWpJU4oQhUDGmFx9VvINzlkimW9jQYECMbvJSr%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMhtnGCNE6Y4KCGOU2KtwDl7Wsnyx8i25AEgaPiulChX4NyBFFO8ITBP0mWV2BeRmzC%2BmvNMudsQYXnVANJ3nIxGj%2Fg%2FcRVYS7XeiPabjYs3NOo%2BYcuKdkmva9ftsc3pYAtuGCoymytaf2tSPZblVUIftsMVd9934HazBsYty69q2%2BhwwlRV62dl11ONqm4IDRn9XFWUj9ZS5ygfOWEGmZjDMQoPQZUbRON9bw%2BDBBwJw1NQSteJc49%2F9wq5fEROwlc5mzG5W4gDlD45Y8gBEjlalsR66RYQFjOxrVHGmWbw7gRu22egM4wANCHqzY4aMKTZTxWv%2BcAqG32XQKoF6c5NquKnOUQt3Gltv2ZYA%2BIO0z2MVnzARZijUb%2FNOd24G%2FgXVAADVJlEyurJIgNXNKHHsf0hSeJPu6B4mkFttMLB%2FC0h9PsgFPWDn98EGEW0bAbLXQuq%2BWL%2BUO9Z75CRa696gtZgas6ozv1jXVi1HAS6bEFTEY1MqsEtqq%2BKBFtey1FuGBG1hPY1t4Y800HpVJqwb8Vo5493ZmrZEsdoBhcv%2F0XK2f0zBy0BDx6GuBPFFYoBVGndZdueyXmxYwrfc0SgrWar%2BIQz1NxYxmhsLNkKQOSg%2Bon6mXhyXsaxXSeIbXOIERFdoK%2FoSl1ZIwibfHyAY6pgGT3%2FJNx1t369VUFHCJ64LmuaBtXZUQCF4nsLW26wlj3l3o0LO46ILQWoxAf0WAEcS3wD6%2BUWs729RBSce%2FAc0sQJaM%2F0tNJ4%2BZz7wgZfsJElXCHKgQtpAAuV0wyPEiTrjtb968OZcdmlabjumjgOHaJragb2%2FGMKQYR3zlT4p6l5OoaH7MLxXebziPx33v7UmAIk8UdKwBfFMiGiKL1LWBKC3Tlceb&X-Amz-Signature=089dc48912107a8f91d8ff32d3737c13450449578e80ff712e012b9aeec775b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

