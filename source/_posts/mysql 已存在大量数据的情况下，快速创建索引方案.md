---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7UTSFHL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T130108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH0hw0MGMgA2a7q7%2FQlt3%2BgEPvSE66WfvzkVPEm7oNYfAiBDxWDnniASR8R7KqWXeL7c%2FCI4Zq1dMN7lE7A%2FLHQBaCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0RjBdXM2j7OqfXV5KtwDGh8pZMXZC720APlhMtoW%2Bf5N%2FBoHEfEv6iwujV%2Btp8JHhbf1qDz0n95UiXZ7iVVNjuaQ4yyJTv0NtyXcrnqivuoVy3EWowRj1Nb5x%2FWRd64k8Rj9Lh0s%2FT6R3%2Fan1kOB%2F0tmaBnugzRYaGelaYzg4BgZpTt2%2FSiEOZOaU68bKtL9axtAOyjeCqfw%2BnTK2bmIdCnJ3GOu0prqyxKF64iAf0IUgGQbL47NGIdKztN8XFt%2FZlKFX8BTUQAARFL%2B9UULgTLr5mLMaPEUAAa1MfG2f0TUOVHQeqgVnXEoD1E9NuftwsExFyGk%2BLx7c9UYvvyILI2%2FImXEPa3izmqOY%2FhkGMzzApz6Xt%2FGcQkRwXpTMhdnXY3%2B0bZHbr2RKSIPG471uPcRr9BBWIq%2BUdoh2YnJjBYLWl849DJNNHfe5x1MZNDUufJmG38%2FijrVmhCoj%2FW%2BDuUleeh%2BjQ%2Fu6ASH9Ns%2Baa1aLAzesJ4DXMONEBuD9btZyYHy4dG5KpDMW9hQ8lGfA7fxEahoGmgwjCFb%2Bq1R6ZHEqJT1fIOPuhfmJu3ygoBmwQ1e16tsYzudVYX%2FEX3k3SCT0jA%2BZqP2bQo9Q1%2BAvBpC3nba3SegpQ%2BHfG7OcAYXtRhLt3jkEILQ4lAw0tilyQY6pgEHQjXRqKNeqL6w2ULhYJcDKQYrdwTwAYaqKrRZUS2IaBOwY8Y5yFZ94CHiTrIAny4elItvFEYo%2B74v%2BzEmc0THfvWZNfurUiXP8nB6miYpdgELgmGPtuGpyjcR8tXo9XDHF1ruN2WxU8OKkMES2P%2FqkR9jhKVZiWfBay%2BklLyNZKs1IzCVjuuyuxQCCnB9EcEjQ%2FjwFoyNA5CyXxtauteE2r5bCGue&X-Amz-Signature=011d66a2e5f6f40254a414d21639f64dcf83e25fa8db17d38ea52f1a56120821&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

