---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFYQKDKQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T120055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIAR%2Fw0RBLXrwIcH%2F6X%2FAjCDesPa2A6IS5xjCA%2BhalLZfAiBIUlTe91bzwoJuX0TvuGfHi%2FAFMkmTYMLyWf6d9yh97CqIBAjU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjc6E9I04sIXG4MdTKtwDeqaUOQz6na%2BVK%2FjvNH8DgNEt%2BTiwK9nlGwYqcA5usZh%2BWiuN2CwwBJJP31ba06LvAxEowdooaHxPX8gTo%2F99V63FneNsxQiinaRoopI650kV%2FGL2gaEEWvJfU7DOTNZsdYvCLYbi8muKazsjiHjROHQ6IM2Liw%2BzrYkJ%2BvxKMf3Rn9VZX0TgwXXzhGjLfH19zOVLTUqSjjRdQEFBmP1WMe9IgIlVlnaU3tsUckc3gBFfm%2BkvyYdotrJ%2FcQAWiy7F8l4Bxgp9mHaCraodC6Td%2Fzkx8M6IWuZbSHuC8ndygIq7fykr7n2wKwxX5PfwEtAjR3KQA3KRz2N0EGd3Fqqhkz%2BMVIAEKr9kEbJw16j8w%2FQDd7I6IRPRuxNi4A01njDltQ9CmCTR4qRk%2FmOlGHdvphIprzsTw%2BSjrr4Hn3n9wCJqQa6KMI2%2Bvk9hs5ImwsdHmqkU1Dqe5uKZ9M%2FbR3DhlrAzbdvqpRcOZFbrqvpF4xw3gMDZWNv8XCD2rEqbh1axOwAvDPwTnIQAfZtTlYhuK8srM9Mph56m%2FhEu%2BglMkoIgaAbmWvQl5tpxYPuc3iCW3iKRzqmFGgiWsUrW6K1%2F%2BRGYTwam%2BychKxHBsYIzaaoYUzo6EuIMvQ2uJf4w4em0xgY6pgFuDKMQmYrx22buqaULjdYFSl3spUoHaX73JnM8PLLM92oqurmaZf2HApYs55yKt6MCOBmKWYB%2FhghC38d384SPUZAdGhQVy1OeMtv2TtNONxF3qG%2BDxSjxSbCPRZgRk1htfMOSivWNUgJdjmR7ZIBqhpHp28IBblqR6O%2F9WZk94J8%2FsAClgDxw8TSLnWUHYprdfmPF1EX3aspRcqhOUNOKlP3faSW5&X-Amz-Signature=ec987077b46dc87f07272c38fb38387f12382cc8012b553bc7f07bccfbdab10e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

