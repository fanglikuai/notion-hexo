---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLPEH66L%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T070106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDprYvx59Vj7b0gaZMq4ENc3IvtO8LZBucHVU4brKhihAiEA9UaN3uvYriIuyBDv4xyQBSqABWOv9Wd8RVSjHwpEfdgq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDArfzUXnrc5WTgUlRSrcA3t0d06EDwuOTCscTRfdeY4D2WkAb5whjOH%2FlXQ%2B8ydtKoF7nmtjmBdlvudVczA5g%2FM%2FnaGMeLjukpaPx6HWy01Sp2yrmk8%2BDIzaatCVpg%2BvvyGtJKE1OoUzqXAnHfX3MsHZlU3kRRT%2BCTierY4%2FNds98BVnvvoImc%2B0hOxgl4ImYbJtWYhqvmUxpyNGbIOIXDG4CKLfeWj7BS%2FN8R2TXcFpD5n4IW6NAEKJoZKPIfLXoX2CTep%2By1QiDoOSXHf2bSIxoOEnZKlA8lJgv6s5xnquLOgAi4rj6vJXtP8pQNKEresUwtTpfdP1PhwcFOah7a7nbhHwA%2FMXjj0dvysYlgnqqMp0XHGWtaK7iZRLtwo%2BU54Dv5obxNJl4AeLcNlODRztrjRALfwWV%2BMtD54rFbQyuK2PvLKzlNuI2ZQNbaUCDfvCJE5t7qaxba%2FQTlvla1xttEISzIwQWzSIEI8IFvuiXLj6o1rCgnTvRJ5wh46DS8FXbDzi7N9rk7q2fYO7pk4FcmYBJTiAPVwBNTv0b7YzE5LG%2FV4Q98PWOWdLM6p3SJC4ouTDo5evMHiHs5mf6tiVNJukwFQiIlXTLOylKR93PtvGKcArEUyiEkFujveEXyeDAi2rlub56Sv3MPvq8ccGOqUBleANX2a%2FHTqWg4Qe9sVRKMLFYisMzyWpnE%2Faf8PLQ6RbhhQxV5VUuV%2FOlNKr%2FI9%2F8wnf4f5kpbw9r%2FdQ0FCBPENBfHEZEk2bAfW5nDkQz4jjM%2B3SO%2B8VlG3YCdziYhzxIOYZg8sio94ACzRWH8uSVngGL%2BE3373uNvOK1Ft7c4lv7OPO6et%2FgH%2BkbqvBXajR6pRpW9k32itrz9TzJ%2FFkoOmf1WLa&X-Amz-Signature=14365c3896c580c794cd82c3640ffaf84a83ccca164c6a56771358ef5b91d645&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

