---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDO4BY3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T000057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQCTLGaSLpfjJoeTlZKzMoN3Zk0wNn3siIiBo9DT0oT8sQIgebc5OcTAx0A9wZuUSVULWih%2Bjl3v8slq369q8DnU2n8qiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2F0%2Fmgg%2Fh%2F9Y5G3FircA61O60EOOIE72%2BdNH52eN%2F3WKuwdBBvTk%2FPe3hXVAf2Cplzb7y18112hfgruQ%2BrPAC0ahEPC6R%2F%2F5KVcXMIHeu%2FSpqWM7tfqOuN%2FW2NmACLKbO%2BX3LphvJsvh43JK073NN9zEFUr1tusosbzk0LJZB0Ig%2FAMYBrUpG2p2QzvALfv2h%2F3ZO7lL1CNVbbTNJpVQr3eyL2K2zhZH2qGjBYywzuSBuxPkG%2BgWO%2FMsgFhQagCEJl1RlZi%2BmLxTFtQZfGhw9AMbovlOIINcZ%2FxkrwJ0%2BxVrzxNMHW%2Bsh8bvlsnNOgG9DXHu1ZGnNnIeUN%2Bvf5I9UpHZ%2BMPQDE8%2F5rRzU%2Br1Ggc4c3Z%2BinnR3p5ovqoo%2FmTFqKXUb9Qtmeenq5BQKqQgQPfHLpsSIcNYwA4OKvugub%2BYfXSCZ3iXi30kuzsmrirSWXxkvCKFv6rHtd4NuaNpOKWPKoajKWqEPfwkEJIPcyCPqbR5ge6uqQkpEdv0I%2BfrdRJWI55Hzm3Xzuu%2FNm3jzNJGRVs%2FUXYQGlyYfz2n8YopWlbYik4liXytbmkmw7glt7VwztweZhkOeax7vhG8uwmAwyXScfOHtouBKkKOO7QQ%2FMHN%2BJ1QE6w8Ca6z7bnWJdylDweebyvWGEdMKLlm8cGOqUBhsJRQ9it71q%2B5eFYNe6s63sppyVWC8wLksCEsmvDiThs%2F6cbq%2B1IblmOYWLi3AeUaIhww7DqfL%2FJ%2FNcZiAiRCdwlVMNtaDwtcUGMgnLjdTNqGGYZyCts%2B%2B0Uw5ti18Gm6tMVUpQ55FjTJUTF%2FX5LRz2BQKcHYitAdmeMuIHrHLXCQqQziVd%2BIjU1bf3ddL9SCA4vp1brUiugz6N1AKdUl3kVxJk6&X-Amz-Signature=fd9ce0f2780eaf47f792a05b104e280ef55cadd7694cd75cce85700241acea48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

