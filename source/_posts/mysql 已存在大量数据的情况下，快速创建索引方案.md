---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXT7BMT4%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T000050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICpycJcN%2F0ye01Q2uZa0xGq%2FAO7dHm%2BX8Y4spTrPTbn%2BAiEAqY3RqSKP%2Bnm2Ui2oYf4oTWm99LJDxQxVna4gLyzNU2Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDMrxNiw0oAN6QNzu8yrcA9b0EiAR1F%2Bo4QEmQKULKRTJ75NkWiBjN017Y4x16oefDO8BBHSjs4YcWVCVDFG8PsSXHLW5f7%2Fd062MuAal56eRLDq%2F%2FglU4UITAKmaIcR2AqRWNsNcVazgQHMasY2QM5c82bd79LQr9tIVUSDIs5Kv4K6RgWawje41JZpH7Xsn3oxcf6ZVM2DL8viwBXT1BLUgbq1nlDqbOpWISfFyCgeZ%2F9Knr3nZW5isE6dT4wyYzp6SJGjjo1OwJEr4vA5yaAQzuPFXHFh887W1CNDjX3x2WO9WGQR3oVBL2Raj7XbeVkJEEIzdm%2BfqrzA1D9WG0dnaX0vcdVXdU22qseJMzLdVwmrvaXip7fzIWUucoKHY5psZy0wEq4pgAj6FcOxPRqE5k%2BH%2Fo5BfLvn3BXK1Yc3WqvGJqpHrJRKNGKPC7jDsT1QFzq5yMqL1JeRAychwK1gjc97qa8SZQBi7dYj1BuxiS3m2u%2FTpc7dl0T0wKAG%2B3ygUsywUIJXXSS2cmZOyjhSMK5jhevMcklbkZwJJzOEAsVr%2FLYDWsGRi6MNJ3A4jtvOWykOQxjzw9XZ%2Fng4kCBfQTo7Qb%2BDV09wdflzF%2BrudA9V2SKanno6PfdKzkHtj3cVYhGlTzHdUuJthMLXqsMcGOqUBcCD%2FyU36C9Kdv3iUqdyaRL1oXwG%2FHSt2DEu3h4%2FG2xsD%2Bov%2BEmxlnMoexL6uOy2gkTL5hYfROr11yOCTn21iqaUWeh1ZBtHOiU92NLSauckGua1Ix9E8G86HwFAR1UBxTDTpDZ8BQDpUswVsC9UnUBL8a9FNoNku7L9LCfWBzTkpkQbAtMi7uUzs5ymKmUReGuV0lzv8QAR5yD9fadnAuQwF%2FJSF&X-Amz-Signature=cc13c1fcb33a9fcbde24524c189e299a56441b5d9932be75b0e154f6b20b63d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

