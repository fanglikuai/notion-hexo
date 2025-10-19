---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB5U35AV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIBH1gmHqrmizge6SoPjvoGPUyIhOVA3Y6AVMUbNf6IEFAiAhgpeS2rNPY4CGs68gT37YcTVo00w66Ul4JpxKGSqqcCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLkrIwIR5OaICxu%2B%2BKtwDLP9VbSI8dZJLWqvF3iK4U8D49fS5kpbwU8s3pt8XNi2IiM3s7m6K1xsxmJ8y84YR6opES9eUoM1%2B%2F1hgz3nF1VXcQk5WC50DhN8Xg2LY2J7TT5h%2F6iOiQ8EvVMK314IGMx%2BIEfrBYqIPA2fGQWZm9zAcYv5XQrXY0hg0Wi%2FVw7gWY%2F3t3p7znTbLVqWuanCPGgngrZRCUUGs3kAILzRmyJCdyaoinhTl0II9JtHz5VcWC%2FG4XRk71xj2TFiJZISryAZVo2%2FayWAprhlFwJ%2B8OBaVOqUhzFv%2FL3OCKVk%2FWJpW9QPRnYRWwWEON27aKeJVJC8RskwomOb8PlOndz%2BX29r%2F5IHpqmeuHmGUKHK3bpjQlGsjGSyTh2uAY1Wonsb082u0DbkZB4ksw3H%2FjAkFe7XIjtpJI2SaHBAj%2FAft8sGo1oB3P6qKEVnZmo7MKpDuH%2BYj3AKe0JUNJxhjM6lHPszthbBfJKJ2OnV3eq7CxuxrA3OvwKrRzzytRa%2F1oGFwyYzT8thY7%2B7jG3eoobNqw6F1KXJp%2BjEl7xHioUFKKIK2dr7cKtLkjeDun71qoMSInOwIE9jIdvQDD5ofxWVdF8svdSfFWdZo6H%2B69QupR8oyHDlbLfSbuXlAt7wwu47SxwY6pgFfC8MwxKIwgGYCP%2BmUUqjWt1P%2FtYZR%2F3pingzSUBJqPo5H5WOzWrQ1th1ObYlWXuENdJ%2BSIA1c2Tu%2BpzQQrVVCRT878NoSiP1t%2Fhjo8gMWJHeSxaTtVmEyPBUo5CDrPqpMC48xhBwNIA7q%2BK5V18TKdwzM7eX1B1XoBBsxTQarBLUE8xmQEcROi%2B91rEgqbToeDT98UWXTnMc8s0z3fsMgBB0slXyO&X-Amz-Signature=c249087a237b8affc7ed37b79f2c473493e8cecf1d73868d508720c45d8dd04e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

