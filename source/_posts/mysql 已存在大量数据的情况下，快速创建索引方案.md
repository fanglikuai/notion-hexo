---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663M2SFTUR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCyy%2Fak7GVdkZrPBs%2FkNis46aY42M2MsUeip9n0ECOneQIhAIvKzKKLDNeEx8SMnFlPfxe%2FspKntt7eAFuxUkiVUMSaKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyZRDg9Bzd52g9xMAwq3AMH98JWxsX4nuo45LiLxo7FCgCCChl6GqBeoMKPUK%2F5tkJYJWn%2BbT0v3bSbBHXarEp0I2Zk7BEFWhlQ5DMGrlEmOBzeBUtdS6OE5sArdXuJIbXrnqoFfBCgqrVMYyth2MAf4bVCTWc1QMN8t1JaESDZQts3etGd5M%2BvAEwkzzMnTqBk0kt6UAnlnCZBa%2FREjeJ3ZvOJrAgh60etcAa%2FAktjNqapQ3j4c5HQtwcO1IOC5cLIVJ%2Bq9fTZAM1PP7h1TIkdT6n5F6SrzMmjNybDZ4hiSNxBS4F4ZvsCBWcFu3z5N%2BHFB1fR1tPrCWlXjwPSicc1FLBoH0vP2p8FbnNkcH8tSjr3U7r7V3xE6cE%2FVLHM3iwL3DgafI3uIVlYOi8WCJX5iESnepe2cewlJj1tMhPV%2BdOenJHijHHXZCED5JkuEItyx8VhXm6QpWTgEqynPGNQOZ2Gu2tH2ikb2jjs%2BxvnUVqeqX5e6GxtnWTH7Rn0MaTpOn9zcsU2KKVqHrokBBc65n6rk3AoBWyN4vQ0l4ijOoV5eZda6x473OiY5nb5oUFxKAS8W2FvCXRch7sIRBT0KWz0X8iuj5g8h%2F%2B%2BAfYIrU3qTjKrlu4ECTE%2Fo9QBc3ERng1RkuKYetICVDC%2Bh4jIBjqkAa38q%2BiPuqlimf6IFPnWt3BICXOb7MaN6e7M27Vsp0bxC08jHv3caNNZPi9SWwdCLKe2nHogXHPZdtsPuWf7AOYsPmVkMtPTFL%2Fep8Ue2p5Z4cV%2F4i%2F39DpOwlW%2B1NRdMYK6dQCYv0o8y3lgK75OsO19j%2BKLJyICs6O3aXS%2Fj0lXT7EHp54yQKFyzCtkUr4FBi4Q0WD4bVP7DCLqIQRIgdvjjEP%2F&X-Amz-Signature=fe6f96550622ce8914c41d05dfe6a6154a5ab7dd729cd9d6c94c212c2fbe07e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

