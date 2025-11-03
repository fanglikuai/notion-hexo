---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JJVYUJ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXVJiZgZPSrKnZMUpIZqSkfsz8QI7DDAlYEMlYSc%2BF6wIgcZPmJhShoEjr%2FVnLmUQGy0Iknx2aNMQbjuHXcm91C3sq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDJ2oyQwgSQ%2FYnUMpdircA%2Bo%2B3Gccp%2BBPU8Q26FvdmC4T%2BAO6o4jM61sKnD2N07CqbM9ziIDnxRspjCogdxcR1Dtv%2BEZlRzARPKii2iIRZsb8O%2F2z8lwMlN6WprtozkhG212X0CT%2Bok7rkQX2ymYGZvCEJKDavSlCEJAd%2BZJGiqAwkFl%2B6OlDaAOV3QybHVxKzwaOaMPP3Ai7UqBMU63cnYPtcEf7V0L0uu6smw8yeZZVO0uma5k7t2rirU8SXcOKemnampvX8Kvw9S%2FNdAvpz1Rt4d39HXNVk5NzuyOOGQ3dPeQ5rY5TBX1uhRfqme9cMncPzs9kx1X%2BvrOh76vjxVn0QClI%2F%2FG3BHfqnbZQ1tQgJhZVIrn9vy0nkT9P5BFMZWId3Ud%2BxvxX2U%2FKYBJJwyeEM9wKIcVSFm8Ns%2Fy1EMMia2RaGFxsq5RtPRQl49mxfkZ8esBFWVVa9Gnec2YGqOjwFbrFthwTv3rFiNejG7fHIFw4hlLTOaWH9Eegue5U9JpZOlNNgyKNalKC3wM5mLgXzCsyjKBPth7NyXpuiqs9hPJAeslSjDycMQuvSSdzuXE7IwceW3iMEnJtG2Sswb0q9I8NQJDVIN9OegV8fm9d1NNBS4MN6HY7DzPn7rqtT1HakS250xGONQDxMJW7n8gGOqUB5AxnQQqBSeg8DUcYZeaCwSuNjUMWQJn76C7Gs8wItBiPiGx7JJG%2FA2FV%2BidktxG60ylNDLw188e%2FYZ7aais3P72b%2FhoCqSHZFJ5sQ9n1nAsneCjLvQBOC8BceT6YNmKf8WOo4%2BCF2%2B%2BJJzbkdnneEVV1nNo%2FkqFTNDTaUODy2O1uZKzcP%2F6B4ricpvMSsOO0Zey5o6qbe9NedRnYWLdlB3rQC40d&X-Amz-Signature=ed5409319c2e54bfefb9853360672cfb4fda08df4189f8a57d5c67960cac36a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

