---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLWZC5KD%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCMWhNqupK9Rc3Uoh80BV%2F6FPs2uDHGC66jyG%2F8hmKDsQIhAMChhBB5Ev%2Fi1WtYWxXF%2BmvPuLXH1u5hXUVVTDMe7E8yKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyexT7TPfuWM2%2F5lRsq3AP%2BGdwLIzUPDhVnDiZ9rkR0bKfsFz29CXc7Qc92VA%2BeDo5x2V5f83x0JLAoDh1OBnN6TSbxJkLh6eeZa1Ajr1UHrZInHlPWPHDmHs4IXJPIpIjy7Plj3nbEd9KXJGOTlBOWDCSuIZyYT%2F5VY%2FSvKEn1NRGUiU17PieN%2FjamQLD0MfFbkf5eHzaQvJnMaHQvlr7qe5yvix5Hd%2B8jr%2B9qv5t1tQjIkEtH%2F1dTFmlJk%2B7EC4shz4dlDagjURkTdZc4xodzep5uTmujouCT%2FIjy8CbghKLvZTFhvcIUaJkXcHD8cUnc64PXSXojeLSWNzRc4RezT1M9nJvEcXJTMd20tK3%2B30ix3NIG6Fo8fkZa1GsU3mUWsR5YBgPTtCb37nePDjB4rOE6rte7%2FqMOIJUBbx%2B4CgoLlDtNZPpfVjyNN7WDVyWji058NxEDMVMBRaY1V9S706u%2BWHqpKV2fC77q1Pbwy27m%2FZgtEmieH28%2Bf6a7j3%2F2Kpb232TPM6rnSktulsaBKtJQrv%2BzrnGpxYT60Xmp6nWCxeg%2FcJatNuzVEHN23f5nCkyMuGz1xwqG4jw0XVaE8VagWMUiCctqgaRkIO%2ForYtNujCv5QUhFC0ih7b4wBrZkZEADz8h9LlerjCb2PjHBjqkAVRxf0z5BZSQmf8MDq8OUvbDpN%2FXmXO%2B7ugE6w0scgvyfWexU514cXFqlRkcw1d3LubEOad8rCApmmjHZWK5lNKU1hQnyLwyH%2FvgY5aNSZtrT7exF4KBZVZ2wmFUh6xO9j9HlavUv5C32wm0Sf%2Ft%2BeWRluG6ilIHY6ZOR0YRK7IUgVejIvSr5oXh1DVleY8aMf4QZuj045CXB1nbFEJkhcsOWRC5&X-Amz-Signature=84eb6594949d8b5a4038c3abf0da3fb4aec06314f9bd8217f0be7a0380f5d4f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

