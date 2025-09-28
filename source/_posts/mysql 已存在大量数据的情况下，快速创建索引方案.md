---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667UC6TVD%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDhOecmkRTJTjzzk6Q439dYst26XgnfXiGcx1t19oXbyQIhAIAWcZv0QaqlAYdCoBJAsXF%2BObxgl7rPavbhpKGqQSHRKogECMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwdLRoHIB9zz9lcQSwq3AMZr57y%2Fi2orLnSox8wmpPygDL0Ea1j5HEGnwRX3aQ0lImoiS23TfdMI6VFEj%2B3DJRZjWn5dtJzhamorrWLLROWOiWPQdAjAou6FrtUNuzX4jb0PnP02FBug%2BHR%2FeO0WWONwpa6gGfKDnDz5xjwKN7kAZvnJxVrKmDnNraV9olO9C%2FlEDpfrNDCFw8D8olvUy33UHSqXbdTYt9h1yU%2BNaEfT%2BXms3PuorTDZF4V6sBXcHXYqSB0B0LjgHGvXzv8fWsIcar59xT%2FhwGqiCbxEyLLtqDbNn0AZFvpaWi2FzuEF%2Fz%2FjUJlRhHt96c%2Fy0LEt77cxfMCFuhx5MkLY8lIQJXSiAZL1yvVen6lNIVrMb0FyzIgi4lt5cZuoB%2BMbKwpvDJVofl1L%2FW9L936DolrQZ%2Fp2sYdqcbeNfswqp5xEjxTUm90pVSpQG0PFSpxF5N9PXE7oS819qZyjbDaO8Ngn7GJtcuBEaqtd1RUjxNA37nAe7rhoq167pCJ6HJs3E48Z7GU%2BfPEwlI3%2B8Z5X3QG7etv97XGiNvOnp8FyyiIfWgdUm5Baaspl21jEMt7r3iFbSQFbeb0XRXS2o14J48KhH3O5X3hYhAHROLprRrKoTO0Ajjwbfwha4cI4Pc09zD%2B2ubGBjqkAWO3Y5mVinV3aaZPwMiB9E%2BNnwcDk15P4Mshs%2FdrEpjSvEr5Q7u%2FPgxHdkUHUnQLb0Imd5c%2B4uUtK3LSnexYssUY3K0p1ovEKB4BNSQ5AZgmPh0AZvJ9x6K7Sweyfmmttj3XnAjDHSIvTSDAcBwmZy50ICYBPQWvz6kAOW99MITOcSryJcg1HOLKLNVSgj5NToBpY7rX2dQfA4Tw9XVM75XFD%2FOV&X-Amz-Signature=d5372988f1166330aa9e74697de96c22819085d22adf4b1d1a6cd1a9cf8bd8ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

