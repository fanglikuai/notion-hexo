---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666D2JGT7N%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIBvsszpZwN%2FUnOO7GNh6E0kUX4z0lVRcjno6pPntUHeTAiEAxerPwODFDXM%2FwJBH2lxd1rpLlSzNaSEiy7Lz0QTj70IqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGj8hE40QBlA9lEvZSrcA9iBw93jqfjRByiSopqDUX6zrr51ShscgnWRZQB94TSy4NRUcCdkATj1AQoxZAU4SKbVAw3jREYzGc4t7ObGrvQ6gtA84Do3qXwIRqRmOAx44T82mbvkBpy9%2BrCc274L2VriZ%2FKhjGCcqvPW5BrHgaQm%2FsEIThRSoQCsH4CX8hdzV41t%2FW%2BtAl7t70qMS0gMjD1X7FsiPgPlEtefePrUl5f%2FydlAabP7xiQKTvVIafA3ym%2BL1CTnFgzrKEV%2BXk%2BdDgTgsjbIHWIRcs3lCqrSekerVYYjCWhrHP8fHAoG03GRN9c4ovhn%2FBDLkxGqaEbVJtkEW074Fi%2BQgR6NAV6DjUP3LAglJV2WPgGRiGWlAz53XgGo8NOFwCkvPGxsuw%2BukS1IX6gVzcAHHdsOV3ez7dnvbIaVPJJi2GnXsHCqZaIm9NOZVAv6vf0quCGl7miZPcA6rbyrA6OYTUXtT3uu0Anrl9cllM%2FACxQAg32KxGQD%2F4Cd6j7t%2FkjOQzhGhG6KdrR6fsOB6ewE5huNrRis7kHVvznQprT7W3g1c1H1xpqsscNFzAiajxBFQHsP3jF7rYW57T0ZvgSfvTf3hKSYM%2BDaBYSxrQCOLtLiWF7k4WK2zfMqe%2FIAhkmw0TUTMIn2sMYGOqUBbUd%2Bg3OO9X6%2Fa6jEFpqMFeue6XQfOqGjmCzEFghocLWni1gvYnDAeg4%2FOPS1Hqw0A%2FcXGUAlSnIa37NNpbn56xTfqgsAECyg0Tv7USFni8L1%2FDpjzN%2FIMsJYqGc5%2BWt%2BRLJYTfubW9C1HmKv4YvYCsrV9AL1bvUzTlExYeulssRf4r9gkHmIYhPULwKMVzwL%2FOFuZERQZptgbI93jYhed3mljg%2Fk&X-Amz-Signature=dcfbb028ef94d69d03be1eeb9d2d38a2a8a7b0090ecab76fefe1c0d280fdf786&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

