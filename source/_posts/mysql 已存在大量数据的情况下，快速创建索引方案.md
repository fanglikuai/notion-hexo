---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZLCGJN%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQCCYkNXttw5RWHJ3OBr4TbpGxD8GO5PGsNmswbIdgrQpgIgRmx%2Fr7HMZpzZgGCYZdKPdL0UpwXSopiMCW0gQVIj7xoqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKrKDgCAVMzNGI5DVCrcAwcANL9myXWY2e%2BU7h18SQCrmGVfdgbDFq6LVFE9EINnEb%2B2RT0AAKfrrsPW6X6YkCbqp%2F4MOR9dpoO2%2BTBL%2FmodeK%2BRegMnPu6UWY%2FExAKo0p7HLbVZGUnMNeJ5WrUvrQ1WzmPj4%2F%2BLhpy9yRxAS01S0oNOqou%2B3WAFon3KD4LE6Dzu%2FuupP1ZivbXobFdOgVzAja3YCg1%2F25WO5GmUyEqHE9n67tSaMrXckVNBID91Gyo31mhyDGZi2DPV%2FzCNut2SAoFU4PM07GdbW%2Bo0mh5lhb3YAysMPzcgx%2BJ6ofR9R7lcLwCymMn89Wx3LO26nMDWk5D4EwnV%2BElz5RDjBZe%2FuFGbwNPrGF6r7m22l48gMydyaird2RvGYlk3yPxhbF7SXrYlrC48LuPRH8fmxAnJSHW9KSxAY34ewe0T4WIRVbBWdjWZnnV7jsLmjZFHQahtIVZpcKbiMdEq6uGou%2FF29UXIKwND4KrTBlzloxLyeqYaOjdX%2Fg0VLY6FpWsCcKTTuJXRaD1%2Fq4vXlC7l%2BUyJgIycyfRQrJHmrVVbWs7N1A1dSs2m9MFCoBwT6ZWwHsegjVgsC9D5izDGCCJuouAVuq2bE6vOdxo8fR4ujFYoRQgPPD2J%2FUIFOxd1MO3E7cYGOqUB3rykU%2Br8k%2FkXDRGWj1DRxoHGYeRRD%2FohfR%2FArdbwKsrgi%2BA8ToyP3o5H17xmBMZye9cpynvX3vE%2FEMcTWs5fcPOgtZmgtS2dttYA%2BqCZdNG8dswcjDpT3HlizvCEaxUV42NgwdWQCGWB%2Ff0RPOxmcuCz3ZFCtv7cS15ft%2FmiTVnVEy6IngP6bL9IrE37aFbaMv1GhrZG2lvEQB0TNaPSqYch4Ze%2F&X-Amz-Signature=aa851e57eee2cf97319d6783198119b70c63dff79c58036de28adfad8456a2f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

