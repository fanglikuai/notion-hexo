---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHZPIW4A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaVUUV37E878Wlq0NbApguncuH57q7wb3K%2Fj6Gh%2BCXNgIgbOcHQKwBsF1wDAB%2FmBtTkxJGwsWjHe5qasxSTWMVh%2BcqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJb1uJsGsfK0HWYBKircA1a0OR8%2BuQ3K4ig0KhsU9NuKp6gVTViK0WsjdQ%2FT7%2FFp8Abeqbme7BYH0ldRLkGEcz3JPMyUNDWJQ7GpuKGc9REHmqVh1NsGVtUuQefdxLM5vrtOslIJL8xfVK%2B21BYuNjZMKOmkhIsMn6CJTF4rBV8%2FPDFuPyk4xRi4HcL1fWVSZBraYHaPBSMfkcEWsDxoZIS2gFDpjuCis6ZzyMrAdGytAI9OeZLkIIi4jsSqpEmAxLlbTefqnBk%2BOqHRW3xSnITdw5dgIWy%2FVYCeXEHSYrTqPO3gAEE1zJgVENY3V8f3MarZQfCNOVxp5qNnMWxkko8Xls9Uhc7efiMW7A9qWHi7kcivy5BLS%2BFuT%2FAAPcMIzbZH7489E4uy8ZCjft7qu3Km8iS1Mrs5LaxwjMV8TjCv5wMURzn%2BsfMqx7rtIpcXbABOt3ffPduxKD2zy8lxAfvKJEM8JU8g6ohOYCma%2FyJkDKcVWrSM8BkaGldLybph8HubcDSJSCxKJ5xLqsfmuojRHRsvw39sq%2BAwXBqQS2kuI6EBbRHnSgWdUgsqe6FM4uG4DOfnPSOJQkt%2BhRzlJTLQdAQul7OLW2VAbGPHTh9TjQZEi8fwIi8a55cs9c5hIdLUtH0iVIVAmPjFMOuvxMcGOqUBcGowSa44MAuIBQhltJpm%2FH8KEy8U7QPSgsy4rTwodSkMNhmYdc045IQf7jrM1Bm0itxdlVw%2BP%2F%2Ff5h4tmDbKpaGy8Rh4eSHS8ecCj7ogb01u8xWKxCY6%2BJw2AjEKY81i%2Fv78xkGwL8ymhs0SW4RNQ0NFK6Q3LAkRSLv9FVIsBH2jRRFJy2q34uR8ib0eWYX5gv8XMlt8PugD5c26E8GT2l9XpNqv&X-Amz-Signature=2bb615eb01686e6ed21c5c03ecaaa4c72724f1feda04d92b8de5f0d7d361e383&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

