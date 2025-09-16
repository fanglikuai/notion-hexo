---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEFOFLRB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T200049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIEqYRuATGrcxBkAyJxqCqcTGaaWD5wll1Pg5DVGXUas3AiEAwjIJgwKvvIixS1%2FyeyR%2FjOlQXw2ydY37kDkwgmwvtEIqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD%2FDy67ostCRY7NDhSrcA1QmjaDfoTimnhf89dsjyibAWe3EAC1V3QGO%2FawmMuO4PZG4G2rM5dXjq8AFa9qzZEkWYGTxyproWeyhT3k2fCBR9XCqt6VgxQ7EOH2fbtJv%2FVB9oU38lF6RCIQa7DukLnIM%2Biu1m4Mmrra6wJs%2FMkHs2rsnhM4g%2FpOuZmI5eWk7%2BMXFcS5KSdo%2F65Klw6gTZ0Q5SPZKBYQhKyXXEbcvEjM5XhQRY1pV6EJCj8npFcfdsSzHJ%2FRZAlA9xTEqtDWansryEKKOjieKWl1%2BjXIacKwLVQUMQa%2FaFz%2F427qVl9do2AdC273tq6zYRofJeE45Ymo36sVFvXW7nkGNeRbmW84TK5LPNVXEq37gKyvYAsn3tnWlC115KZkqA5PudkOGADA%2FqCFcRO0T3VdLADm1mnp7to8Oegz2uvM4Nzky5KOKr1B7Fuzni7yl5X9%2Fq5aASn42HetHZqPwdLI70KhGE5Fn2H%2FIb3McbwKLRrBTWphiruXcenVmqj95ikeCUHW4bcvXk886Ur7cqfkUM3Um58Xs69ei%2Fre6lmbNHPHm20RX%2BeKLgJ3qNpNtt3V6wuV1TcIUAXNsEkPJ3%2FboCr9aDx%2F98%2BAq9pPyxpkBnqMAJHd0kpZWqYRVjBk47O7QMOWCp8YGOqUB2VCk5wn04HzyfEq5WDU0E4TnvbYiY8%2Bn2JW4zO3NzSWLYkq57saDI1WidZJq%2FSQnKSPCK5RKdQBt5p1bEWbFJHkrh5Lkdy2MVYvNBXSozbmcanERK%2BGO1nj4RR1fc0eIO6TMYY7tedD6BtJHNLHnmzhNWDYWYVuPa%2BVX4y%2FLyFFsCvl6RRDaRF0NDemOpL5bN3XqnFtCx0ek0yfblfQL81R71kdK&X-Amz-Signature=33d0f52138c4c5750327e8c97b208b904065ad21a7c38afb4ad51c4b12f35142&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

