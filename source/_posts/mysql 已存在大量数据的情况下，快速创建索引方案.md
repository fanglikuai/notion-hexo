---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PKMEBNL%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCIAHINidy08HsrH6adFgVy3NoKcj4bWYwZzkveObrMflzAiAsPlm4b0I4R7MC2POmnZyYQDBbSqq2zkhJO7NjINEFISqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMox1LfDFcyEWStdLLKtwD%2F5T976hi9ySlwwtQoGcCr6zIOQ2yFvtzYBeObgrXgTTrfMO24YsYdpIcUPzE%2BpDNkzYiZpWEogDiDFH20fa1ry5G2TGG%2F6v4hxnQ%2Bxdz%2B%2BCwt0baNgKtMLPnaS8QhyX%2BZJcc0ukXuGsLXksJFZ%2FzLB8yPFz47SjvDw%2Fo9LhsefA2J7N4Z62HJ49jY3Cvu21kU5cGOVVR%2BcvWIZ1LqfnvrcEJsadBXBp8TnXKkwHDSooqn%2BGJeifUc%2BaUQkDvSu2nKN0WLjGI14xDR8XfQjD39bxQf%2Bg0oohiUOP0IDI9e6Blt4VIvSw%2BZK471%2BFyoxGs4VzZB63mOqg%2BET7YkReuNPRVjb7VorFx7LGVY2utfX9e0ek0c8uK3cdzPCBAHzSWNrSp1yyx4oo9%2FhM2pMUZj7vgoIx%2Fn6UvrjmNYhNmXPSHuon9oP%2BPpko5DwLPPHz0eeEB%2BycKlut%2F%2B1D8W9MeR6jZ3JgGyoslrnasmdAQnQ2iGtkTNy%2B21TxsKHrI4rPt30j%2BxeRpnVYRdCpmIoYgnnJ%2F4%2BdG3iR%2BqDJNHaSuPFT%2FbiPiUsixd2IPf8l%2F5xrBNEnpLJbgZjfT3qp81SBk0Amale07gfuB5i5DMcYWri2sCjtTGm2y3QLBYFMw5KPzyAY6pgGG0t9QJBTEoIihjtrcvpqQY4maYN708MdjrfWc3xP%2FV3OD%2Fared9VdPNfBM7UYOazfDkUJyc%2B7fieq5TYE76bsV1zWn%2Bkhpzak1Yy8G3VGwPTgh8JcJalsKE%2BYNLFTFRae48rKuIWdmPFz9W2L8O59TMwC7bRwbuLRdG3jPsLKk1eWIdktRVmqlYr3AzTqyVBLAUEFbFk2LCJJcax3uYm%2FRe%2BoyOMw&X-Amz-Signature=ab9a186ed88a1facc61c2e5e847f9d875bedbfaa57f5ffe8aaa292ac3539391b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

