---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RNTAT7C%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T090104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCf547U3zbnlXvF1GZhA69wdhzU7qG%2FEtirVcKRPVOINgIhAMrk9yomXXyiblDQUiNfTcVVWtG147vIEfe4nxfHrdS3Kv8DCFkQABoMNjM3NDIzMTgzODA1Igzh6Mq8PYA9UBAXVRMq3APfq6TMkNXR%2BxJDGnBPlrrrW0DijYNf79me7EJZhpt15r5V3OePo4dhlp7KJUMr0Tuzovjia2vRT9P4RGMfjV8czLAXrsCWoLUMqT6xiPMsvj3%2BGSLblqUaAiRO0oIb5lOpW0UO1v8IyGgzou73rZ2HOAFOjuS9SmACCV1xpwiTk212zWRLtxL2cwEsr7eulp6qMwFuFF0GdphFkBjzp9y%2FMvgNL%2Fj8UlptJ5wSMJW1D7z0nSh3WnAbP3VU4Kayj%2FyaVUv6ak1seFpBko050jD8gjmRx4wssLbx89UQvqbF6fvr52Xk9KWCHlidVuN%2BpTsJNGG8%2FiY9CCZ6PHf1Dsd%2Bb7ZxjBnp9MNQCW7Nst%2BBiR0QKDHkvAyZBmoE28epBgRcvUI7EmymcTPLsf01rUcN2NPyZstbFscIVtWZ9h46ZtKn5VdmGWebk59h2XQJzl8LCW6mXvq94guH9jgTAUf22rADgU9bPNC8QEfuAzR%2BbRcyzQVHAa8lJghCt9mPBrsxnh76AGWIiHMM62UDfrlYtvI6nFHoS%2F3vhTiuBZ4jyOkc89WdaBLiS2k41Zwnvv5JItgtUBpJ58R%2BZsa%2FqjZ66C3nqLNcxAZ%2BBV1eYlL6wxueZlS2s09f7wdz8zDfgLjHBjqkAVV6Hzn3wqtuEHQiWABOoRNG9mAYUObOZc%2BVp6QGVcAcpLIJ%2FWZVw0LqXvOG3zV3bPZWRQOLfNH%2FZbsbeYakfE%2B%2BlsFZ0I5SrQ7FNRJwuNn40t%2BhIr1fWc%2FzP0PrXSXwzVKclI%2FwWZ%2FHVba2xKh2oEKuQqJd9Gw54mf0pzTtbn5fQiEw4WH4OWMoCP0SdnrDNdXjCwoG8MW9RilvW8knj1RgvLkc&X-Amz-Signature=1243def7fcb322ccc52a7e491ebd02863a19319d2df8be94c90391d537f819cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

