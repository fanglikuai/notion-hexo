---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI2PWPH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCICw3OpBZsXPhmUsyLsXODr2F9bNxTIMVwvxLpYHbpT5sAiBvP23lcOsFvQ2TV3A%2BiR6UnsDDOR4RfjQYxz%2F%2BxofKqSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7jWrb8yVL2C4z%2BL5KtwDPgN4Y2PM1xCRJVNbN8t0BvoHSCxVmT%2FEQUWf%2FDPRPI1xSzez2rhf59%2Bfhs3sUIb2o58KWUhfOJuxVmfk06sn5qQ56z9UhneApdpVlaqawIbLW1skOmNqo99TrhyYeUCnkKHAqYBDfHfxpiqg5jVhSzKi5PCVDTYyFYlAX6TKvdyGgMUGCtVe2krFmr9661TPjY8Q%2BhuBV0aFqz5YwdrjYq0Vl1hsPtaPF7TwHtPUM%2BFxenN6h5fRVRJYh3L%2FSLMqiejTIoHLpHEmGy7radmlaeEqkmuTLKZ42%2FOaOmlO3RwOyO8vC6pGgdf85cnMwuOS3m4pvy%2F1LRfKDpkGOnEsCAoJHNDnZfZ4lWzIcyXHMJlRVyUMVPi6mdXPUTQki%2BStDPiHDwV%2BhlQcGKhWQWnL7M8Xl390WT6ZKwkOyHlBlpZ2%2B%2Bf133b%2BDgd3bQrl8YhH2p%2Bf5bGEY5KXyeFo1Dk61TvF1ycgTsIaRcdKk2H1WHpuKTN0ZQshdKkz7Lr1rSFYiTTD29H4bYJ4lT5O0P77tjWLsO3I3nzac11kH9XvUBFn5YMd9sgHE6z%2FQ%2F2jz3yTYaYPZviM2Qn%2FBAqb5e%2B%2FHWo3JOtRTmOgmgK%2BGVcbrFM%2BuTrUoTtP%2Fg7iSmgwu6SgxwY6pgFtolR3LqGCwm4FdVwkll6XpqzyDDeq9ppuLZuKb4TQo%2Fi5Efr4ngaGBdtzqdECRLg8ItdgOutf4ofdecDJwSe2o2ZMHj4Uw8x4Db53rsu5zuD97ReOKNS6eDpde7CyHTzNllxjrdRq%2FlD%2B%2BJwL4IAz2eRjcwGV%2Fl0QV5NgBqVNX76m2Ay46mnUIprXqjDblNJod6tAH5ZQe5DgvqaRj7%2BLE9t47tAd&X-Amz-Signature=fee50aa9a027824047efb2d946f3f5ad6d57de0a71bbeb24b3b80e5f776ed851&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

