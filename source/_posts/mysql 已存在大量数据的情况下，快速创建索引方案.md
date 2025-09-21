---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXUEZOS%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSuBuAq%2B1y4uoksjUsXT0UHPI00XLExRB13k1Hfa6R%2FQIgZpmmH4hIUIF4SVuGbrV4jEpBeEWmUnlIdKpz1TzC%2FTwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGvbTTYYcwOQGtjodircA4Ps3CgFNJTiTc5O4mDhLD7TwKzJcfdrYf3x6gMKwsLci7dEp9riLWSFd78jAu6AkR8ZsaV9ok2XQt7W5Mo4A5%2Fh3PTlfe9oEpN0eHr7ySl0r4OjVy%2F30g1My9%2Fw7hKSHw1eg3NDx8sU5B21Ih7ihSxq4YjXvihR%2FiIjDSDKb5%2B7WOhNdv0LhZ3INpsNWppkXTXC0WQNiMYcO18lVxrbviUcSaJPXkOLzcZuJNDYOmyFLXgVl2yDvLUU1FNQ4A9HulqbVFhS7NhjrpUUgzW6PpD%2Bpd7%2Ff5uiccf%2F0qJkiKLP6aqd3aB%2BCTa2N03tVF0i6kd0%2BRMFjRYnOQ%2Bd1bJwd2R9n2DF6oPxSIJKVVq7RwcqXcLJyhFfkuTpDvlV%2Fw7iFWMVeTttI8E30bGRqTk%2B%2FMqF2Z5Ahw5lU35luS4Mdw3lwTkKFqrrswF5qngyuF8vS5Wab0mB9YwVCEpn5Pi0oFm1JlJsNRN%2F7%2BFTNUVgPBhjUkpKbUVUryylg4%2FOl%2FpuoZ9zUOiQgt6wQ%2BRVs5Ld0nfLt5S6z4Fm5xS4%2FZ47HAUwbzJP2wxTznjjimJipijYKoRdYouguEPYIZTuUgndckRwJguDpxeeKFrgRFw08BjsP%2BwYGDeEBQ8zhYrKMI3gwcYGOqUBgimtMW1v7eos0SfP8%2BAA6mzW4qvihKWghtEZd4nzkj3WKX%2BW%2B8kXvBJOIixyChH%2BgUoBDObhkzku5G6cn%2BrbLkJByRUUckjwcYiAsPkloGn8ZhFu1HBmojx5qzLZCqwyji%2FFLGAU4tKI5ZJDUf9342rywFg8BYC0mnUnkg8ZJZppWSLVMBQwHFD9OYlqcVP1fwh4ShGqXfQm3r1d5o5dckqqx8wy&X-Amz-Signature=b48734d2e7df32b75529a1ff7f7e1b9cfe09dd2e5913faabea142121d16ff005&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

