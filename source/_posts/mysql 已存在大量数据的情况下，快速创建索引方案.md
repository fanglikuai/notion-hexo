---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFO2B5FJ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQCNCH4KBfgYDoHuoVnL164xxk6Fn9fJMSFA3rPd7wqIEQIhAMRQwBJUXw4367Crd8vzn8pcScOMXsr4BiFvqBainj2SKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwsOGqC6nOHBcvgZlwq3AN3gJ0it9FcEOOz6Y4LBCdqWcScD5J%2Fnhoxpbu8Iz3BrIO8NLEmXatpOOmPgbXscXKK3dQvR1W%2FN%2FAZvBziXgnBjwBppb2%2Bvk9MTPyYkfVjqlm78JkkVjyb338wML7xkamcKQ9Q0nkgI%2ByMkW1I5HG%2BqJL%2Bk3TXCqu7bOaATZ%2FiRCgUYwNI4o7F339mRGGLrlJAogifGq8L38zcLabgx5qKWnouyMdbb%2Fn0d97JtvK0yevu74JTYqe1Ejsw5YII1Q%2BWwp97kfNkLm%2Ft82eA2C3KUirV186QgREb7bZbPtx7LFwmBaJCmpharPDbfQBKYMDjwziyVWr09nZBy0%2FaE%2FTIddbFZ%2BXtko4O7MqP51CRMbhUUCkImPToTYOzuc05h3zXc%2B5KwSwdehEoZUnNOEtP9jJ0fL5N%2F3S6%2FtWHJe1lFpdEwQmH7cyXeGX8xU4eHft9lTePhpbiFXysfgT7SFVAaVQ89qNd1hJCduM%2FW3cH4yF9BQTZF7C6C%2BxyNyRcOe7emBF5qgArTrwvxqgd9q8sJ5TXy7%2BAi6Ww2nibs2UNa5eomKHLNEYyxsEBhtG%2Bqyn3hHVSbsafOVjLs9q9WW2SISnzT3rs9figQVsn7TxYXph1CLb1FU%2BvCXA%2BMDC9n6bHBjqkAdfE6N%2BK2zT75ebaEaQJrx7rC%2FWSspFnx15cA0SJQlfJ0QtRfC3EYb7nHh3MPnhfZ%2BKK2qw5SjyvPi2ptlrL8xGQeE8tYFDbb56zwEhTyfuVgIKcAcT%2B2n%2B%2BWhehs8y3yP2Hn4eVuFb8aNxSRoCKjxrHfDiwqksgMebgh5rP2s0hqv4QbE9gvOs4KA4rUH9ctJ32ilhgN4222hcCGXXF8%2FSHcWP%2B&X-Amz-Signature=3c537b2cf23cbe590dc46365bc276577fcf728158743e9c324f1a8b8217b1b7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

