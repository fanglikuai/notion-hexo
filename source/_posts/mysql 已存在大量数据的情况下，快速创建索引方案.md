---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHDWAQDP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIEpgg0KmTVrHUn49wCGDi4ckxgZ0Qpmd1XHvLrJDzrmKAiBLqKdj%2F3gLwyY6aFdd%2BQxqNqrx3kkwmU8bMrhwW4fp5yr%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIMNtt%2FaSBDwn45Zks9KtwDqvjLGzYYuiMtrbGsQRYMJtpkbmNHrpttM7woOX%2FcugVrp5FzGKMwQdn9KFnSY%2FNXSfIUZr25BH9JkQ3IT%2BJqZ4JlUUapNBc%2FNvldR9WU6cyYXBv4vzOipKRv8YoWfDL8cbj45s7dbDu2CgA9PDUBRZQ70TI%2BFJk7RI9%2FxaA0GNoXwk%2Fb6eHUu2NQbNnJ5JjYBPAYjmfUZYraNUmgT6t28OpG%2BIysZFzVObOz5qtY1xUVDmZKoSsVfVvLJ74G5Cha1L%2BCbjHf%2BdLtUL2xcr5zor27W1r%2BoZEZV%2BLS%2B5yE%2F2BNiquVNyMYBRr%2FfSCqfgp%2BQ5Gscfp7g3KeBf0Jif4UKlAhgB79u9cQngeymnf76JWc0t7XE0jSKEGI5%2FCUiaRAukYwlQYgln59PCHZalMGqZTDu47Gqo4FPQBvx4%2F3eHs4jH4HrFPahKhcTq8JyIM4zSl%2FHjUGlVFL0MEOjk6NEwsz0UdfbCn0eR4i201C7opcVor3yoa02P78E%2BjkHACjqM4SJcJGvMcVfZYljZQchgpxgItQBnWvnqlMZU3VE9gP%2BpKlUiKQYSB%2FFAG%2BLoqSKvgIi%2BN%2BS8bL7Jiw15NtiNyFEkXX2AIKMGUaoIJa4DWn3oKRoafUdiNNxm4w69nUyAY6pgEL%2B9R5m3XImowJIydFRoT%2BmRSrNX0XhhKY6k5n5bFSRWoBjHI%2BtUiwdvNNm4gFEOs%2BgUAJp3JR4GfA3zXi67OOk2%2Fm5UGAjreHyZJbE2wSHLu75CKyGQfyURXBPnJ00WTt7TbdpkE0mgAnqMsVeLgOxtyiIok4r%2FvungAIQa8GP%2FTFpKZadfl9xtxuInJ3L7XSEUReXpabcTNWrDRrQ8lEmzPkKF16&X-Amz-Signature=cfab9a71b8a1bfea4f40415f51bf4f84a57f7083c486cc2b611d149c89bc5f90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

