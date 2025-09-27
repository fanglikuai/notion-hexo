---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZWDUYYV%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDmsOu4ZireEJP%2FFAwvQmAWJhnTLYS3vVjWrGhhurb1kwIhAOme7wMopVOwJLQAk5r3moILh3km3cH1NeKOhp0XtmJZKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwdFuUq%2F15hYkGs3iAq3AOrQw19xB7E1p2qaW%2FIzC5sYE8HhjkHkn260BwPjY4ZzqbFLBJZZZ41AUsOX9Tny%2FPf3ZsE6iTjVgLPLAKimpauOXh9QoXkoJdwEuNQt0YJ32xCOGXmt7TCaxpF1fHvopcoMsNfVhX3wie9kKln2VkzT1IK%2BWK5FKjLSv5waSQhOJdmc%2BOZ7TO4IZiZNCU8%2FMGgT4JOYlafJFoHwTSCc6HS%2F5OnRO3psbhPfWMSblJPLK%2BiKBr0c%2FgmevANa10%2FdKSZS6KZ0u2%2BwawSMefZOO6A8z80%2BvrEYdkh42WSzlZz1QaAFMPr5QDu7EPODxJOeMQ%2FXBFhfxxju6L6F1GTd8usukp9JZujqSZ96VIry0kuoJ2JlfF1zM6t12LLOisltTItzOimE%2FYLFHXHzmPtZp8BmMZkg3OKvlgW7czMgC4b4P6jWHNQXDNex4koBaSy88n1mDXkNByu2EI8atztFl8Fg5NcTl916DiNIUlW19MIO%2BI4YZ7D0dpJW%2FzMJXyklbO%2B4a2K30p2azBB4Wit0nEBMsmKI937Cfikgeks9FpNxFGcFylTm5Czak%2FPgePkTCYDfQEs7Wazmr6HAugxNyagHHfWZXLLiyN%2BdqgDAQSTwNrKa5Txfn7jOG3y1zCs4t7GBjqkAaueWHz%2FpdLhIAObKdT7swPQcBZcmIgW44WzHy654%2BhEjxAy2t9gvUvB49vWPCu9lHMEiTzGytBy%2FWX3uwNtl4UOXtg%2F5LQe53i4sRn7gPpAnEw23Rde7r5gJ8dnXlnQV8GTon4WkZ%2ByMRvAJ7shik0qDS8ps7BUEG9w1dkdI1fzQPgLi1ZYNd1T%2BbbUzNz9lrm5Eogh1X7I8%2FJTnZtbHaStq8Vs&X-Amz-Signature=1e7ddfc40be690fe574e5bc432597bbbd62d5397e1ebf7f1badc362926381b3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

