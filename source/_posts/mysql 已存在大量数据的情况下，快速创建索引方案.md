---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCPPJV4U%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDe8WK0N5Vt3G46RJgYQyhtmoiUL6lrcHEwI1GjHeBTdQIgXtP536Q2aykl6ZRQ5mPKVnQa%2F4%2BCjZNT2M8pCoZD5JQqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJAj0lpkT01KnEn8byrcA22eAAs%2BBwAzkx1iP2fVJT52QUFXcgqd2p%2BXCMUCivUHflLyMmu28JZXRejA1wC0E8rzpKqe7Q3gvTWGCEmQMhzUtssCWAPRb49UJYRi8fxc9Pj1iKlS37gm2Lhis7vR0qZ4kpchiN0Rm6QPaDLWbywh55mTskgYi7pMUkzjjetAhai%2BRjXa7bs%2FmZZRusJtPmLfkhDZog5obJd1p8wEpzXZEQcduf6TOzElo%2BsE2L7A1HgJVjjA9GtVb51zWmGZQg%2B5owDZ93Bjceo8bqI59sQGsPNgZzko2P1SI9Hixf%2FCVhGyJw5sbulnsucWc6oWshXg2gMziIDDT1741DHIFZTVy4SIruM6tPQmOspiXrd4B6HrjBgkKrSLaP5zI3MBKkO3qI3lssgv0Gq3MF6TzPxc7t3WIWjP1nQufJu1yTOb1yAFRKmQDjctzmEptTWzHwo6fQIlTNFtUUi590ALN%2BqbbpG68tkUa4SQl36o9ZWEGBISfp5u3gJ53ygm%2FpHsQhX448eDrhh23uWpF2udnwziiXSesDtpEmi18w03ycuiVUs3NiUwoeAISvCE8cs2unuc2XPOMj1IjLh7gfgB0U6tn9hZObH6BLkNmEm26iQT3h%2BViqHQn4NE7hEpMNHv5MYGOqUBjLUxgKOZAVS%2BdorlxUVrQUx46hOZVLH08ZbX1LCoR4KBjYsgzmFwJGoAUMsSLwYIOBlAexvAgg3fCPkUNtOtBj9iZfANc1MYVvSxmGBhlNcfRT2nAQA1r6jB%2FG3Zm18BHubsFCdDQaxqMO2sotUvYUKzYn5%2B4K%2FWMgdUJfOlfc8RXmxe15%2Bdlp2Xw1rIyy4XmIKACkgp3YxXzkdl%2FKGbUbG3voOW&X-Amz-Signature=2c6c9b1e67e32068afa3853d9730dbee023a26cd7c229a6dde107d9516eaf2b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

