---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZH2XGHA%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T090140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCICu7se%2FqKEZGxrZDNnN2RFSEbVkNdlKgGxHh%2BvNcaybRAiBEorfZqLKYdLUdpzRAp7Ac%2BqXNacA9NjhQU2ZEjmngniqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMljaLBpB%2Fq88kseVSKtwDNct9FZxQZWS%2FAC%2BoWeLQTXGy9cJSmxTQE6AOm3rv4m2jc2l22EW0rDCmoRLx6%2B8xDsSqNOtMyVzaJaWrQCVAuXzr1aPvqQd1GV5wmVPHDyknNCoBhfjye%2B%2Fwf3%2FOzopw5cFmk5pdw0wnIZYo%2Fb5ZEMVGGTa4DvlejNs4RSlp9Fv5J2bxkSK2%2FCZdj2HvIjJOQFnp1zEQl2rIpk25quUbHpCjx9VYqwAJicBc1sthDMopT%2FUt%2FkCU5nsKivwIfmdszM8np6el6OmBKuZqNlPfs3P06xnHZlqSNe5QDvDf4hBdRIBZDlk3XQZqLUkBlI4ofMHkGfhszFUUY8ZgD2oLzeGUjmH%2FAsLbHKZFu6tR9H5xxO8U03G%2B6WWGC%2BoNeGNjoZFWURvwBPx82JYDMlaNPZuPGiouFKrc1BNuTYbX%2Bygy5MeFZ7BCDcnDr%2B5lUSAIWt1uk2goaEsQjrce%2F2jGi4JCFnPoxhmHDK7nRhtIJ5640kOvAtt6Q32Ip8%2FgZIk8F%2FkXv%2FTGKbYr5CZpXyoemsdidtaWvI3fykVI8mAlnT2n0orbAknQpmG28kUjeemobBtIlEPC9FFqjqQesFp%2FEwlwH9AWfKHX0Kk76pKBtqxaHWFyBCcVqfl%2B%2Bakwk%2BKnxwY6pgHOUe1g5Osm3%2FfQvWNRCnYyksUDBeZvBCzFW1V2BoVhzBUW8K%2BgX89ZDecV3RJRR6i6zhdA%2F4QLFuYCXAHTGnCoqGBUUHX8tsGsNn9TRZhwb2%2Bj6WXlkN7wtz0nxNq%2BgvyJmAeGww0qpC6HrrGonXSwxWw71qTyfW7uutcplzB7b5Dh4UVlXSP1yalWRpijZF667W8ZebAjADwUhPrJhzvVMbVYHQqE&X-Amz-Signature=760c15b235ffdfc8446df5c2eb3aa41ec1a400c45dc343a47ae76f9e07fe5f7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

