---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EFDRXO3%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDtMFU1uq3mQRfXLWtVXM6zrGyq%2F4zpiFGv6SHxPKzUTwIhAPZsNHY0YDxmQ4tocSjbeDoVPXWMCIO5Wtpa7HSKiR00KogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYPiJjLCUqbGbgukwq3ANqwoA3Jgt1mHPQsNVicAiPUrhNWsgnARejOPTmVCphVbHiBDx3pAx%2FpN4aXrzU7yTQBiDa0n5vtVl8PhYYgiz%2B4ykN%2Bbq3iPeWOWBpjQ0jqdT6GhL0%2Fv%2BMigpuCzsxktgYTMqypBeM8YU%2Fxru8YHw0o6cPjLUWtuFTOsI5Srbp9PFg3RXmxDWHsOYz4yZrFbtqBC20ammaUdWleiX3x%2FfrY0vP%2BZxR8h%2FiNd1%2F2nAyiCpGxvXMVNFoSVVhXJOc86nAT%2FUs%2FGOy75ENtQ%2F4fG%2FcgHEcbEKotrus%2FigudpK4p6Tr%2FAarZxBemUnzryfUNMYuFdcyZmC%2BTXocP9GCpxe7BscwB8E4Akbsv2we2%2BS8PU6NlO%2B8UkXoEfxR%2Bn0sQTltM%2FnwWjgZm9wjOCPQP6VxlIgp5g8eMnRJNNRuBOa1yF%2FA4N%2Fld8FJvhhzaLdFUpRYQvArxENv8zGMTjY6XtfJN2MNKMwGOH8EpQwtNXDaMqmy%2FaeBGq5oFO8e5x1Z2PGnnwZNOxO%2BbjG3J4g%2BR6pgxwxxFcQPHev7X%2BlXuV1PMiixh%2B%2BYNAvnQHYYcnhw0aPpx6Bx2XLTyhY4AESxVwJUt7Jpjfzt8TyhzedArbuBccX17pzE0oMdm5T7cjCgua7GBjqkARg6iXyDskH49vFYDHa%2B9Qc9N98NZlqjqwgp%2Fr%2BinQsWQt3QyECPNJV3MsQHFFwakzk4izrd7djBE8spe%2BiX%2FDgCn8fVLQUONWh%2BwRLM8okuFKPDNzhzrzkarFpVGSnJw1l%2BfdYZa2O0ehYSwB%2F9aOs%2BLwzdwUbrdQDT5Ee%2F89xstwrSAt79F6aty2cHAWgQ1ExI%2F3pPtP2zzzK0KNZc4C3SwaLf&X-Amz-Signature=317bdf4699dfe2ea1007dc66239f46824a71d69371e5be441b4bec9533e1958c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

