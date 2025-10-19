---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB5U35AV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIBH1gmHqrmizge6SoPjvoGPUyIhOVA3Y6AVMUbNf6IEFAiAhgpeS2rNPY4CGs68gT37YcTVo00w66Ul4JpxKGSqqcCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLkrIwIR5OaICxu%2B%2BKtwDLP9VbSI8dZJLWqvF3iK4U8D49fS5kpbwU8s3pt8XNi2IiM3s7m6K1xsxmJ8y84YR6opES9eUoM1%2B%2F1hgz3nF1VXcQk5WC50DhN8Xg2LY2J7TT5h%2F6iOiQ8EvVMK314IGMx%2BIEfrBYqIPA2fGQWZm9zAcYv5XQrXY0hg0Wi%2FVw7gWY%2F3t3p7znTbLVqWuanCPGgngrZRCUUGs3kAILzRmyJCdyaoinhTl0II9JtHz5VcWC%2FG4XRk71xj2TFiJZISryAZVo2%2FayWAprhlFwJ%2B8OBaVOqUhzFv%2FL3OCKVk%2FWJpW9QPRnYRWwWEON27aKeJVJC8RskwomOb8PlOndz%2BX29r%2F5IHpqmeuHmGUKHK3bpjQlGsjGSyTh2uAY1Wonsb082u0DbkZB4ksw3H%2FjAkFe7XIjtpJI2SaHBAj%2FAft8sGo1oB3P6qKEVnZmo7MKpDuH%2BYj3AKe0JUNJxhjM6lHPszthbBfJKJ2OnV3eq7CxuxrA3OvwKrRzzytRa%2F1oGFwyYzT8thY7%2B7jG3eoobNqw6F1KXJp%2BjEl7xHioUFKKIK2dr7cKtLkjeDun71qoMSInOwIE9jIdvQDD5ofxWVdF8svdSfFWdZo6H%2B69QupR8oyHDlbLfSbuXlAt7wwu47SxwY6pgFfC8MwxKIwgGYCP%2BmUUqjWt1P%2FtYZR%2F3pingzSUBJqPo5H5WOzWrQ1th1ObYlWXuENdJ%2BSIA1c2Tu%2BpzQQrVVCRT878NoSiP1t%2Fhjo8gMWJHeSxaTtVmEyPBUo5CDrPqpMC48xhBwNIA7q%2BK5V18TKdwzM7eX1B1XoBBsxTQarBLUE8xmQEcROi%2B91rEgqbToeDT98UWXTnMc8s0z3fsMgBB0slXyO&X-Amz-Signature=b3ae74d80775dad71972274655bb2acfe37ece5bf8816c3ad7a0c6c7346ba220&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

