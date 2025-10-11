---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WOKOIOM%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T120052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIERf%2F2EG6br%2F%2BlnAJ4Nwwe4fA5Esuz%2FrUBFRYvBobN4hAiBDPJ4z0dT6Fs2pLkq9yrK7ErY8kcoxTl%2F7nCz3AE8s5Cr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMPRIeDcd23%2F3hSaApKtwD7tq4H2TyaYIrOOYBtAq7%2BsOLDG45fFOzpJgL3M7lH5DrDwuVf5wdezS0fcHZ0YaB%2Bl9Pdnd31vJWYWeSpQ6aU4aZ%2BGzOIvblVNWBmmwpXlqYIiX2UjDhlDAk9i%2B3QQedqDn3yFDt4mu2fqiHYd9%2BCiO4cOz4Ww3EUjHdTcNXsIVj5WQBlErSozpDp5Jp98UeGX2C2usWM0TQnunrEtgY0cR%2F5klpFsGJ%2BWqKiMKw7iDu%2Fgp9rRUQ91UCKX7%2FcHaSxRjPv%2F%2Bk43dcvisfaqrgAZLhF69P4TKxwnNK4wENjmuYhDGCxCQHaYnLmPje12XY4j0Bn1NvFiFKhxEXh0uJqQvEw3bv2K%2FNrXCyOYvFfcPmqcTeeREdGdTpQc5Lj42AVETHq5LPXjqSnJqLcQI0zXtve92B5Jmo6QqivYHfV2BKT%2FW1ZtF1X8chYh6VP4uPkhG%2BBADaUyVdyGNCz0vrk%2BNXz4u2%2B4yXss%2BwHTw7f84Fg8ccVUC17BKY9BY30dPtkSRNw42MhpLgtIjAt%2FHalnwqvh5ju1Sy9dyg0rgMC%2FXnGcsUMZo7AwMz%2BB3Ok7NotvfkmmLeQOcbqBe5gYg67QEObtstcx2NJVhMOIjQIbbb%2BolUdLpBM4VEkosw2IapxwY6pgHOzXd1gdtZjdvE4MXJLDqj8oKqqjZn8VSdXK8swrCo1CEtHjzWFaFYTM%2BjjKhmFS73rD4kFzww5co1OyDbN6zBtFtJgpGmB0yWcAggj9L%2ByldOrkgl5fxYlzInUFKN8E1plzt%2FbuWwM9pj1hHjboZ8teBVbCzSsLhb2Pb%2BDRwmK9Vs10UhSC639D7Oc6Wktntiw85bVT%2Ba6ZWca3lhWS%2F2Qopxh1ka&X-Amz-Signature=8041ac71c22766355232de02a9f8d0101b636b1001559581ba7aae669f4dae6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

