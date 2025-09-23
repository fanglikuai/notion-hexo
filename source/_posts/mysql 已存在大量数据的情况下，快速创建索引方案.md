---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTBBYACL%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnA%2BFx2QVU27oMUxqH69eSOYvV2qXLavRUaChMwhyvzAiEAz2WFyS9FJdAoUHbX%2F5CoEvOeyNCsWHMRo3j8sMMYwz4q%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDAPPhIMKPKu2olcYCircAwvzFxWiDV4JlHkQuEXjID80mGrBvFqFX4%2Bt1HX2JixHi5%2FhYehfRp%2B2oNvQ0ZSSE8PEcMg5hgfu9k2HsGEzqvpGYCTrud5WtjOMnHjseRxLTjXxsEx3EsP%2FBPw3MIR%2Frm3qgDJ9XitRv3Xdr3LJQ13aRKFSY1NsdKUEjOZztFMo98QpgAG2xZHo4OJ%2F60vmmxD8aMamwKyjsUZXYs6V0bThd1redrONObwbYJBofhXxzTMkqBKYOI%2BWr0uxiEJU1hB9cJvIcicpwayN1rvbSjkzXKMfvjcO80XfHfWXH1abyoJk7nKuHrMUPfjUb8IoovMlbeGmEAPsUjlqawqrCR%2FzCrC%2BjUwSRglciGm2%2BRC%2FhUuhOk94AlE1SH%2BXc5CfCcGSiK6Oo0rDTFk7QDqn9NuHCD%2BK8lCme0Ezg5VMANDUF9rJkAqx%2BTvuPI2uO2cYyTmeQk5WAvYdsTvCOV9D38pfsep3hswvAh4vzXr6B1sYxom22GRDbzlCQedMRve5o4omZ2oIwYov5vDkqrp%2FErrv46DQp4%2FPXbdZaiukViOtYcs%2Fd6B1nNm0uy3u5Ok%2F6KmkT8%2B2nMIjs3YwcGqnqEuXvT1pu1UH35LUAdmHK%2FbDdiqsPJVSbmeaA41PMLviy8YGOqUBWURPzzBKZiKdN4vILEzePSU6ZU9%2FjKe3Sro1po576NOSNqUU5IR4Boy7iiun7wu6J%2FP5upDszx5lUW4s5hTtx4TDokTst1mjUYFcEYKbo2kMsCx1cENd7WxBsVAMq8JWywpuXMa6TW%2BPgW8pZNS3tZpNjUz%2FkTfoaJf3zhMwzr1QfCt%2FnxaXrR05bLUc6PW0F49gKNx2DWvxmCIpq43fyNfsyePs&X-Amz-Signature=fbad5e344a4324a109da617f37ce23c83606bb3b61c5ce2c8843aa684582c33f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

