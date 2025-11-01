---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664OQOGEE%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQDEkghuyXG9QlHgyUGl4KPdZqDsCwC8YrfydbU5xtAlhgIgZl2hHrrKhXyR3LtiGmcD8D%2BSMnDYUoqHMkYb4neRdScq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDItgywClHzsh051LNyrcA0Lm1mDkaIn7BF6TjcKS8k8zofgkEqYsmETdM%2FoxI9By1FkBNbJGEjAt43lB6OgbArB5yiXg6847KtbN7Cn9b6iGQ5pdwRlBqFgxZ9pFdiic7Ab1E72%2Fxuch8uOQBeUS7ZOmqjBrrUzgU0NP5B6D6VEJZQTa6qdUUMZDJ%2Bp9mHxFjLl10A93GHZ18ViVs98FII9v92uzHIlmq9LlAtZlCBch4ixUvSqcxAVivzQbnwBWcwfI1TlMyo8fpNkVzfAlPYpJYjDWoZ5Gfffv6HNpXao%2FYEW6sG2qOvuV06T4JoYKDK9u3HNVU8Ejbs1zMILR9ayhee3ApFa2FXMwxkUDxHfmKQiL33bA7utEZaAW459Zx45Mi8L6HKDsMrqMfhtpKeiy8Me5rcENBurZ8NH4i1Td8vlHc5rk3bfwS4BxNBbF9snJvKjpvYTk4BmPE2YMX7axlIxfig8ajN%2BDTT%2FLdXtO3HIy%2F8wSVnMw4ftWdS76Pcd6yqlKQK7BC2pKHwPjlB2sSAuBWBfvJtpl%2FNk6WzcpsxjJ1i3ytKj75XLN7yxUSTDzR8eXox%2FPMkeSpeSJ877Azlill46itZbGv1Nu3N%2FeAWNWCUR4NbwLMxQj1aYYvR5rTZ2n9cQVWj3kMPf4mMgGOqUBtUiQUuvX0GD2GvkfH%2F0dKruOoQ%2FXJEarCAoCRnUKx7xhBvPCcQvZAznr7Ti7LWzXG9C8pecuums0Rv2m5mT0Swf5QaP2m3hA4SNii%2FG1ByWNT65FKBJ3QdxWOJddXn%2Fo65tnmx8cfBud4X%2FeoYh%2BVK0vMOe1g31rlpNnFMy72Z64ZTFJfkVsxAOywQSaGZkN2sOP34j5pEMipT1x6jy87XnTyRS5&X-Amz-Signature=035810874d31195fcb655899b5d9cf4cd58d61c7884d8ac0b4dc43aa5c289fb5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

