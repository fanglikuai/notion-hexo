---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VD3FWWOH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGrb%2B5gEtNQsU2TYI7JTsXDq6wLe43QAvE%2Fi1qwIoaq2AiEAluoHwtknOcCgCwuH7o4RqEPqll5nVvgtgAOmnJV0EuMq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLMQS1U3%2BHBWPbh6vyrcAyeOYVjVlCnGiVndHrPcE84TYtQtwHzdqCDs4XrC3tnUbW8advh4WAtNUbKEp4b09zn1qeq1m3vcAjwhE8d1pIrRxz1UHVRWDLLAG0yuWNatlEn5GY8NrdywZsLNYnZRmdNxotYrScNnJhtG5nUb%2FSTC7aT%2BiEDiZNoYzf5IEsYHr1NyytMlMjwUknxC0%2FpnKxNyGXE7CWV7pmeNyLRjVvU2hEZJz6TIpU8Jo7UlhTm789HBnEGvZWVZJOID6dy0tqveyPW%2FmIbjhTRNRx%2Fx2vb5TSVQ4IJy6Ft6k4mdVDkfPh921J%2FmT5dx10Ei9y8ozE3Fdlysuu6Sh%2Bjt8TAuE5jF9JgcnECtYujPrSlGAl9IjfYxpC54SM8xc7Ju6jon58M4%2BsOd2Ry%2BOvxFBk0MKYEBSu3HK8nhXaFTFJqQYb1UhO9u3jHMmviyuaAuoAQt7N41R8guZhQ44v313lpf%2FC1iTxijOl6as4%2BZCwxm6ID43UftLIFomLsOgAnqxGV5jlxjzyO%2BjvbWFlFZCJ5z5kA7SkO34kEnSW%2BnyT91EURsUVNCIDjvOFSi4CnPmpLFmG48c0AOouow4xkHeWzw1tlOuDBlgDVB62ZSVM58jFcJpbbEKseI0Q7XaoAnMLvy3sgGOqUBLeQBFPRGQ4cZFZP3T%2BrDJx6iCYiHA%2FigJDwORsTc2iIkoPzTT1h3iQoaoCdX82nxQ5z6l%2F78d5fvpAt2I%2Bc2n%2FFCA3yldKF2JaO16iYzWCBrickkQYAFVitqIfzAmnhfdua2m51jqGD5TdPUd5k1zISOJ4XC16ILQTaA0vmoVm5aBB9NO7pFXno6btPbo6ZRJKP8F%2Bfk%2BKKXb%2FC1SjKOoEtbE%2FaA&X-Amz-Signature=8cd1ed145090284ade41032a3c41b9a836296be909c86ffa97bd1784501e8942&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

