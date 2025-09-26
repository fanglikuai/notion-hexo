---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A7VJP5I%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCN3QuZ4SEx5DkjGWMW5Nmc%2FW3yEqQRJQCwS%2FmSCRngGgIhAJ%2B93WnGxD%2BoIDxI0OdAnwiqlhyYM6YS31ilDtBqpIwKKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx6UzHNovvTzsWNB%2FEq3AOt6Ro9tdYvceU02oO1wBNK59kF7JjQN49obBA83ZI4WgrIiKQoVBUMm70Vi86uULpYhe5O%2FG98M8mZFH1xptDL%2F2SmYvxDxBzMixQ2Irl3036CIwzVMK3LicW5H9iTj4GvNi3EM6nTylikPwpEefjm1qMxtbY%2FwPx5W8NusUuObwioXckgWhoNteWhtFA5P4PvSrhYa%2F3yYaIVQDq5J3i%2BFDFs5z3YSG7j%2By5nIg5CsJsAccFytCXNGT2fTkyR8BRau0q7aryOTeocEs7Cebf0PJ2OJ6ttdaeB9I1YyaPun3RmaWEaLGLajDlnyGxyA14c9qKaFaRCxWeCiq55chRxRgysKl7Y2yS%2F5KdMHeZTuv7xqcFWzUg20rT%2F2FJPQ04P7n8xR9ylvL4Y36mri1H7ZINoHGCzpiBps%2BVf8ijWpA0%2FlC%2Bmp9OkN11uZ7P8lnREgYPP22E7r8HgBKAsDUQYW4ZPF82o16yo8qwmBj3Hz5GbPzQ0ZWDToW5EtZdX6plRZFW%2FxcAbGFjoHbUb%2BIDNGcPgcw%2B2xKRgtMKXxz0CIEb2mPS%2F8LlEtuJiNYP1Ecr2L2%2BdRwqrwzCX4RFeMtOXBAyh4cc7bO9yoFDZOL42HqjpHSrRkcfgnKWfvTC5u9jGBjqkATXrId9M4SHAX53%2F0VRTAnWrDfpzlyyw5x2GKW9pfbh4MK%2BEoogkjdajKSqphsiWDBzFUiJS5Kcc7mkF00P%2FvTcx8hTW4D2SkfWJYpDOwP76gNSJbGwDPXjM3DNtpqgT03OQh0QRb%2FyKrn%2BNxIlY2NjgMaw2C4LoEHe8XXP0blKrkzbaP4v%2BFfOnKVPGFmp3E0cDmmb35KcAljK26oPx4q6sW3vy&X-Amz-Signature=9e7cab41f63edb41f4cc073badcf6ef2eea8902f4dfe201a54bbeb45ebbccfda&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

