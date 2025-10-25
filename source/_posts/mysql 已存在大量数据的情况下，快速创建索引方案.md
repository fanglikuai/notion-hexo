---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWW6V5JM%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T090054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpUOCIDklE9vA3FYSCiDzV8gE%2BGI%2FErRfdBPKMKCGtzgIgFUjOeBXpR8sOS8Lk3%2Beo%2Fkts%2F9Q8aAxb8UwWRbB78J0q%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLHkC56yH63mu2y74CrcA7x3ldH8iD4YmT0da1l1n72fOXFviRQVB5fX3UEnlGTAnfQkS%2FvMc%2BFznklvZ14xo9Yjr7CWr8KYtPu4jToX2Jf8K%2FaNG23R7D7FseK7Q6wONPXaCFt9xOJCGxRqiGzjbc3DfMN2iu7hXn5qHni6lWcVd8cHkJPEU7Dfzuq4SOUUvI2ZnthmGWjUzdqDrH6QigqyyHJVDiwN3%2BUnuhBw%2FfOJCciMvgzSKlyjZb8Qnze%2B5LYb7af7pRJ0qKFOTTUgWI3d5vEaB%2Bdvfas7%2BqRwtNxGL1M87BcMqrIcfofWrbE0XSx%2Bk0vvJMOD3b7%2FE%2BsWvAx0B600p22Co58BVVbixrHfBV%2BKtZYLG5I22xnKqCXFw4M96jTdIh22GnR5K3su3qqjg8kFYXQw5YFHWMaU0%2BVIuqITFdbm2uSFGbODoee84MWuuNdu3WTd1Hs7w1odQEOerMog8SeOgj8owcso2ZHm6sAnlkmqpC86F7PYffftGJT0CWDQIXJSzhqOUXtdfM1ug8N%2F7UXkzdyv7UlpEZiCj4JM9zdFlnMy6oixy4RrsP19koaoIqXsn7yvqjxRo%2FXaawIPBjO2xBg423ZCzTiQvj08qPrRPqffviFAZhr76wANK%2BUuSwqwHmOLMLfq8ccGOqUBwmAmy6NQ1QK3oCtmmgRvrczRjutISnBwqr%2Fpc0zR81VV8YlCk9Ywb9NsLcAYY5PNorkxiKPsHnnue8HTTZboUFyhOWkiHbe4HLdeYiF1ErKSK76%2FNu7VO1Q39nh4pvSHcgjA5LdLqzi4HwZarXfbrsMZi0Fg16oFUcPzW8lCmoKy4CsF5dmA9AEguUwIsr9jkjjwE7wvfRE2pRHKpfn7AZSsxBcB&X-Amz-Signature=18dc02afa19f3adc5c4121a74ff35b4d8d0735f723f953d141e490f83bc159bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

