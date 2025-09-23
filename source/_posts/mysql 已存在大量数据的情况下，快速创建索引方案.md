---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVRRE4X%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIALNNbwvCotw7sUyu1va1Xk23rCFMlgHx2Vj3M1o5tDvAiAhfJyQdR8ErzVHOm%2F3GlWbrfbW0dV49%2BcTJNlvrx%2B9jCr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMfl7%2FRED75odPinPcKtwDcJFwXIwj7cyDvHqf9GgYec0CsSgfVjDVdq2FeEnJ7vYPvZnZL8xXipEx2c7XDExEdL1FYYrxjKH7z7l12OaNLZRKFm7SM3zxr1%2FUhB%2Bt9AH4%2FNDWuAezJOaHBGxE4JgkCRwgsvrzTAEaCXTaB%2F0qpL8cepurXJqiY4o7zqDVqY6wb8WqI%2FkN8pKLB7J0zpgJQ22X5Lx%2Bd%2BNFhkIOS4nN6R7vhpKoYqI15oWZotaundnSMg2BrIHUDEKsbrGalDS7ePlPHfOlxsiYq%2ByQzWLbaPQQ%2FWe6T57O3hnsq1NZLLV1zr3q%2BG9ugtvtI2kmD4Nm3iJDVSYWhjPx%2Bs6O%2BTN8x%2F3sRXA6%2FxeIpHG%2BOp8AFGUEA4vI4RrgXygyXxib0SHx7YB%2FkSAYYwj6lilKPCv2nitISUsVHEaQViL4%2Flt%2BgnYXOXCkIUktAZAoMsu9u088jD4x45ciHmA9t%2FGug14jGpIXVCpEtPA54IGr5QWjLdIVeg5b6xkMmsLY%2BskxxtNKtG%2FmTi%2BCr6KujXX55y2ljgEU6cinvz6Rxl%2B1Su7JIiHaBq%2Bge06jKUdB7Jz1xjfgEtvSCxpK3QwNQ6ffx1r2nEW3BjqXy%2FRMFRIWb5WUctf%2BLyaCX3JngAQJBOgwroDMxgY6pgEhSArYwO1f8fsU5qgi46or5093vQ3OI2i5pt2bEQlwUdqeJyahSMSxx98AX0nE86dorge4NOem2m5l%2F03ne82Mkt93IYLQsmg6JabHtAcr7En8jNt9M4NLRwqUjCpeXszz35PnBr8b71uB0OqJO03WizHNBUPqjzLtlvBGC8M4FLLcIlNqKuMHe0TRqm5a0fT%2ByPWMkWLUjBW6z5Xh8yPoNv6AAgJe&X-Amz-Signature=427ea8fc16e98c46dbda1bb55bf255262c0605c35b40300575200cc965186398&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

