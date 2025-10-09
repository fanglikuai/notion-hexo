---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOHMJBTI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T010113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDtTEJPjtEfkfLTTvjDmxE7zrgArMcxur0U7jU4xoD%2BxgIhAIoGEF7e87gSrWzmaSbl%2FGJDPDxDScWZoglutRmHWyhNKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwlLtbu%2B1VQxMO0D08q3AOR4g1OJ1KyVN%2Fd6Dv%2BlwDixGLeRy37egNLvBi0Mo%2FqyJmlIvR9q5o8Epe2dN58OrcvbGRUlZmTeVNvyzyGMz6s7Kt8JYtOp2PbV2zJLAX0yamMGLaVNuGs2X3fa%2FUzFhUuJ%2BStCQ7GBN14Xr0ThC8cVfne7DJJxDJdc6w7LvDy3vZk8YcFUhKPSiUtp5qu8vbCojdMylejGOzk37A38jemG%2FsCepOil%2Ftm%2FjtZ45kXDfHpFuNClatQtk3k1NAGoDNJh6igfnufPu%2F93r1uh%2F8yqb6OyECMHxz5OmaZQg9zL2HcO2rZHWRzFxRlgOn6Rgot6Kk3Pn5XcSCM6hRnacFdZNsKokdwj3xIUVKBXZCOykJ51QJILcx8naFyzFRFt%2F%2F00lZ0hZJst%2FBW%2FriOC2SJoMJV9HWUNsk72DnjQeOswKTzwaykgBgQDCDBGeg0y5EJ1AG%2BENTxVn43NtEdzAOgpjUbMqKTbftXAn%2BEuU%2FKdtNww46Qm%2Fo4vD18GL2geCu0Lp6A4hYqxFObwLcQIKAf4XqFyQ1JBteNpQwV12ZrCIu6HUqweeWeevhTS98cYP3typDTq8gRLT5vrWtNsTakWHmMGMut0EQwYc6SqFsCO%2FOrl%2FU3i7gC7mem9DDMhpzHBjqkAa%2FR%2F19%2FKInWoXERzJ5mAkk9EDJR7tsnbtgMQOKTX4y%2FSqAEUz3gG%2FE%2BiZkn2663OleUdcNdnHV6C0%2BJ0MFIK%2B8xaNS35faBhaR8H%2FOvNPLqJRLAffZT6eDzPpgbh0uGzh1llxfB9phJpFt2FZydhijD2YbMXGVJHX5FPc%2BrvXzQYHASjlZOXXJWSm0Rz5vGPHMo31%2BGkdtpMGWsumUYlWxiOKd2&X-Amz-Signature=3abd651e16103afa24ca5c7e12106ce948e96d88982bd0912d65ba8664f79ede&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

