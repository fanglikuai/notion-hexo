---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFIOGEYO%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCPDhD5PraYDJj8hZh4QQpnsyzxW6vOjSX%2FjTTYAkpiIgIhAK%2FK0iQfPu42s6%2FcnHrraSk%2F44J6z6bJXJfmCvtejZA4KogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyCH%2F%2B8vWQCWMaTeKsq3AMQR3gldFWdK9WmdVN7VD6x939wdFiWtkWgSCmVcSkweBtVQnycAG09LclvV3A2XerKCxgxRVSaOuExOgh2Dx569k6L10XyYEgMhnfkwMq5V73PV4kR09sdU5LnUjn%2BYe%2BGTZw615fxrZrT%2FxwOxrgXzbCnmAK%2BI2MDE%2FNlah0nQ4GQ0qNNoWqvf2qisDDUPMZA5pg61VhQOC18eOxJDyoIXE6nSruHIHYrBWzX1w9P9sYBFqMkD9fWc%2BgdZb6HZ7bt9O6v0KvOqrtyQ5pfHUVqg1ixsPrNKgtVE%2FwAodzZdaEXTr%2BkJayywF9zQipy15s%2FwdCv6Xz1sZr1TZh0IZmIK5GxRDch2dEuqJiQN2CpRRdnEckx6V0SSNGSOWYiAJMq%2FqeRcq7BEGk9Yr%2FSSZsJgaYonGGSozBicJ1Roi6Dq4aT%2FHqLta9Y1o9VlOALhXDhNQLUYPy0OvJETdajNKQULygFZSAgHMlFA4RmBiaW58n4CPq2GUvM%2BzMObfg9LA6Zmi3%2FnjQhz6Z2wsCBecz7MSX%2FbRSxuOUzas5bDdrCSh740FX%2BQA%2B9iPl14b6KR7%2B5xCtQWD8%2F2DojmbqWkGbDzZdS6fdwi9Mi8jNsoeP%2BQxaa%2FSvNAPHBUfSrDjDRmuLGBjqkAQ0Lid2xyU9IP%2FxOzRTS3qsBx43mJo7PNk2ocuj7gBCyP%2BWIigvOTfQPDm7aKgDpIE%2FJ2XEUXLIWtRMKl61OtJKJXHz3MrrlBQKqP8WlzL0yibDe143gtbshrF9EOthyE11LVAAM178QbPztiHupOL8isKbrMOXnuteflPpgdUtmtKxCbvf3rVxk%2FTUk6sgelYMRLuzGBNYybUuEpnACjtAPi1P%2F&X-Amz-Signature=cc8b935fa55a3b892fad1d44c29ced508dea6df1a9df03e4968a114785f2ef95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

